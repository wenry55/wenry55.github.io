---
layout: default
title: Git Branch
categories: [git]
tags: [git, branch]
---

##### 리모트 브랜치 삭제하기
```bash

$ git branch -a  # 리모트 브랜치 조회 (-a)
* master
  remotes/origin/master
  remotes/origin/wenry55-patch-1
  remotes/origin/wenry55-patch-2

$ git push origin --delete wenry55-patch-1  # push with --delete option
To https://github.com/wenry55/wenry55.github.io
 - [deleted]         wenry55-patch-1

```

##### 다른 브랜치를 가져와서 머지하기

##### 현재 브랜치를 다른 브랜치에 머지하기
