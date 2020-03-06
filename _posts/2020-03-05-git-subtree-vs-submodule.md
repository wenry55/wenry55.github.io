---
layout: default
title: Git Subtree VS Submodule
categories: [git]
tags: [git, subtree, submodule]
---

##### subtree or submodule?

subtree와 submodule은 각각 언제 사용하는 것이 좋을까요?
두개중에 어느것이 좋다라고는 생각하지 않는 것이 좋겠습니다.
프로젝트 규모와 방식, 환경에 따라 적당한 것을 사용하면 됩니다.
사실, 그렇게 하려면 위의 두가지 방식에 대해서 정확히 알고 구분 지을 수 있어야 할 겁니다.

그럼, [subtree](/git/git-subtree)/[submodule](/git/git-submodule)에 대하여 각각 이해하였다고 생각하고 이들의 차이점을 한번더 생각해 보겠습니다.

우선 subtree는 짜집기 느낌이 강합니다. 왜냐하면 리모트를 모두 로컬 repository에 등록해 두어야 하고, 이렇게 등록된 remote를 소스트리의 부분부분에 subdirectory로 할당하는 방식이기 때문입니다.

반면 submodule은 로컬 repository에 submodule로 사용할 remote를 등록하지 않습니다. 해당 디렉토리에 각각 pull을 합니다. 이 때 특정버전을 fetch & pull 합니다. 
