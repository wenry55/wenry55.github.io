# Git Rebase

git rebase는 현재의 HEAD가 가리키는 커밋을 다른 커밋으로 변경하겠다는 뜻입니다.

![rebase](https://wac-cdn.atlassian.com/dam/jcr:e4a40899-636b-4988-9774-eaa8a440575b/02.svg?cdnVersion=861)

위의 그림을 보면 현재 Feature를 개발중에 있다가 master가 계속 업데이트 되어 녹색 commit 까지 갔습니다. Feature의 개발이 끝났을 경우, 이를 master 에 머지하는 방법은 
pull/merge/push 하는 방법이 있습니다. 하지만, 만약 Feature의 개발이 master브랜치가 한참 진행된 이후에 완성되어 merge할 경우, pull/merge/push는 git history를 로컬에 다 가져오게 됩니다.

참고) 
[Git Reset](./2020-03-05-git-reset.md)