# 07_Git_Config

Git 사용자 정보 확인

## 1. Git bash 사용자 정보 확인

인턴하면서 지급받은 노트북에서 내 레포로 푸시했더니 이전 사용자가 Git에 로그인한 상태여서 내 계정으로 Commit되지 않고, 로그인된 계정으로 Commit된 사고가 발생했다.

의도치 않게 학습용 레포에 컨트리뷰터가 추가되었다.

![화면 캡처 2023-09-19 081519](https://github.com/siwon-park/git-study/assets/93081720/592c2b94-32d0-488b-8181-4bcb693013f9)

앞으로 해당 노트북으로 내 레포에 작업하여 Push할 일도 있을 것 같아 사용자 정보 변경이 필요했다.

### 1) Git 사용자 정보 출력

```bash
git config --list
```

![화면 캡처 2023-09-19 082257](https://github.com/siwon-park/git-study/assets/93081720/a85fcfbf-4d90-4b05-b1ba-000df7247848)

혹은 다음과 같이 유저 이름 및 메일만 출력 가능하다.

- --global은 빼도 무관하다.

```bash
git config --global user.name
git config --global user.email
```

<br>

### 2) Git 사용자 정보 변경

```bash
git config --global user.name [변경할 유저 이름]
git config --global user.email [변경할 유저 이메일]
```

변경할 유저명과 이메일을 입력한 다음에 확인해보면 다음과 같이 바뀌었음을 알 수 있다.

![화면 캡처 2023-09-19 173216](https://github.com/siwon-park/git-study/assets/93081720/aac1ed89-5ff9-4866-9684-2e55e6d2be8e)
