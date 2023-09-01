# 06_Git Upstream

![image](https://user-images.githubusercontent.com/93081720/215497874-6f09a672-618d-410d-9106-e395f5021ef7.png)

## 1. Origin

git clone 또는 git init을 했을 때, git pull/push origin ..., git remote add origin ...과 같이

`origin`이라는 단어를 볼 수 있다.

여기서 `origin`이란 git에 이미 존재하는 repository 즉, remote repository를 의미한다

<br>

## 2. Upstream / Downstream

> 상대적 개념

upstream과 downstream은 상대적인 개념이라 origin과 local을 기준으로 생각하면 origin이 upstream, local이 downstream이 된다.

그 이유는 push와 pull을 기준으로 했을 때, origin - upstream(상류)으로부터 local - downstream(하류)로 흐르는 관계가 형성되기 때문이다.

`git push -u origin main/master`에서 `-u`옵션은 `--set-upstream`옵션의 줄임말로서, origin의 main 또는 master를 upstream으로 설정한다는 의미이다.

그래서 local과 upstream-downstream 관계가 형성되어 git push/pull만 해도 main 또는 master로 push/pull이 진행되는 것이다.

<br>

### 1) Fork

그러나 fork를 하게되면 upstream-downstream 관계가 조금 헷갈릴 수도 있다.

기존에는 local이 downstream, origin이 upstream이었다면, fork한 상태에서는 origin(나의 레포지토리)이 downstream, 원본(fork한 레포지토리)이 upstream이 되기 때문이다.

즉, fork를 한다면 origin은 본인이 원본 repository로부터 fork를 한 나의 repository이고, fork 대상이었던 원본이 remote로 upstream이 된다. (물론 local의 입장에서는 origin도 remote이다.)

#### (1) 작업 플로우

1. `원본 remote repository(upstream)`를 깃허브에서 `fork`
2. fork한 `remote repository(origin)`를 깃 클라이언트로 clone
3. 기능을 완성할 때까지 반복
   1. clone한 repository(local)에 commit
   2. local에서 origin으로 push
4. upstream에 반영하기
   1. PR을 등록하기 전 upstream에 **바뀐 내용이 없는 경우**
      1. origin에서 upstream(원본 remote repository)으로 PR(Pull Request)
   2. PR을 등록하기 전 upstream에 **바뀐 내용이 있는 경우**
      1. upstream(원본 remote repository)에서 local로 pull
      2. local에서 origin으로 push
      3. origin에서 upstream으로 PR(Pull Request)

<br>