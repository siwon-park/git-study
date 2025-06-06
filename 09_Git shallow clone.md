# 09_Git shallow clone

> Git Repository에서 특정 History(commit)만 clone 해오기

엄청나게 큰 저장소(몇년의 역사를 가진 큰 프로젝트)를 클론하는 작업은 전송 데이터가 많아 시간이 오래 걸리거나 때로는 네트워크 이슈 등으로 인해서 실패하는 경우가 생기기도 한다.

## 1. shallow clone (얕은 클론)

이럴 때 필요한 것이 바로 shallow clone이다. 전체 프로젝트의 history가 필요한 경우가 아닌 이상 shallow clone을 하는 것이 좋을 수도 있다.

shallow clone은 전체 프로젝트의 history를 다 가져오는 deep clone에 비해 확실히 디스크 사용량, 시간도 빠르다.

### 1) 장점

가장 최신 commit history를 기준으로 프로젝트를 클론해올 수 있기 때문에 프로젝트를 빠르게 실행해보고자 하는 혹은 소스 코드 검색이 목적인 굳이 전체 히스토리가 필요없는 상황에서는 매우 큰 장점을 지닌다.

또한 전체 히스토리를 가져오려고 deep clone을 하려고 했을 때, 네트워크 이슈 등으로 인해 클론이 중간에 실패한다면 shallow clone을 하여 단계적으로 전체 히스토리를 증분하여 가져올 수도 있다.

### 2) 단점

그러나 얕은 클론은 치명적인 side-effect가 존재한다. 전체 히스토리를 가져온 것이 아니기 때문에 협업을 함에 있어서 문제가 발생할 수도 있다.

특정 코드를 수정하고 push를 하려고 할 때, 해당 코드에 대한 Git의 이력이 확인되지 않는 문제가 발생할 수도 있다. 즉, 이 수정된 코드가 어떤 이력으로 만들어졌는지 확인을 못하기 때문에 push 자체가 거부될 수도 있다.

Git은 단순히 파일을 업로드하는 파일 저장소의 개념이 아니라 형상 관리(이력 관리) 시스템이기 때문에 파일 자체보다 이력이 더 중요하다.

따라서 해당 프로젝트를 개발하는 개발자라면 절대로 shallow clone을 해서는 안 된다.

### 3) 명령어

clone을 해올 때 `--depth` 옵션을 줘서 shallow clone을 해올 수 있다.

```bash
git clone --depth=[숫자] [레포지토리 url]

// 예시
git clone --depth=1 https://github.com/denoland/deno.git .
```

※ 일반적으로 clone에 아무런 옵션을 주지 않으면 전체 히스토리를 가져올 수 있고 이를 deep clone이라고 한다.

`--depth` 옵션에 1을 주고 `git log --oneline -[n]`으로 커밋 히스토리를 n개까지 확인 시, 가장 최근 커밋 히스토리만 확인할 수 있다. 

<br>

## 2. unshallow

shallow clone을 통해서 얕은 클론을 했을 때, 이를 해제하고 전체 히스토리를 가져올 수도 있다.

이 때, 줄 수 있는 옵션이 `--unshallow`이다. (이미 클론을 해온 상태이기 때문에 fetch 명령어를 사용해야 한다.)

```bash
git fetch [origin 특정 브랜치(생략 가능)] --unshallow
```

### 1) 증분 fetch

네트워크 이슈 때문에 단계적으로 가져오는 것이 필요한 상황이라면 다음과 같이 fetch 명령어에 `--depth`옵션을 주면 된다.

```bash
git fetch [origin 특정 브랜치(생략 가능)]--depth [숫자]

// 예시
git fetch --depth 1
git fetch --depth 5
git fetch --depth 10
git fetch --depth 50
git fetch --depth 100
```

