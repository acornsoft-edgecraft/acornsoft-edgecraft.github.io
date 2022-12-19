---
layout: doc
menu: GitHub with VScode
title: GitHub with VScode
category: 오픈소스 가이드
order: 8
---

- [Contributor Guide with Vscode](#contributor-guide-with-vscode)
- [Branch 전략](#branch-전략)
  - [Git-Flow](#git-flow)
  - [Summary](#summary)
- [Contributing](#-contributing)
- [Pull Request를 위한 전반적인 Flow](#pull-request를-위한-전반적인-flow)
- [(optionall) Install pugin for vscode](#optionall-install-pugin-for-vscode)
- [Pull Request Work Process](#pull-request-work-process)
  - [Fork & Clone](#fork--clone)
  - [개발 준비](#개발-준비)
  - [개발 완료후 repository 동기화(fetch/rebase)](#개발-완료후-repository-동기화fetchrebase)
    - [1. 로컬 개발 완료후 origin(forked) repository의 feature/featureA branch에 commit/push 한다.](#1-로컬-개발-완료후-originforked-repository의-featurefeaturea-branch에-commitpush-한다)
    - [2. (optional) 개발 완료후 commit내용 합치는 작업을 한다(Squash)](#2-optional-개발-완료후-commit내용-합치는-작업을-한다squash)
    - [3. remote(upstream) repository와 동기화한다.](#3-remoteupstream-repository와-동기화한다)
  - [소스 동기화 완료후 Pull Request 생성 - Vscode](#소스-동기화-완료후-pull-request-생성---vscode)
  - [리뷰 승인 확인 및 feature branch 삭제](#리뷰-승인-확인-및-feature-branch-삭제)

<br/>
<br/>

# GitHub Contribution Guide with Vscode

---

Vscode를 사용한 Github Contribution 방법 소개  
<br/>
<br/>

## Branch 전략

---

### Git-Flow

![Basic Git Flow For Making Open Source Contributions on GitHub](https://dnncommunity.org/DesktopModules/Blog/BlogImage.ashx?TabId=65&ModuleId=454&Blog=1&Post=1470&w=1140&h=400&c=0&key=289a2e46-efbd-471c-830d-ccfdd93d46ea)

- [Basic Git Flow For Making Open Source Contributions on GitHub](https://dnncommunity.org/blogs/Post/1470/Basic-Git-Flow-For-Making-Open-Source-Contributions-on-GitHub)
- [우린 Git-flow를 사용하고 있어요](https://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)

### Summary

- 개발은 원본(kore3lab/kore-on) repository를 fork 받아 로컬 repository 에서 수행하고 "Pull Reuqest"를 통해 리뷰 프로세스 수행
- 리뷰가 완료되면 원본(kore3lab/kore-on) repository의 branch에 merge 하여 반영
- Feature 개발 : **develop** branch에서 로컬 branch를 생성&개발 후 원본 repository의 **develop** branch에 merge
- Hotfix 개발 : **master** branch에서 로컬 branch를 생성&개발 후 원본 repository의 **develop** 와 **master** branch에 merge
- Release 개발 : **develop** branch에서 로컬 branch를 생성&개발 후 원본 repository의 **masater** 와 **develop** branch에 merge
- Release/Hotfix 완료 후 태깅처리하여 릴리즈 출시
- 태깅 및 **master** branch로의 merge 작업은 관리자(owner) 권한 사용자가 수행
  <br/>
  <br/>

## Contributing

---

## Pull Request를 위한 전반적인 Flow

---

![Pull Request 를 위한 전반적인 Flow](/images/pull-request-flow.png)

1. Pull Requests를 올리고자 하는 repository를 fork 한다.
2. fork한 repository 를 Local PC 에 git clone 한다.
3. 수정 commit을 fork한 repository에 push 한다.
4. push한 commit을 원본 repository 에 pull requests 한다.  
   <br/>
   <br/>

## (optionall) Install plugin for vscode

---

Vscode에서 사용하는 Git 관련 플러그인들은 아래와 같이 제공된다.

- GitLens
- Git Graph
- GitHub Pull Requests and Issues  
  <br/>
  <br/>

## Pull Request Work Process

---

```sh
1. 원본 저장소를 Fork & Clone
2. 개발 준비
3. 원본 repository 동기화(fetch/rebase)
4. pull request 요청
```

<br/>

### Fork & Clone

1. 원본 Repository([kore3lab/kore-on](https://github.com/kore3lab/kore-on)) 의 우측 상단 "Fork" 버튼을 눌러 fork repository(girhub-user/kore-on) 생성.

![Repository Fork](/images/github-fork.png)

2. vscode에 fork된 repository를 연동(git clone) 한다.

```sh
$ git clone https://github.com/<forked-user>/kore-on.git
$ git remote -v
origin  https://github.com/<forked-user>/kore-on.git (fetch)
origin  https://github.com/<forked-user>/kore-on.git (push)
```

![github clone](/images/github-clone.png)

3. 원격 저장소(프로젝트의 원래 저장소)를 설정한다.

```sh
$ git remote add upstream https://github.com/kore3lab/kore-on.git
$ git remote -v
origin  https://github.com/xxxxx/kore-on.git (fetch)
origin  https://github.com/xxxxx/kore-on.git (push)
upstream        https://github.com/kore3lab/kore-on.git (fetch)
upstream        https://github.com/kore3lab/kore-on.git (push)
```

![원격 저장소 추가](/images/github-remote-repository-add.png)

### 개발 준비

1. develop branch로 이동

```sh
$ git checkout develop
$ git branch --list

* develop
  master
```

<br/>

2. remoet upstream(원본) develop branch와 동기화(fetch/rebase)  
   개발 전에 origin develop branch를 최신 소스로 동기화한다.

```sh
$ git branch --list

* develop
  master

$ git fetch upstream
$ git rebase upstream/develop
$ git push
```

<br/>
3) 작업할 feature 브랜치를 생성하고, 해당 브랜치로 이동한다.
  - 로컬에서 feature branch 생성: git plugin -> 분기 만들기 -> 분기 이름(예: feature/feature name)
  - remote origin(forked) repository에 feature branch 게시: 로컬에 생성된 Branch를 github에 생성하기 위해서는 분기 게시를 한다.
    ```sh
    $ git branch --list

    * develop
      master

    $ git switch -c feature/haproxy
    ```

![Create local branch](/images/create-branch.png)

**[참고]** 2번 3번 항목을 vscode의 gitlens를 사용해서 할 수 있다.

- fectch
  ![1. fectch](/images/vs_code_gitlens_fecth.png)
- switch branch
  ![2. switch-1](/images/vs_code_gitlens_switch-1.png)
- feature branch name
  ![2. switch-1](/images/vs_code_gitlens_switch-2.png)
- local branch 생성
  ![2. switch-1](/images/vs_code_gitlens_switch-3.png)
- local branch up to remote(origin) repository
  ![2. switch-1](/images/vs_code_gitlens_switch-4.png)
- remote(origin) repository 선택
  ![2. switch-1](/images/vs_code_gitlens_switch-5.png)
- remote brantch 생성 확인
  ![2. switch-1](/images/vs_code_gitlens_switch-6.png)

### 개발 완료후 repository 동기화(fetch/rebase)

로컬에서 개발이 완료되면 origin(forked) repository와 upstream repository를 동기화 시킨다.  
전반적인 flow는 아래와 같다.

```sh
1. 로컬 개발 완료후 origin(forked) repository 동기화 한다. - commit/push
2. 개발 완료후 commit내용 합치는 작업을 한다(Squash). - optional
3. remote(upstream) repository 동기화 한다. - fetch/rebase
4. rebase할때 병합 충돌이 있다면 해결한다. - rebase --continue
```

<br/>

##### 1. 로컬 개발 완료후 origin(forked) repository의 feature/featureA branch에 commit/push 한다.

```sh
$ git branch
  develop
* feature/contribution
  master

$ git commit -m "commit messages.."
$ git push
```

<br/>

**(또는)** vscode에서 commit/push 한다.  
![변경 내용 동기화](/images/vs_code_sync_origin.png)

##### 2. (optional) 개발 완료후 commit내용 합치는 작업을 한다(Squash)

오픈 소스나, 여러 사람이 같이 작업하는 소스코드인 경우, 커밋 이력이 많아지고 복잡해져서, 커밋 이력을 추적하는 것이 힘들어지게 됩니다.
Git의 Squash 기능은 이것을 방지하기 위해 여러번 커밋한 이력을 하나의 커밋 이력으로 만드는데 사용합니다.  
정확히 이야기해서 **git squash** 라는 명령어는 없습니다. **interactive rebase**를 하는데 필요한 명령어 중 하나입니다.  
[커밋 합치기(squash)](https://meetup.toast.com/posts/39) 섹션 또는, [VS Code에서 Git 스쿼시 커밋](https://dannyherran.com/2020/06/git-squash-commit-vs-code/) 섹션을 참고 한다.

```sh
$ git log --pretty=oneline
d442427eae836f15e94f5df0445c70081df79a3e Task 3/3
26395437be53e4e6e68f83aa98560ef93838aaa0 Task 2/3
7c6535580a038e9dcfaa72a98e04848812da9aee Task 1/3
2260a88777c247c31170ff6074d95569ac557afb Initial commit

## 최근 3개의 커밋을 interactive rebase 한다. - vi 에디터에서 편집
$ git rebase -i HEAD~3

## 확인
$ git log --pretty=oneline
9833ca676c5a24361c1cc36fb173746328dfac3a Task 1/3 ~ 3/3
2260a88777c247c31170ff6074d95569ac557afb Initial commit
```

<br/>

**(또는)** VS Code에서 Git 스쿼시 한다. (GitGraph plugin 사용)

- GitGraph 로그로 이동하여 유지하려는 커밋보다 이전 커밋을 마우스 오른쪽 버튼으로 클릭합니다. 예를 들어 모든 커밋을 빨간색으로 스쿼시하려면 녹색 커밋을 마우스 오른쪽 버튼으로 클릭한 다음 "현재 분기를 이 커밋으로 재설정..."을 선택합니다.
  ![VS Code에서 Git 스쿼시 커밋_1](/images/vs_code_squashing_1.png)

- 그러면 왼쪽에 이전에 있었던 모든 커밋에 속하는 단계적 변경 사항이 표시됩니다. "모두 커밋 ..."으로 진행하십시오.
  ![vs_code_squashing_commit_all](/images/vs_code_squashing_commit_all.png)

- 마지막으로 원본 커밋으로 강제 푸시를 수행하여 기존 커밋을 모두 단일 커밋으로 교체합니다.
  ![vs_code_squashing_force_push](/images/vs_code_squashing_force_push.png)

**(참고)** vi 에디터 대신 VS Code에서 [Gitlens Interactive Rebase Editor](https://github.com/gitkraken/vscode-gitlens#interactive-rebase-editor-) 사용 하기

- VS Code를 기본 Git 편집기로 설정
  - ```
     $ git config --global core.editor "code --wait"
    ```
- 또는 rebase에만 영향을 미치려면 VS Code를 Git rebase 편집기로 설정하십시오.
  - ````
         $ git config --global sequence.editor "code --wait"
        ```
    ![Gitlens Interactive Rebase Editor ](/images/gitlens-interactive-rebase-editor.png)
    ````

##### 3. remote(upstream) repository와 동기화한다.

fetch/rebase를 사용해서 fork한 저장소를 최신 원본과 동기화한다(commit base를 재구성).  
[git rebase를 이해하기](https://junwoo45.github.io/2019-10-23-rebase/)를 참고.

```sh
$ $ git branch
  develop
* feature/contribution
  master

## 1. 원본(upstream)을 fetch 한다.
$ git fetch upstream

## 2. 로컬 브렌치(feature/featureA)에 upstream/develop의 commit내용을 rebase한다. - .git 디렉토리에 저장 된다.
$ git rebase upstream/develop

## 3. 이때 병합 충돌이 나면 충돌을 해결하고 base를 재구성한다.
$ git rebase --continue

## fork한 저장소에 push 한다.
$ git push
```

<br/>

**(참고)** 병합 충돌 해결 절차 - Vscode

```sh
1. Vscode의 병합 수정 페이지에서 충돌을 해결한다.
2. 변경 사항을 스테이징 한다.
3. rebase --continue 한다.
4. 변경 사항을 확인한다.
```

<br/>

- 원본(upstream)을 fetch 한다.
  ![upstream repository rebase](/images/vs_code_rebase_upstream-0.png)
  <br/>
- 로컬 브렌치(feature/featureA)에 upstream/develop의 commit내용을 rebase한다.
  - rebase 항목이 있으면 선택창이 활성화 된다. 여기에서도 Interative Rebase를 선택하면 커밋로그를 합칠수 있다(optional).
    ![upstream repository rebase](/images/vs_code_rebase_upstream-1.png)
    ![upstream repository rebase](/images/vs_code_rebase_upstream-2.png)
    <br/>
- 병합 충돌이 있으면 해결해야 한다.
  - 충돌 소스를 수정 페이지에서 수정한다.
    - Accept Current Change는 upstream 저장소 내용으로 변경된다.
    - Accept Incoming Change는 origin 저장소 내용으로 변경된다.
  - 변경된 내용을 스테이징 한다.
    - 변경된 내용이 스테이징된 변경 사항으로 활성화 된다.
  - rebase중인 항목을 완료 해야 한다.
    - 열린 편집기 파일을 close 하면 완료 처리가 된다.
    ```sh
    $ git rebase --continue
    [HEAD 분리됨 c566396] contribution 작성중
    8 files changed, 65 insertions(+), 9 deletions(-)
    create mode 100644 docs/images/vs_code_gitlens_fecth.png
    create mode 100644 docs/images/vs_code_gitlens_switch-1.png
    create mode 100644 docs/images/vs_code_gitlens_switch-2.png
    create mode 100644 docs/images/vs_code_gitlens_switch-3.png
    create mode 100644 docs/images/vs_code_gitlens_switch-4.png
    create mode 100644 docs/images/vs_code_gitlens_switch-5.png
    create mode 100644 docs/images/vs_code_gitlens_switch-6.png
    힌트: 편집기가 파일을 닫기를 기다리는 중입니다...
    ```
  - Git Graph 플러그인을 사용해서 이력을 확인 할 수 있다.

![upstream repository rebase](/images/vs_code_rebase_upstream-3.png)
![upstream repository rebase](/images/vs_code_rebase_upstream-4.png)
![upstream repository rebase](/images/vs_code_rebase_upstream-5.png)
![upstream repository rebase](/images/vs_code_rebase_upstream-6.png)
![upstream repository rebase](/images/vs_code_rebase_upstream-7.png)
![upstream repository rebase](/images/vs_code_rebase_upstream-8.png)

### 소스 동기화 완료후 Pull Request 생성 - Vscode

remote(upstream) repository와 동기화가 완료되면 pull request를 생성 해서 reviewers 에게 확인/승인을 받는다.

```sh
1. github fork 저장소에서 생성 할 수 있다.
2. Vscode에서 GitHub Pull Requests and Issues 플러그인을 사용해서 할 수 있다.
```

<br/>

- Gitlens에서 요쳥
  ![vs_code_pr_create](/images/vs_code_pr_create-1.png)
  <br/>

- GitHub Built In 또는 Vscode Plugin 선택
  ![vs_code_pr_create](/images/vs_code_pr_create-2.png)
  <br/>

- pull request를 요청할 저장소/브렌치 지정
  ![vs_code_pr_create](/images/vs_code_pr_create-3.png)

### 리뷰 승인 확인 및 feature branch 삭제

1. 동료에게 리뷰 승인을 받은 후에는 로컬에서 자신의 feature branch를 삭제한다.

2. fork repository의 feature 브랜치들을 삭제한다. - optional
