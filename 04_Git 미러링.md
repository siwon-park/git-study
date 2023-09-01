# 04_Git_Mirroring

## 1. Gitlab에서 Github로 저장소 옮기기(미러링)

### 1) 미러링(Mirroring)을 하는 이유?

> Git Commit Log를 유지하기 위해서!

#### (1) 명령어

```bash
$ git clone --mirror [원본 레포지토리 경로]
```

※ 이 때, 현재 디렉토리로 복사해오는 [원본 레포지토리 경로] 뒤에 '.'을 찍으면 아래와 같이 폴더가 클론된다.

![image](https://user-images.githubusercontent.com/93081720/197517747-b763dbc6-8d5b-46e6-aa8e-0af32e776281.png)

이렇게 '.'을 찍지 않으면`.git`폴더가 생기지 않는다.

![image](https://user-images.githubusercontent.com/93081720/197517921-4ac7c35a-a017-4b62-9044-76f4c6277495.png)

해당 폴더로 들어가든, '.'을 찍어 현재 디렉토리에 복사를 하든 hook이 있는 디렉토리에서 아래 명령어를 입력

```bash
$ git remote set-url --push origin [이동할 원격 레포지토리 주소]
```

브랜치 주소가 `(BARE:master)`로 바뀐다. 그 후 git push를 하면 된다

```bash
$ git push --mirror
```

![image](https://user-images.githubusercontent.com/93081720/197518648-b8ab4e2e-8223-4262-bf5d-bfe46934ef0f.png)

#### (2) 참고 사항

50mb가 넘으면 깃헙에 올리지 못하므로 이럴 땐, git lfs를 설치해서 올리면 된다.