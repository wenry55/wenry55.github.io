---
layout: default
title: How to add remote and merge
categories: [git, remote]
tags: [git, remote]
---

workbench-client 와 workbench-server-for-web 둘다 방법이 동일하니, workbench-client 만 해보겠습니다.

#### 1. 로컬에 위치한 소스 경로로 이동

먼저 workbench-client 가 위치한 경로로 이동하여 `git remote -v`를 하여 아래와 같이 나오는지 확인합니다.
사내에서 클론하였다면 다음과 같이 `origin`이 설정되어 있어야 합니다.

```bsh
$ git remote -v
origin	https://git.ess.sk-nemo.com/git/jhhong_skcc/workbench_client.git (fetch)
origin	https://git.ess.sk-nemo.com/git/jhhong_skcc/workbench_client.git (push)
 
```
  
#### 2. 원격 레파지토리 추가

다음과 같이 하여 github에 있는 workbench-client를 `github`라는 이름으로 추가합니다. 추가한 뒤 `git remote -v`를 통해 제대로 추가되었는지 확인해 봅시다.

```bsh
$ git remote add github https://github.com/wenry55/workbench-client.git
$ git remote -v 
github	https://github.com/wenry55/workbench-client.git (fetch)
github	https://github.com/wenry55/workbench-client.git (push)
origin	https://git.ess.sk-nemo.com/git/jhhong_skcc/workbench_client.git (fetch)
origin	https://git.ess.sk-nemo.com/git/jhhong_skcc/workbench_client.git (push)
```

#### 3. develop 브랜치로 변경

현재 로컬의 브랜치가 master라고 가정합니다. 만약 현재 develop이라는 브랜치가 만들어져 있고 현재 선택된 브랜치가 develop 이라면 아래 부분은 넘어가도 됩니다.

```bsh
$ git branch
* master
$ git checkout -b develop # 만약 develop 브랜치가 이미 존재한다면 `-b`옵션은 제외합니다. `-b` 옵션은 체크아웃시에 브랜치가 없을경우, 브랜치를 만들면서 체크아웃 합니다.
Switched to a new branch 'develop'
$ git branch
* develop
  master
```

#### 4. github/develop 내려받기 및 push 실행시 적용될 remote repository/branch 설정

github로 부터 소스를 pull하여 로컬에 있는 소스와 머지 합니다. 그리고 이렇게 머지된 소스를 push할 때 사용할 remote repository(github)와 브랜치(develop)를 지정합니다.  

(만약 merge conflict가 일어나면 해당소스를 resolve 하도록 합니다.)
```bash
$ git pull github develop
From https://github.com/wenry55/workbench-client
 * branch            develop    -> FETCH_HEAD
 * [new branch]      develop    -> github/develop
Already up to date. # 이 부분은 조금씩 달라질 수 있습니다.
$ git branch --set-upstream-to github/develop
Branch 'develop' set up to track remote branch 'develop' from 'github'.
```

#### 5. 소스변경 후 push

이제 설정은 끝났으니 소스를 변경한 후 push 해보도록 하겠습니다.
여기서는 README.md 를 잠시 수정한후 이를 커밋/푸쉬 하는 예제입니다.

```bash
$ vi README.md
$ git status
On branch develop
Your branch is ahead of 'github/develop' by 36 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git add .
$ git commit -m 'README.md changed for test'
[develop d439c17] .
 1 file changed, 2 insertions(+)
$ git push
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 287 bytes | 287.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/wenry55/workbench-client.git
   575e56b..d439c17  develop -> develop
$ 
```

#### 6. develop branch 의 merge

모두들 develop 브랜치를 바라보고 있다고 가정하고, pull/push를 정상적으로 하였다고 가정하면, develop 브랜치에 최종 반영될 것입니다.
커밋/푸쉬가 끝나면 카톡으로 연락주세요.

#### 7. 수고하셨습니다.
