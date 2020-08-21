# GitHub를 통한 Cloud-Barista 컨트리뷰션 절차 가이드

## 개요
-	Github 컨트리뷰션 사이클 가이드를 통한 poc-farmoni repository에 컨트리뷰션 방법 공유
    -	Git remote repository 등록 및 Pull-request (PR) 처리 방법
-	cloud-barista community participant roles 
    -	Contributor : Issue 및 PR를 기여하는 사람
    -	Reviewer : Issue 및 PR를 검토하고 의견을 주는 사람
    -	Approver : Upstream 소스에 PR을 Merge하는 사람
-	cloud-barista community membership
    -	cloud-barista community member에 등록되면 Reviewer/Approver의 권한을 가진다. (현재)
        -	membership 등록 정책: TBD
    -	Merge를 수행하는 Approver는 @powerkimhub 및 @seokho-son으로 한정한다. (각 Repo의 공식 Approver 확정 전까지)
-	참고
    -	Issue
        -	문제를 제기
        -	담당자 assign을 통해서 컨트리뷰터 간의 중복 작업을 줄일 수 있음
    -	Pull request
        -	PR, 본인이 작업한 브랜치의 내용을 upstream에 반영해 달라고 요청하는 작업

## 컨트리뷰션 절차
- 개인 Github 계정으로, 컨트리뷰션 원하는 프로젝트 Fork
- 자신의 Github 계정에 있는 fork된 Repository를 개발 환경에 clone한다.
- 본 예시에서는 poc-farmoni라는 Repository를 대상으로 한다.

`$ git clone https://github.com/seokho-son/poc-farmoni.git`

- 자신의 Github 계정에 있는 fork된 Repository를 개발 환경에 clone한다.

`$ git clone https://github.com/seokho-son/poc-farmoni.git`

- clone된 디렉토리에 들어가서, git의 repository 상태를 확인한다.

`$ git remote -v`
```
origin	git@github.com:seokho-son/poc-farmoni.git (fetch)
origin	git@github.com:seokho-son/poc-farmoni.git (push)
```

- 원격 저장소(프로젝트의 원래 저장소)를 설정한다.

`$ git remote add upstream https://github.com/cloud-barista/poc-farmoni.git`

`$ git remote -v`
```
origin	git@github.com:seokho-son/poc-farmoni.git (fetch)
origin	git@github.com:seokho-son/poc-farmoni.git (push)
upstream	git@github.com:cloud-barista/poc-farmoni.git (fetch)
upstream	git@github.com:cloud-barista/poc-farmoni.git (push)
```

- 현재 브랜치를 확인한다.

`$ git branch`
```
* master
```

- 작업할 임시 브랜치를 생성하고, 해당 브랜치로 이동한다.

`$ git checkout -b up-common-config`
```
Switched to a new branch 'up-common-config'
```

`$ git branch`
```
master
* up-common-config
```

- 원하는 내용 수정
- 변경된 사항으로 빌드 및 테스트 진행하여 무결한지 확인
- Git 상태 확인 (not staged된 파일을 확인)

`$ git status`
```
On branch up-common-config
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
	modified:   farmoni_master/conf/config.yaml
no changes added to commit (use "git add" and/or "git commit -a")
```

`$ git diff`
```
diff --git a/farmoni_master/conf/config.yaml b/farmoni_master/conf/config.yaml
index 9ebeaf0..cf31b94 100644
--- a/farmoni_master/conf/config.yaml
+++ b/farmoni_master/conf/config.yaml
```

- Upstream에 커밋할 파일을 스테이징한다.

`$ git add farmoni_master/conf/config.yaml`

`$ git status`
```
On branch up-common-config
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
	modified:   farmoni_master/conf/config.yaml
```

- 스테이징된 파일들을 커밋한다. 이때 1줄 커밋 메시지는 이해하기 쉽게 작성한다.

`$ git commit -m "Update frmoni_master config for a common etri environment"`
```
[up-common-config bcf1ddf] Update frmoni_master config for a common etri environment
 1 file changed, 69 insertions(+), 69 deletions(-)
 rewrite farmoni_master/conf/config.yaml (68%)
```

`$ git status`
```
On branch up-common-config
nothing to commit, working tree clean
```

- Upstream repository의 최신 작업 내용을 fetch 받아온다. [필수이며 생략하며 안됨]

`$ git fetch upstream`
```
From github.com:cloud-barista/poc-farmoni
 * [new branch]      master     -> upstream/master
```

- Upstream repository의 최신 작업 내용 쪽으로 rebase한다. 이때 repo의 최신 상태와 conflict가 있으면, 컨트리뷰터가 반드시 해결을 하고, 다음 절차를 진행해야 한다.

`$ git rebase upstream/master`
```
Current branch up-common-config is up-to-date.
```

- Origin(본인의 github repository)에 작업 브랜치를 push(업로드) 한다.

`$ git push origin up-common-config`
```
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 883 bytes | 883.00 KiB/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
remote: 
remote: Create a pull request for 'up-common-config' on GitHub by visiting:
remote:      https://github.com/seokho-son/poc-farmoni/pull/new/up-common-config
remote: 
To github.com:seokho-son/poc-farmoni.git
 * [new branch]      up-common-config -> up-common-config
```

- Github의 fork 받아 온 repository에 접속하면, “Compare & pull request”가 활성화 된다. 이를 클릭하여, PR 생성 작업에 들어간다. PR 내용은 리뷰어나 승인자가 판단을 쉽게 할 수 있도록, 최대한 수정 내용을 명확하게 작성한다.
- 생성된 PR을 확인한다. Upstream repository에 PR 아이템이 생성된 것을 볼 수 있다. 리뷰어나 승인자는 PR의 내용 (파일 수정 사항 등)을 확인하여 의견을 남긴다.
- 리뷰어가 의견을 준 경우에는 의견에 따라 내용을 수정 후 커밋 및 PR을 업데이트 한다. 업데이트된 PR은 다시 리뷰를 받는다. 기존 PR을 업데이트하기 위해서는 작업 공간(임시 작업 브랜치)에서 파일을 수정, git commit –a –m “Rev 1 xxxxxx” (스태이징 및 커밋), git fetch upstream (최신 정보 업데이트), git rebase upstream/master (최신 버전과 차이 비교 및 로컬 머징), git push origin [PR을 올린 임시 작업 브랜치 이름] --force 를 수행한다. 마지막 push에서는 --force 옵션을 줘야 기존의 PR을 업데이트할 수 있다.
- 리뷰어가 merge 가능 의견을 남기면, 승인자가 해당 PR을 Master branch로 merge한다.
