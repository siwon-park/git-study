# 05_Git History 삭제

자율 프로젝트를 다 끝내고 미러링해서 git organization에 레포지토리를 올렸는데, 며칠 전에 어느 한 외국인 유저가 git history에 키가 노출된 것이 남아있으니 삭제하는 게 좋겠다며 이슈를 open하였다. 

![화면 캡처 2022-12-06 203333](https://user-images.githubusercontent.com/93081720/205902951-e1ec2200-7863-488f-baf8-c5e87ad45acd.png)

사실 일전부터 인지하고 있었던 문제였는데 크게 신경 쓰지 않았다. 방법도 몰랐고, 크게 문제될 게 없다고 생각해서 그랬다. 그런데 이렇게 이슈로 남겨서 피드백 줬으니 가만히 있을 수는 없었다.

## 1. Git History 삭제하기

### 1) 방법

#### (1) git history 삭제 및 rewrite

```bash
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch [./경로/경로/파일명.확장자]' --prune-empty --tag-name-filter cat -- --all
```

![화면 캡처 2022-12-06 202406](https://user-images.githubusercontent.com/93081720/205903936-8e732660-ba9e-44c4-9ed0-86a686a71323.png)

- 예) `git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch ./FE/alertyou/android/app/google-services.json' --prune-empty --tag-name-filter cat -- --all`

해당 명령어를 입력하면 잠시 후에 위 사진과 같이 history를 Rewrite하기 시작한다.

#### (2) 바뀐 내용 branch 업로드

push할 때 반드시 `--force --all`옵션을 넣는 것을 잊지 말자!

```bash
git add .
git commit -m "chores: 깃 히스토리 삭제 및 재작성"
git push origin --force --all
```

<br>

### 2) 주의 사항

#### (1) 경로를 잘 쓸 것!

처음에 `경로/경로/파일명.확장자`이런 식으로 썼더니 뭔가 작업은 진행되었으나, 제대로 history가 업데이트 되지 않았다. 알고보니 상대 경로로 작성해줘야 했었다. `./경로/경로/파일명.확장자`

![image](https://user-images.githubusercontent.com/93081720/205905554-d83dd2bf-026f-4cb6-9f86-c4dd694d7faa.png)

제대로 업데이트 되었을 때만 `was rewritten`이라고 뜬다. (처음에 경로를 제대로 입력하지 못했을 때는 전부 다 `unchanged`였다.)