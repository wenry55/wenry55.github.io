---
title: "Find remote a branch is tracking"
layout: default
tags: [git, remote, branch]
categories: [git, remote]
---


```bash

# 로컬브랜치와 각 로컬브랜치의 리모트
$ git branch -vv
* develop e3e312d .
  master  e3e312d [origin/master] .

# 모든브랜치와 트래킹 리모트
$ git branch -a -vv
* develop                        e3e312d .
  master                         e3e312d [origin/master] .
  remotes/origin/HEAD            -> origin/master
  remotes/origin/master          e3e312d .
  remotes/origin/wenry55-patch-1 e387d2c Delete index.html
  remotes/origin/wenry55-patch-2 cce653a Create mytest.md

# 로컬브랜치의 push/pull 설정
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/wenry55/wenry55.github.io.git
  Push  URL: https://github.com/wenry55/wenry55.github.io.git
  HEAD branch: master
  Remote branches:
    master          tracked
    wenry55-patch-1 tracked
    wenry55-patch-2 tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
$    
```