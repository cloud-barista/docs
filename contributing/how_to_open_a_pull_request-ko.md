# GitHub를 통한 컨트리뷰션 절차 가이드

## 개요
-	GitHub 컨트리뷰션 절차에 따라 원하는 저장소(Repository)에 컨트리뷰션 방법 공유
    -	Git remote repository 등록 및 Pull-request (PR) 처리 방법
-	Community participant roles
    -	Contributor: Issue 및 PR를 기여하는 사람
    -	Reviewer: Issue 및 PR를 검토하고 의견을 주는 사람
    -	Approver: Upstream 소스에 PR을 Merge를 승인하는 사람
-	참고
    -	Issue
        -	문제를 제기
        -	담당자 Assign을 통해서 컨트리뷰터 간의 중복 작업을 줄일 수 있음
    -	Pull request
        -	PR, 본인이 작업한 브랜치의 내용을 upstream에 반영해 달라고 요청하는 작업

## 컨트리뷰션 절차
- 개인 GitHub 계정(예시: cb-contributor)으로, 컨트리뷰션 원하는 프로젝트(예시:cloud-barista/cb-tumblebug)를 Fork한다.
- 자신의 GitHub 계정에 있는 Fork된 Repository를 개발 환경에 Clone한다(Fork된 Repository는 **Origin으로 통용**).
- 본 예시에서는 cb-tumblebug라는 Repository를 대상으로 한다.

- 자신의 GitHub 계정에 있는 Fork된 Repository를 개발 환경에 Clone한다.

`$ git clone https://github.com/cb-contributor/cb-tumblebug.git`

- Clone된 디렉토리에 들어가서, Git의 Repository 상태를 확인한다.

`$ git remote -v`
```
origin	git@github.com:cb-contributor/cb-tumblebug.git (fetch)
origin	git@github.com:cb-contributor/cb-tumblebug.git (push)
```

- Remote repository를 추가로 지정한다(프로젝트의 본래 Repository이고 **Upstream으로 통용**).

`$ git remote add upstream https://github.com/cloud-barista/cb-tumblebug.git`

`$ git remote -v`
```
origin	git@github.com:cb-contributor/cb-tumblebug.git (fetch)
origin	git@github.com:cb-contributor/cb-tumblebug.git (push)
upstream	git@github.com:cloud-barista/cb-tumblebug.git (fetch)
upstream	git@github.com:cloud-barista/cb-tumblebug.git (push)
```

- Upstream 저장소의 최신 내용을 fetch(정보 업데이트)한다.

`$ git fetch upstream`

- 작업의 Base가 되는 Upstream의 특정 브랜치(예: main)로 checkout(이동, 해당 브랜치의 File들을 Local에 적용) 한다.

`$ git checkout upstream/main`

- 작업할 임시 브랜치를 생성(`-b` 옵션)하고, 해당 브랜치로 Checkout한다.

`$ git checkout -b feature-add-new-idea`
```
Switched to a new branch 'feature-add-new-idea'
```

`$ git branch`
```
main
* feature-add-new-idea
```

- 원하는 내용 수정
- 변경된 사항으로 빌드 및 테스트 진행하여 무결한지 확인
- Git 상태 확인 (not staged된 파일을 확인)

`$ git status`
```
On branch feature-add-new-idea
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
	modified:   src/conf/config.yaml
no changes added to commit (use "git add" and/or "git commit -a")
```

`$ git diff`
```
diff --git a/src/conf/config.yaml b/src/conf/config.yaml
index 9ebeaf0..cf31b94 100644
--- a/src/conf/config.yaml
+++ b/src/conf/config.yaml
```

- Upstream에 커밋할 파일을 스테이징한다. (여러개의 수정된 파일들을 스테이징할 수 있다.)

`$ git add src/conf/config.yaml`

`$ git status`
```
On branch feature-add-new-idea
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
	modified:   src/conf/config.yaml
```

- 스테이징된 파일들을 커밋한다. 이때 1줄 커밋 메시지는 이해하기 쉽게 작성한다.

`$ git commit -m "Update config for a common environment"`
```
[feature-add-new-idea bcf1ddf] Update config for a common environment
 1 file changed, 69 insertions(+), 69 deletions(-)
 rewrite src/conf/config.yaml (68%)
```

`$ git status`
```
On branch feature-add-new-idea
nothing to commit, working tree clean
```

- Upstream repository의 최신 작업 내용을 Fetch 받아온다. (필수이며 생략하면 안됨. 상시로 확인하는 것도 좋은 방법.)

`$ git fetch upstream`
```
From github.com:cloud-barista/cb-tumblebug
 * [new branch]      main     -> upstream/main
```

- Upstream repository의 최신 작업 내용 쪽으로 Rebase(Upstream의 최신 내용과 동기화하는 작업)한다. 이때 Repository의 최신 상태와 Conflict가 있으면, 컨트리뷰터가 반드시 해결을 하고, 다음 절차를 진행해야 한다.

`$ git rebase upstream/main`
```
Current branch feature-add-new-idea is up-to-date.
```

- Origin(본인의 GitHub repository)에 작업 브랜치를 Push(업로드) 한다.

`$ git push origin feature-add-new-idea`
```
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 883 bytes | 883.00 KiB/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
remote: 
remote: Create a pull request for 'feature-add-new-idea' on GitHub by visiting:
remote:      https://github.com/cb-contributor/cb-tumblebug/pull/new/feature-add-new-idea
remote: 
To github.com:cb-contributor/cb-tumblebug.git
 * [new branch]      feature-add-new-idea -> feature-add-new-idea
```

- GitHub의 Fork 받아 온 Repository에 접속하면, “Compare & pull request”가 활성화 된다. 이를 클릭하여, PR 생성 작업에 들어간다. PR 내용은 리뷰어나 승인자가 판단을 쉽게 할 수 있도록, 최대한 수정 내용을 명확하게 작성한다.
- 생성된 PR을 확인한다. Upstream repository에 PR 아이템이 생성된 것을 볼 수 있다. 리뷰어나 승인자는 PR의 내용 (파일 수정 사항 등)을 확인하여 의견을 남긴다.
- 리뷰어가 의견을 준 경우에는 의견에 따라 내용을 수정 후 커밋 및 PR을 업데이트 한다. 업데이트된 PR은 다시 리뷰를 받는다. 
- 기존 PR을 업데이트하기 위해서는 
  - 작업 공간(임시 작업 브랜치)에서 파일을 수정
  - git add xxx_file (수정된 파일 스태이징)
  - git commit –m “Rev 1 xxxxxx” (스태이징 및 커밋), 
  - git fetch upstream (최신 정보 업데이트), 
  - git rebase upstream/main (최신 버전과 차이 비교 및 로컬 머징), 
  - git push origin [PR을 올린 작업 브랜치 이름] --force 를 수행한다. (rebase를 수행한 경우에만 --force 옵션을 사용)
  - PR이 자동으로 업데이트 된다.
- 리뷰어가 Merge 가능 의견을 남기면, 승인자가 해당 PR을 main branch로 Merge한다.
