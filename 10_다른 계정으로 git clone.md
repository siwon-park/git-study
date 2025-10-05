# 10_다른 계정으로 git clone 하기

> OS상에 이미 로그인된 계정과 다른 계정으로 git clone을 하고자 할 때

## 1. 쉘에 로그인된 계정과 다른 계정으로 git clone하기

git의 레포지토리에서 프로젝트를 clone해올 때 간혹 다음과 같이 인증 문제가 발생하는 경우가 생길 수도 있다.

```bash
remote: HTTP Basic: Access denied. The provided password or token is incorrect or your account has 2FA enabled and you must use a personal access token instead of a password.
See https://{git메인주소}/help/topics/git/troubleshooting_git#error-on-git-fetch-http-basic-access-denied
fatal: Authentication failed for 'https://{git메인주소}/{계정명}/{repo명}.git/'
```

일반적인 경우라면 프로젝트가 public일 경우에 clone 시에는 이런 문제가 발생하지 않는다.

하지만 프로젝트가 private이거나, 조직에 속한 사용자여야만 열람 가능한 경우, 위와 같이 인증 에러가 발생하게 된다.

이 때에는 2가지 방법을 사용할 수 있다.

### 1) 해당 repo에 접근 가능한 계정으로 로그인

#### (1) 기존 git 계정 확인

```bash
git config --global user.name
git config --global user.email
```

#### (2) git 인증 정보 삭제

```bash
git credential-cache exit
```

#### (3) git clone

다시 한 번 동일하게 git clone 명령어를 입력하면 이번에는 로그인 팝업창이 나오고, 정상적으로 인증 시에 프로젝트를 clone받을 수 있다.

<br>

### 2) 접근하려는 계정 명시

이와 달리, 접근하려는 계정을 명시하고 인증 정보를 입력하여 git clone 받는 방법도 존재한다. (훨씬 편함)

#### (1) 명시적으로 사용자명 입력하여 git clone하기

```bash
git clone https://<username>@{git주소}/{계정명}/{repo명}.git
```

해당 repo에 접근 가능한 계정명을 https 프로토콜 뒤에 @과 함께 입력할 경우 계정에 대한 비밀번호 입력 후 git clone이 가능해진다.

굳이 로그인된 계정을 로그아웃할 필요 없어 훨씬 더 간단한 방법이다.