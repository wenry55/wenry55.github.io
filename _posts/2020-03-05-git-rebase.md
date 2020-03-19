---
layout: default
title: Git Rebase
categories: [git]
tags: [git, rebase]
---

# Git Rebase
### Abstract

git rebase 현재 브랜치의 HEAD를 `1) 과거의 다른 커밋` 또는 `2) 다른 브랜치의 HEAD`로 옮길 수 있게 도움을 주는 명령어 입니다. 위 두 경우 각각에 따라 약간 다른 동작이라고 생각하기 쉬운데, 사실은 같은 오퍼레이션 입니다. git-rebase명령의 동작은 우선 현재의 헤드를 다른 commit ref로 옮기고, 해당 커밋 이후의 커밋들은 pick/squash/drop 등의 지정을 통해 정리를 한다는 측면에서 동일합니다.  

git merge / git rebase 두 명령은 모두 branch를 merge 합니다.

하지만 git merge는 새롭게 "merge commit" 을 history상에 생성하는데 반해, rebase는 merge 하려는 브랜치의 커밋들을 로컬로 다 가져오고, 브랜치의 마지막 커밋에 현재 브랜치의 HEAD pointer를 위치시킨다는 점이 틀립니다. 

다른 시각으로 보면 현재 시점의 master로 부터 새롭게 fork 하는 것과 다를 바 없습니다.

### 개념적인 오버뷰

우선 이 명령은 git merge가 해결하는 것과 같은 결과를 가져옵니다. 
git merge / git rebase, 이 명령어는 한 브랜치의 변경사항을 다른 브랜치에 합치는 역할을 합니다. 하지만 하는 방식이 틀립니다.

여러분이 새로이 feature 브랜치를 할당받아 새로운 기능을 개발하기 시작하였는데, 다른 팀이 master 브랜치에 새로운 커밋들을 하였다고 합시다. 그 결과는 forked history가 됩니다.

[A forked commit history](https://wac-cdn.atlassian.com/dam/jcr:01b0b04e-64f3-4659-af21-c4d86bc7cb0b/01.svg?cdnVersion=864)

자, 그런데, 마스터에서 커밋한것 중에 현재 feature를 개발하는 데 필요한 것이 있다고 해보죠. 이 새로운 커밋을 feature branch에 반영하기 위해서는 두가지 옵션이 있습니다. merging과 rebasing입니다.



```
git checkout feature
git merge master
```
한 라인으로 줄이면, 

```
git merge feature master
```


> 다른 방법으로, 만약 feature 브랜치에서 현재까지 개발한 것이 거의 없거나, 다시 작성해도 부담이 없을 경우에는 feaure 브랜치를 삭제하고 새로이 feature 브랜치를 만드는 것이 편합니다. 

#### The Rebase Option
다른 merging 방법으로, feature 브랜치를 master 브랜치 위로 rebase하는 겁니다.

```bash
# 명령이 위의 merge 명령과 동일합니다. rebase빼고.
git checkout feature
git rebase master
```
이 동작은 전체 feature 브랜치를 master 브랜치의 끝에서 시작되게 바꿉니다.

[Rebasing the feature branch onto master](https://wac-cdn.atlassian.com/dam/jcr:5b153a22-38be-40d0-aec8-5f2fffc771e5/03.svg?cdnVersion=864)

The major benefit of rebasing is that you get a much cleaner project history. First, it eliminates the unnecessary merge commits required by git merge. Second, as you can see in the above diagram, rebasing also results in a perfectly linear project history—you can follow the tip of feature all the way to the beginning of the project without any forks. This makes it easier to navigate your project with commands like git log, git bisect, and gitk.

위의 말 중에 오해하기 쉬운 부분이 "you get a much cleaner project history" 라는 부분입니다. 이는 feature를 rebase 하게되었을 때, 새로운 "merge commit" 이 발생하지 않는 다는 뜻이지, master로 부터 fork 한 시점이후에 발생한 commit 들을 하나로 묶는다던지, 그런 말이 아닙니다. merge commit이 없어서 클린해 보인다는 겁니다. 

git merge 명령은 feature 브랜치에 새로운 "merge commit"을 만들게 되고, 두 브랜치의 history를 묶고, 다믐과 같은 branch structure를 만들게 됩니다.

[Merging master into feature branch](https://wac-cdn.atlassian.com/dam/jcr:e229fef6-2c2f-4a4f-b270-e1e1baa94055/02.svg?cdnVersion=864)


git rebase는 현재의 HEAD가 가리키는 커밋을 다른 커밋으로 변경하겠다는 뜻입니다.

![rebase](https://wac-cdn.atlassian.com/dam/jcr:e4a40899-636b-4988-9774-eaa8a440575b/02.svg?cdnVersion=861)


`git rebase -i HEAD~3`

위의 명령은 현재 브랜치의 HEAD의 위치를 변경시킵니다. git reset와 어느정도 비슷한 면이 있네요. 이 명령을 수행하였을 때는 rebase 대상 커밋 이후에 3개의 orphan commit 이 생기게 됩니다. 이 orphan commit 을 유지하려면 pick, 하나로 합치려면 squash 를 합니다.


```bash
$ git log --oneline
6c316a5 (HEAD -> master) .
9bc63a8 v3
5a2c182 v2
c3f6f80 v1

$ git rebase -i HEAD~2  # 모두 drop을 선택하였을 경우.
Successfully rebased and updated refs/heads/master.

$ git log --oneline
5a2c182 (HEAD -> master) v2
c3f6f80 v1
```

```bash
$ git log --oneline
0abb1d3 (HEAD -> master) v5
16f317c v4
7cca5bf v3
5a2c182 v2
c3f6f80 v1

$ git rebase -i HEAD~3  # v3를 pick, v4, v5는 squash
[detached HEAD 9a50836] v3
 Date: Fri Mar 6 22:09:25 2020 +0900
 3 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 v3.txt
 create mode 100644 v4.txt
 create mode 100644 v5.txt
Successfully rebased and updated refs/heads/master.

$ git log --oneline
9a50836 (HEAD -> master) v3
5a2c182 v2
c3f6f80 v1
 


참고) 
[Git Reset](/git-reset)