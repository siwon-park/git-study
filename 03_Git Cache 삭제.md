# 03_Git Cache 삭제

> .gitignore 변경 후 commit 했는데, ignore하고자 하는 파일/디렉토리가 그대로 반영되는 경우

실수로 `.gitignore`에 등록해서 레포지토리에 올라가지 말았어야 하는 파일인데 올라간 경우가 있을 수도 있다. 그런데 이 때, `.gitignore`에 등록했음에도 불구하고 레포지토리에는 그대로 올라가 있는 것을 볼 수 있는데, 그 이유는 git의 cache 때문이다.

이를 해결하기 위한 방법은 git cache를 삭제한 다음에 원격 저장소에 올라가 있는 파일을 삭제하고 다시 commit, push하는 것이다.

## 1. Git cache 삭제

### 1) Git 파일 삭제

우선 git에 올라간 파일을 명령어를 통해 삭제한다.

```bash
# 원격 저장소에 있는 파일만 삭제하고, 로컬 저장소에 있는 원본 파일은 삭제하지 않음
$ git rm --cached [파일명]

# 하위 폴더까지 캐시에서 삭제
$ git rm -rf --cached [폴더명]
```

<br>

### 2) Git Cache 삭제

git cache를 삭제해줘야만 `.gitignore`가 제대로 적용되어, 이전에 잘못 commit되어 올라간 파일들이 더 이상 stage에 표시되지 않는다.

```bash
# git 캐시 삭제(. 명령어로 모두 삭제)
$ git rm -r --cached .

# 캐시 삭제 후 다시 커밋 및 푸시
$ git add .
$ git commit -m "commit message"
$ git push origin master
```

- 예시

```bash
$git rm -r --cached .
$git add .
$git commit -m "fixed untracked files"
```
