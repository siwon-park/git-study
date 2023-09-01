![Git로고](https://git-scm.com/images/logo@2x.png)

# 01_Git

> 분산 버전관리 소프트웨어

### git

#### git이란?

- (분산) 버전 관리 프로그램
  - 버전: 컴퓨터 S/W의 특정 상태
  - 관리: 사무, 시설이나 물건의 유지, 개량
  - 프로그램: 컴퓨터에서 특정 작업을 수행하는 일련의 명령어 모음
- 버전 관리란 컴퓨터 S/W의 특정 상태를 관리하는 것을 말함
- 효율적으로 변경 내용을 기록/관리하기 위해서 맨 마지막 파일과 이전 변경 사항들만 남김
- 분산 버전 관리 형태로 원격(Remote) 저장소와 로컬(Local) 저장소로 이루어져있음
- 원격 저장소에 문제가 생겨도 로컬 저장소가 정상 작동하기 때문에 리스크가 적음
- 반면 중앙 집중형 버전 관리는 중앙 서버에 문제가 생길 시, 전체가 일을 할 수 없는 상황 발생할 리스크 존재

#### Why git?

- 협업 상황에서 백업 및 복구를 위해 버전 관리는 필수!

#### 구성

- 실제 폴더
- 로컬 저장소(git)
  - Working Directory
  - Staging Area
  - Commits
- Git Hub    

#### git 명령어

1. `$ git init`: 깃 시작
   - 실제 폴더에서 로컬 저장소(git)의 Working Directory로 파일을 이동시킨다고 보면 됨
   - 현재 실행 디렉토리에 `.git`폴더를 생성하고 CLI명령줄 끝에 `(master)`태그가 붙음

2. `$ rm -r .git`: 깃 삭제

3. `$ git status`: Working Directory와 Staging Area에 있는 파일 정보를 보여줌
   - New, Modified 등
   - VS code 디렉토리에서 파일명 끝에 `U`는 Untracked File을 의미함

4. `$ git add 파일명`: Working Directory에서 Staging Area로 옮기는 개념
   - VS code 디렉토리에서 파일명 끝에 `A`는 Added File을 의미함
   - `A`상태인 파일을 수정(Modify)하면 `M`으로 바뀜
   - `M`상태인 파일을 `git add 파일명`해줘야함

5. `$ git add .`: 해당 디렉토리에 있는 모든 파일에 대해 git add
6. `$ git commit`: Staging Area에 등록된 모든 File을 Commit
   - 반드시!! `-m+'텍스트 메세지'`를 써줘야 함
   - `$ git commit -m '첫번째 commit입니다.'`
   - 처음 commit 시 오류가 발생하는데 이를 해결하기 위해선 config 등록을 해야함(2개 다 해줘야함)
     - `$ git config --global user.name 'siwon'`
     - `$ git config --global user.email 'abcd123@naver.com`

![image](https://user-images.githubusercontent.com/93081720/157832590-1bc05a0a-2e0b-4d41-96b5-69247410c254.png)

### Git 기초

git은 분산 버전 관리 시스템이다. 따라서 dropbox나 구글 드라이브와 같이 파일을 저장해서 다운로드하는 개념의 저장소가 아니라, 버전이 기록된 곳임. 따라서 특정 파일만 선택해서 push 또는 pull 할 수 없다.

> <span style="color:Red">주의!</span>
>
> <span style="color:Red">1. 원격 저장소에서 직접 수정 절대 금지!</span>
>
> <span style="color:Red">2. 모든 수정/변경/삭제 등은 반드시 로컬에서만 작업할 것!</span>
>
> <span style="color:Red">3. push 하기 전에 pull 하는 습관을 들일 것!</span>



#### git 명령어

``` bash
$ git <명령어> <인자> <옵션>
```



#### 사용자 정보 설정

```bash
$ git config --global user.name "<사용자 이름>"
$ git config --global user.email "<이메일 주소>"
```

- 커밋을 할 때, 사용자 기본 정보를 제공하기 위함

- 해당 정보는`~/.gitconfig` 파일에 저장 됨
- 수정하고 싶을 땐 똑같이 변경 내용을 쓰면 됨
- `.gitconfig`의 내용을 출력함

```bash
$ git config --global --list
```



#### 로컬 저장소

- 작업 공간(Working Directory / Working Tree): 사용자가 실제 일반적으로 작업하는 공간
- 스테이지(Staging Area): 커밋을 할/커밋 전 폴더/파일을 등록하는 중간 공간 
- 저장소(Commits): Staging Area에 있는 파일들의 변경사항들이 저장되는 공간



#### git 초기화

```bash
$ git init
```

- 현재 디렉토리를 git으로 관리하겠다는 의미
- `.git`이라는 폴더가 생성되며, 터미널에 `(master)`라는 것이 끝에 생김

> !! 주의 사항
>
> - git을 시작하면 해당 하위 폴더에서도 git이 적용된 상태이므로 추가적으로 init할 필요가 없음
> - 홈 디렉토리(`~`)에서 실행하지 않는다



#### git status

- working directory와 staging area에 있는 파일의 현재 상태를 알려주는 명령어
- 어떤 작업을 하는 동안에 수시로 현재 status를 확인하는 것이 좋음(습관!)
- 폴더의 경우 빈 폴더일 경우 git status를 해도 untracked된 상태가 안 나올 수도 있음
- 파일의 상태
  - Untracked(`U`): git이 관리하지 않는 파일들(한번도 staging area에 등록되지 않음)
  - Tracked: git이 관리하는 파일



#### git add

- working directory의 파일을 staging area에 등록
- 등록된 파일을 git이 추적 관리

```bash
$ git add <파일명/폴더명>
$ git add a.txt
$ git add my_folder/
$ git add my_folder/a.txt

# 모든 파일을 등록
$ git add . # .이 현재 git이 관리하는 최상 디렉토리이므로 git이 관리하는 모든 파일을 등록
```



#### git commit

- staging area에 등록된 파일의 변경 사항을 하나의 버전(커밋)으로 저장하는 명령어
- 반드시 `커밋 메세지`를 작성해줘야함
  - 변경사항을 잘 표현할 수 있도록 의미있게 작성할 것
- 최초 커밋 시에는 (root-commit)이 출력된다

```bash
$ git commit -m "<커밋 메세지>"

$ git commit # 커밋 메세지 없이 $ git commit만 하면 커밋 메세지 작성을 위해 vim 에디터가 열림
```



#### git log

- 현재까지 커밋들의 정보를 표시해줌

```bash
$ git log

# 1줄 요약
$ git log --oneline
```

- 정보
  - 커밋 해시 `commit 3045597aa45bb.......`가 노란색 글씨로 나옴
  - 해시: 일정한 길이로 16진수로 표현, 작성된 암호
  - Author, Date



#### git 조회

```bash
$ git log
```



#### git 되돌리기

```bash
$ git restore <파일명>
$ git restore --staged <파일명>
```

- working directory에선 수정했고, staging area에서는 수정된 것이 등록이 안 된 `M`상태 일 때, `$ git restore <파일명>`을 하여 이전 버전으로 되돌릴 수 있음



#### git 도움말

```bash
$ git --help
$ git --help commit
```

- 로컬 저장소에서 원격 저장소로 Push를 해야 비로소 개발자로서의 '저장'이 끝난 것임



- 원격 저장소에서 직접 작업하지 말 것! 혼자 하는 게 아닌 이상
- 로컬에서 모든 작업을 하고 푸시하는 형태로 작업하는 습관을 들일 것

#### 원격 git 저장소 등록

```bash
$ git remote add origin https://github.com/siwon-park/TIL.git
```



#### 원격 저장소 정보 조회

```bash
$ git remote -v
```



#### 원격 저장소 연결 삭제

```bash
$ git remote rm origin
```



#### git push

- push하기 전에 git commit -m 할 것

```bash
# origin이라는 이름의 원격 저장소의 main 또는 master 브랜치에 push
$ git push origin main 또는 master

# -u 옵션을 사용한 후에는 저장소 이름(origin), 브랜치 이름(master)를 생략 가능함
$ git push -u origin master
$ git push
```

-----

#### .gitignore

>  특정 파일 혹은 폴더에 대해서 git이 버전 관리를 하지 않도록 설정하는 용도로 사용

##### .gitignore에 작성하는 내용들

- 민감한 개인정보가 담긴 파일, 전화번호, 비밀번호, API Key 등

- 운영체제에서 사용되는 파일들

- IDE(통합 개발 환경) 혹은 Text 에디터 등에서 활용하는 파일

  - pycharm -> .idea폴더

- 개발 언어/프레임 워크에서 사용되는 파일

  - python 가상환경 파일 등

    

##### 주의 사항

- 반드시 파일 이름을 `.gitignore`로 작성

- `.gitignore`의 위치는 `.git`과 동일한 폴더에 존재해야함

- 제외하고 싶은 파일들을 staging area에 `git add`하기 전에 `.gitignore`에 작성해야함

  - 기본적으로 Repository를 하나 생성하면 `README.md`와 `.gitignore` 2개를 만듦

    

##### .gitignore 쉽게 작성하기

- 구글에 gitignore 검색하여 gitignore.io를 이용
  - [gitignore.io](https://www.toptal.com/developers/gitignore)
  - 사용하는 언어, IDE, 프레임 워크, 운영체제 등의 정보를 입력하여 사용



##### gitignore help

```bash
$ git --help gitignore
```



##### .gitignore 작성

```
# a.txt 제외
a.txt

# 디렉토리는 /를 붙여서 씀
subdir/

# 패턴 사용(*: astericsc)
*.txt

# 특정 파일을 무시하지 않고 싶을 경우
!a.txt

# **: 디렉토리 내부의 디렉토리 지정
subdir/**/*.txt
# a/x, a/b/x, a/b/c/x 하위 디렉토리가 몇단계있든 x는 무시하겠다
```

-----

#### 원격 저장소 가져오기

- 원격 저장소 → 로컬 저장소



##### git clone

- 원격 저장소의 커밋 내역을 모두 가져와서, 로컬 저장소에 생성
- '이미 업로드된 파일이 있는' 원격 저장소에서 로컬로 최초로 가져올 때 사용, 이후에는 pull 사용
  - 새롭게 만든 빈 원격 저장소는 clone할 필요 없이 remote 등록 후, add-commit-push하면 됨
``` bash
$ git clone <원격 저장소 주소>
$ git clone <원격 저장소 주소> <폴더 이름>

# 폴더를 따로 만들지 말고 현재 디렉토리에 클론 생성. 현재 디렉토리가 완전히 빈 새폴더일 경우에만 가능
$ git clone <원격 저장소 주소> .
```

- git clone을 하게 되면 `git init`과 `git remote add`가 이미 수행된 상태임
- clone 후 `add → commit → push`하며, 한번 클론한 이후엔 `pull → add → commit → push`임



##### git fork

- 다른 사람의 저장소를 내 git의 저장소로 그대로 복제. 원격 → 원격의 개념
  git clone은 원격 → 로컬 복제



##### git pull

- 원격 저장소의 변경 사항을 가져와서 로컬 저장소에 반영(업데이트)

``` bash
$ git pull origin main 또는 master # origin<저장소 이름> main 또는 master<브랜치 이름>
```

> TIL-HOME에서 PULL이 아니라 COMMIT을 먼저 한 후 PULL을 하면?
>
> 1. 강의장과 집에서 서로 다른 파일을 수정한 경우는 정상적으로 PULL 진행
> 2. 강의장과 집에서 같은 파일의 다른 라인을 수정한 경우는 정상적으로 PULL 진행
> 3. 강의장과 집에서 같은 파일의 같은 라인을 수정한 경우는 충돌(conflict) 발생(master|MERGING)
>    - 내가 직접 수정하거나, VS code에서 선택해주는 것을 고르든가
>    - 수정한 것을 `add`한 후 `commit`한다

 

> TIL-HOME에서 PULL이 아니라 commit을 먼저한 후 바로 push하면 어떻게 될까?
>
> - ! [rejected]        master -> master (fetch first)
>   error: failed to push some refs to 'https://github.com/siwon-park/TIL_remote.git'
> - 먼저, Pull을 한 후에 Push한다.



- 원격 저장소의 변경 사항을 강제로 가져오기

```bash
$ git fetch --all
$ git reset --hard origin/main 또는 master
$ git pull origin main 또는 master
```

-----

### Branch

git이 관리하는 영역 한정으로 독립적으로 작업할 수 있도록 도와줌(like 가상환경)
※ git이 관리하는 영역이라 함은 `git add .`을 통해서 `staging area`로 올라간 상태를 말함

- 장점: 가볍고 빠르며, 하나의 브랜치라는 독립적인 공간에서 작업을 할 수 있음

#### 명령어

- `$ git branch` :브랜치 목록 출력
- `$ git branch <브랜치 이름>` : 새로운 브랜치 생성
  - 브랜치는 생성한 시점(위치)의 버전으로 생성 됨
  - 브랜치를 생성한다는 것은 가지치기를 한다는 것임. 해당 브랜치에서 커밋 후 계속 
- `$ git switch <브랜치 이름>` : 브랜치 변경(이동)
- `$ git switch -c <브랜치 이름>`: 새로운 브랜치 생성과 동시에 해당 브랜치로 이동
- `$ git merge <브랜치 이름>` :브랜치 병합
  - 브랜치 병합은 병합의 주체, 기준이 되는 브랜치로 이동해서 병합을 수행해야 함
  - 충돌 발생 시, 우리가 직접 조정해줘야함
  - 브랜치 병합 이후엔 피병합 브랜치는 삭제해주는 것이 좋음
  - 브랜치 방식
    - Fast Forward: 굳이 다른 버전을 만들 필요 없이 마스터 브랜치에서 앞으로 나갔을 때, 그게 최신 버전일 경우. 브랜치 생성 이후 마스터 브랜치에서 아무 작업이 없었을 때, 이 방식으로 병합
    - 3 - Way Merge(Merge Commit): 일반적인 병합 방식
    - Merge Conflict: 병합하는 두 브랜치에서 같은 파일의 같은 부븐을 수정하고 나서 병합하여고 하면 git은 해당 부분을 자동적으로 병합하지 못하기 때문에 우리가 직접 조정해줘야함
- ` $ git branch -d <브랜치 이름>` : 브랜치 삭제
  (단, master 브랜치에 merged되지 않으면 -d로 삭제 불가)
- `$ git branch -D <브랜치 이름>` : 브랜치 강제 삭제
- `$ git log + 옵션`: 브랜치 로그 및 커밋 상태를 출력해줌(`--all`, `--graph` 등)
  - `$ git log --oneline --all`: 모든 브랜치에서의 커밋을 보여줌
  - `$ git log --oneline --all --graph`: 브랜치들의 상태(병합 및 펼치기)를 시각적으로 보여줌

