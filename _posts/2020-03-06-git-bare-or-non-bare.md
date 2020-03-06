---
layout: default
title: Bare / Non Bare Repository
categories: [git]
tags: [git, bare]
---

##### bare / non-bare 

bare는 remote origin repository가 등록되어 있지 않은 저장소를 말합니다.


Another difference between a bare and non-bare repository is that a bare repository does not have a default remote origin repository:

```bash
$ git clone --bare test bare
Initialized empty Git repository in /home/derek/Projects/bare/
$ cd bare
/bare$ git branch -a
* master
/bare$ cd ..
$ git clone test non-bare
Initialized empty Git repository in /home/derek/Projects/non-bare/.git/
$ cd non-bare
/non-bare$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

From the manual page for git clone --bare:

Also the branch heads at the remote are copied directly to corresponding local branch heads, without mapping them to refs/remotes/origin/. When this option is used, neither remote-tracking branches nor the related configuration variables are created.

Presumably, when it creates a bare repository, Git assumes that the bare repository will serve as the origin repository for several remote users, so it does not create the default remote origin. What this means is that basic git pull and git push operations won't work since Git assumes that without a workspace, you don't intend to commit any changes to the bare repository:

$ git push
fatal: No destination configured to push to.
$ git pull
fatal: /usr/lib/git-core/git-pull cannot be used without a working tree.
$ 

Bongkyoui-MacBook-Pro:parent bongkyo$ git push
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 391 bytes | 391.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: error: refusing to update checked out branch: refs/heads/master
remote: error: By default, updating the current branch in a non-bare repository
remote: is denied, because it will make the index and work tree inconsistent
remote: with what you pushed, and will require 'git reset --hard' to match
remote: the work tree to HEAD.
remote: 
remote: You can set the 'receive.denyCurrentBranch' configuration variable
remote: to 'ignore' or 'warn' in the remote repository to allow pushing into
remote: its current branch; however, this is not recommended unless you
remote: arranged to update its work tree to match what you pushed in some
remote: other way.
remote: 
remote: To squelch this message and still keep the default behaviour, set
remote: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
To /Users/bongkyo/Documents/tmp/remote-pc/submodule/parent/
 ! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to '/Users/bongkyo/Documents/tmp/remote-pc/submodule/parent/'



You can simply convert your remote repository to bare repository (there is no working copy in the bare repository - the folder contains only the actual repository data).

Execute the following command in your remote repository folder:

git config --bool core.bare true
Then delete all the files except .git in that folder. And then you will be able to perform git push to the remote repository without any errors.