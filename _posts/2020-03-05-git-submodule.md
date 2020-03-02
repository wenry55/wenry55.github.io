---
layout: default
title: Git Submodule
categories: [git]
tags: [git, submodule]
---

##### submodule 사용하기

우선 submodule을 추가하는 명령은 아래와 같습니다.

```bash
git submodule add <repository> [path]  # path는 optional
```

위 명령에서 path가 생략되었을 경우, repository이름과 동일한 디렉토리를 사용합니다.

이를 이용하여 아래의 예를 살펴봅시다. 이 테스트를 진행하기 위해서 remote 에 parent / child 두개의 git repo를 구성해 두었습니다.

```bash
# 먼저 submodule을 테스트 하기 위하여 parent로 사용할 remote repo를 클론합니다.
$ git clone /Users/bongkyo/Documents/tmp/remote-pc/submodule/parent/
Cloning into 'parent'...
done.
$ ls
parent
$ cd parent/
$ ls -ltr
total 0
-rw-r--r--  1 bongkyo  staff  0 Mar  6 01:55 README.md
$ pwd
/Users/bongkyo/Documents/tmp/local-pc/submod/parent  # parent 생성확인합니다.

$ git submodule add /Users/bongkyo/Documents/tmp/remote-pc/submodule/child/
Cloning into '/Users/bongkyo/Documents/tmp/local-pc/submod/parent/child'...
done.  # [path] 옵션이 생략되어 remote repository의 명칭(디렉토리명)이 사용됩니다.

# child가 만들어 졌는지 확인합니다.
$ ls -ltr
total 0
-rw-r--r--  1 bongkyo  staff    0 Mar  6 01:55 README.md
drwxr-xr-x  4 bongkyo  staff  128 Mar  6 01:57 child  
$ cd child
$ ls
this-is-child.txt


$ git remote -v  # child 디렉토리는 별도의 repository와 remote(origin)를 가집니다. 
origin	/Users/bongkyo/Documents/tmp/remote-pc/submodule/child/ (fetch)
origin	/Users/bongkyo/Documents/tmp/remote-pc/submodule/child/ (push)
$ cd ..
$ git remote -v  # parent는 child와 다른 repository 입니다.
origin	/Users/bongkyo/Documents/tmp/remote-pc/submodule/parent/ (fetch)
origin	/Users/bongkyo/Documents/tmp/remote-pc/submodule/parent/ (push)

# .gitmodules가 생성되어 있는지 확인합니다. 
# submodule이 있을 경우는 .gitmodules가 존재합니다.
$ ls -tlra
total 8
drwxr-xr-x   3 bongkyo  staff   96 Mar  6 01:55 ..
-rw-r--r--   1 bongkyo  staff    0 Mar  6 01:55 README.md
drwxr-xr-x   4 bongkyo  staff  128 Mar  6 01:57 child
-rw-r--r--   1 bongkyo  staff   97 Mar  6 01:57 .gitmodules
drwxr-xr-x   6 bongkyo  staff  192 Mar  6 01:57 .
drwxr-xr-x  13 bongkyo  staff  416 Mar  6 01:57 .git

$ cat .gitmodules 
[submodule "child"]
	path = child
	url = /Users/bongkyo/Documents/tmp/remote-pc/submodule/child/

```


참조) [Git Subtree](/git/git-subtree)
