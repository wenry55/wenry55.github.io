---
layout: default
title: Git Remote
categories: [git, remote]
tags: [git, remote]
---

### Git Remote

Remote repositories are versions of your project that are hosted on the Internet or network somewhere.

##### 현재 등록되어 있는 리모트 보기 

```
git remote -v
```
위의 명령을 실행하면 현재 등록된 리모트 레퍼지토리가 무엇인지 알 수 있습니다. git clone 을 하면 클론을 한 리모트 리파지토리가 origin(기본이름)이라는 이름으로 등록됩니다. 

##### 여러개 존재하는 리모트
리모트는 여러개 존재할 수 있습니다. 아래처럼 말이죠.

```
$ git remote -v
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```
##### 리모트 추가하기

```bash
              리모트로 등록할 이름      리모트 주소
                   |                  |
                   V                  V
$ git remote add cho45 https://github.com/cho45/grit.git
$ git remote -v
cho45     https://github.com/cho45/grit.git (fetch)
cho45     https://github.com/cho45/grit.git (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```

##### 리모트 이름 변경하기

두개의 리모트가 존재한다고 가정해 봅시다. 예를 들어, 한개는 사내에서만 접근가능한 리모트이고, 다른 한개는 인터넷에서 접근가능한 리모트 말입니다. 사내에서 개발할때는 사내의 리모트를 clone 하여 쓸 경우, 사내시스템이 origin이라는 이름으로 등록이 됩니다. 하지만 외부에서는 사내시스템이 접근불가능하므로, 인터넷에 있는 리모트를 clone하게 되는데요, 이 때는 이 리모트가 origin 이 되어 버립니다. 그래서, 이러한 혼란을 없애기 위해서 `rename` 커맨드가 있습니다. 
```bash
git remote rename origin github
```
위와 같이 외부에서 클론한 시스템의 이름을 github로 바꾸어 놓고 쓰다가 사내에 들어와서 
```bash
git remote add origin https://internal.../proj.git
```
와 같이 해서 origin을 항상 사내에 있는 시스템으로, github 를 사외시스템으로 머릿속으로 구분하는게 쉽습니다.

#### git fetch 와 git pull

`git fetch`는 리모트의 레퍼지토리를 로컬에 복사(다운로드)한다는 뜻입니다. 어디에 복사하냐구요? HEAD 트리에 복사합니다. 하지만 merge는 하지 않습니다.

##### 로컬PC의 다른경로에 존재하는 리모트

>git에서 리모트는 반드시 인터넷 저쪽에 있는 레퍼지토리를 말하는 것이 아닙니다. git에서의 리모트의 개념은 현재 작업하는 로컬 레퍼지토리를 제외한 다른 경로(인터넷 경로 및 로컬 pc의 경로)를 말합니다.

아래는 로컬pc의 다른 경로를 remote로 사용하는 예를 구성해 보았습니다.
먼저 remote-pc라는 디렉토리와 local-pc라는 디렉토리를 tmp라는 디렉토리 아래에 만듭니다. 

```sh
$ mkdir tmp
$ cd tmp  
$ mkdir remote-pc
$ mkdir local-pc
$ ls -ltr
total 0
drwxr-xr-x  2 wenry55  staff  64 Mar  5 21:41 remote-pc
drwxr-xr-x  2 wenry55  staff  64 Mar  5 21:42 local-pc
$ pwd
/Users/wenry55/Documents/tmp
```

리모트에 proj1이라는 레퍼지토리를 만듭니다. 
```sh
$ cd remote-pc/
$ mkdir proj1
$ cd proj1/
$ git init
Initialized empty Git repository in /Users/wenry55/Documents/tmp/remote-pc/proj1/.git/
$ touch README.md
$ git add .
$ git commit -m 'Init'
[master (root-commit) 4009650] Init
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

이제 로컬pc에 remote-pc의 proj1 레파지토리를 클론합니다.
```sh
$ pwd
/Users/wenry55/Documents/tmp/remote-pc/proj1
$ cd ../../local-pc/
$ git clone /Users/wenry55/Documents/tmp/remote-pc/proj1/
Cloning into 'proj1'...
done.
$ cd proj1/
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
$ git remote -v  # remote-pc/proj1 디렉토리를 tracking 하는것을 확인합니다.
origin	/Users/wenry55/Documents/tmp/remote-pc/proj1/ (fetch)
origin	/Users/wenry55/Documents/tmp/remote-pc/proj1/ (push)
$ ls -tlr  # 정상적으로 clone
total 0
-rw-r--r--  1 wenry55  staff  0 Mar  5 21:44 README.md

```
