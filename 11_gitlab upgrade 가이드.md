# 11_Gitlab Upgrade 가이드

> docker-compose 기반 GitLab omnibus 업그레이드 가이드

<br>

## 1. 사전 확인

### 1) 현재 버전, 업그레이드 패스 확인

※ gitlab의 경우 업그레이드가 한 번에 불가능한 경우도 있음. upgrade path를 확인하여 여러 버전을 거쳐 업그레이드를 진행해야하므로 반드시 현재 사용중인 버전과 목표 업그레이드 버전을 확인할 것 

#### (1) 현재 사용 버전 확인

- gitlab UI 상에서 admin 페이지 접근하여 현재 사용 중인 버전 확인 (마이너 버전까지 확인 필수)
- 또는 gitlab 장비에서 `docker ps | grep gitlab` 명령어를 통해 확인

#### (2) 업그레이드 패스 (업그레이드 버전) 확인

https://gitlab-com.gitlab.io/support/toolbox/upgrade-path/

해당 홈페이지 내에서 현재 버전(current) 버전 입력 후 target 버전 선택.

사용중인 Edition, OS 등을 체크하여 GO! 버튼 클릭 후 upgrade path 확인.

나오는 버전이 거쳐야할 업그레이드 버전임.

### 2) 볼륨 마운트 확인

```bash
cat docker-compose.yml | grep -A5 "image:\|volumes:"
```

### 3) 데이터 저장 경로 확인

```bash
# docker inspect <컨테이너명> | grep -A10 "Mounts"
docker inspect gitlab | grep -A10 "Mounts"
```

### 4) 로컬 이미지 확인

※ ee는 엔터프라이즈 에디션(enterprise edition), ce는 커뮤니티 에디션(community edition)을 의미

```bash
docker images | grep gitlab
```

<br>

## 2. 백업

### 1) 운영 데이터 백업

운영 데이터, DB 데이터, CI/CD 정보 등 백업 (필수)

```bash
# docker exec -t <컨테이너명> gitlab-backup create
docker exec -t gitlab gitlab-backup create
```

백업 성공 시, 현재 날짜의 tar 압축 파일 생성됨.

```bash
# docker exec -t <컨테이너명> ls -la /var/opt/gitlab/backups/
docker exec -t gitlab ls -la /var/opt/gitlab/backups/
```

데이터 백업 경로 (볼륨 마운트 경로)

- /data/gitlab/backups/ 하위 .tar 파일

### 2) 설정 데이터 백업

깃랩 설정 정보, secret 등을 백업

```bash
# docker exec -t <컨테이너명> gitlab-ctl backup-etc
docker exec -t gitlab gitlab-ctl backup-etc
```

```bash
# docker exec -t <컨테이너명> ls -la /etc/gitlab/config_backup/
docker exec -t gitlab ls -la /etc/gitlab/config_backup/
```

<br>

## 3. 업그레이드

하기 과정을 반복하여 버전 업을 통해 목표 버전으로 업그레이드 한다.

### 1) 이미지 버전 변경

#### (1) docker-compose.yml 내 버전 변경

- https://hub.docker.com/r/gitlab/gitlab-ce/tags 참고하여 버전별 이미지 태그 확인

```bash
vim docker-compose.yml
```

```docker
# docker-compose.yml 내 이미지의 버전 변경
image: 'gitlab/gitlab-ee:18.2.4-ee.0' → 'gitlab/gitlab-ee:18.2.8-ee.0'
```

### 2) 새 이미지 다운로드

```bash
docker compose pull
```

### 3) 기존 컨테이너 중지 후 새 이미지 재시작 

```bash
docker compose up -d
```

※ compose down을 하지 말 것. 실수로 down -v를 할 경우 볼륨까지 다 날아가고, 시간도 오래 걸릴 뿐더러 up -d만으로 변경된 서비스만 재시작 가능하기 때문.

### 4) 업그레이드 진행 확인

```bash
docker compose logs -f
```

- 로그 중 gitlab Reconfigured! 메세지가 뜨면 성공 (단, 서비스가 완전히 시작되기까지는 시간이 더 걸릴 수도 있음)

### 5) 백그라운드 마이그레이션 확인

```bash
# docker exec -t <컨테이너명> gitlab-rake db:migrate:status | grep -i down
docker exec -t gitlab gitlab-rake db:migrate:status | grep -i down
```

만약 한 줄이라도 출력된다면, 현재 마이그레이션 작업이 진행 중이기 때문에 계속해서 대기하고, 아무것도 출력하지 않을 경우 다음 단계 계속 진행

<br>

## 4. 헬스 체크 / 버전 확인

### 1) gitlab-ctl status

```bash
# docker exec -t <컨테이너명> gitlab-ctl status
docker exec -t gitlab gitlab-ctl status
```

모든 프로세스가 run 중이라면 정상

### 2) compose logs

```bash
docker compose logs gitlab | grep "Reconfigured"
```

grep 결과가 나온다면 정상

### 3) docker ps

```bash
docker ps | grep gitlab
```

(healthy) 상태일 경우 정상

### 4) 버전 확인

```bash
docker ps | grep gitlab   # 이미지 태그로 1차 확인

# docker exec -t <컨테이너명> cat /opt/gitlab/version-manifest.txt | head -1
docker exec -t gitlab cat /opt/gitlab/version-manifest.txt | head -1   # 실제 내부 버전으로 재확인
```

<br>

## 5. 불필요한 리소스 정리

모든 과정 및 데이터, 동작에 이상이 없을 경우에만 해당 과정을 진행한다.

삭제할 리소스를 선택하여 적절한 명령어를 수행하도록 한다.

### 1) 전체 불필요 리소스 삭제 (※ 볼륨은 제외)

```bash
docker system prune # 태그 없는 이미지만 삭제
docker system prune -a # 사용하지 않는 이미지, 컨테이너, 네트워크, 빌드 캐시 삭제
```

docker system prune만 해도 되지만, 이렇게 하게 될 경우 gitlab 이미지는 보통 tag가 붙어 있어서 사용하지 않는 구버전 이미지가 남아있을 수도 있기 때문에 -a 옵션을 줘서 불필요한 리소스를 전부 다 삭제해주는게 나을 수도 있다.

### 2) 사용하지 않는 이미지만 삭제

```bash
docker image prune # 태그 없는 이미지만 삭제
docker image prune -a # 사용하지 않는 모든 이미지 삭제
```

### 3) 중지 컨테이너 삭제

```bash
docker container prune
```

### 4) 빌드 캐시 삭제

```bash
docker builder prune
```