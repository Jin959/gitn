# Git

- 생활코딩 (egoing) 강의를 들으며 필기한 내용.    
- [Gitn 강의](https://opentutorials.org/course/3838)
1. [Git CLI 버전 관리](#1-git-cli-버전-관리)
    - [레파지토리 생성 및 삭제](#레파지토리-생성-및-삭제)
    - [commit](#commit)
    - [log](#log)
    - [되돌리기](#되돌리기)
    - [상태들](상태들)
    - [폴더 및 파일](#폴더-및-파일)
2. [GIT CLI - Branch & Conflict](#2-git-cli---branch--conflict)
    - [Branch](#branch)
    - [Merge](#merge)
3. [GIT CLI - Backup](#3-git-cli---backup)
    - [git push (hosting)](#git-push-hosting)
    - [git clone](#git-clone)
    - [git pull](#git-pull)
4. [GIT CLI - 협업](#4-git-cli---협업)
    - [협업(push & pull)](#협업push--pull)
    - [pull VS fetch](#pull-vs-fetch)
5. [GIT - CLI cherry-pick & rebase](#5-git---cli-cherry-pick--rebase)
    - [cherry-pick](#cherry-pick)
    - [rebase](#rebase)

[추가 학습](#추가-학습)
- [Remote Branch](#remote-branch)
    - [로컬에서 branch 생성시 연동](#로컬에서-branch-생성시-연동)
    - [원격 저장소를 로컬에 git clone 할때 연동](#원격-저장소를-로컬에-git-clone-할-경우-연동)
- [SSH 연결](#ssh-연결)
- [깃허브 repositoy 이름 변경](#깃허브-repositoy-이름-변경)

---
# Gitn

## 1. [Git CLI 버전 관리](https://opentutorials.org/course/3839)

### 레파지토리 생성 및 삭제

(diretory)에 리파지토리를 초기화 하여 레파지토리 생성
```
git init (diretory)
```
예를 들어 아래는 현재 디렉토리에 initialize
```
git init .
```


레파지토리 삭제  
```
rm -fr .git
```
- -f : forced  
- -r : recursive  


현재 작업중인 repository가 어떤 상태인지 알려준다.
```
git status
```

### commit
stage에 올린다. add to staging area  
```
git add
```
파일들을 add하여 stage에 올린다.
```
git add (파일이름1) (파일이름2) ...
```
현재 폴더 전부 add
```
git add .
git add *
```
해당 폴더 전부 add
```
git add (폴더)
```

stage 에 올리고 커밋한다. commit the version to repository
```
git commit
```
메세지 입력
```
git commit -m (메세지)
```
add 까지 한번에 수행하여 commit 한다. 단, 한번이라도 add를 수행하여 tracked 상태여야 수행한다.
```
git commit -am (메세지)
```
다시 커밋하고 싶으면 파일 수정 작업을 하고 Staging Area에 추가(add)한 다음 `--amend` 옵션을 사용하여 커밋을 재작성 할 수 있다.
```
git commit --amend
```

### log
commit 기록, 'q'버튼으로 종료
```
git log
```
상태도 표시
```
git log --stat
```
patch, 더욱 상세하게 어떻게 수정되었는지 표시  
```
git log -p
```
- `--all` : 모든 브랜치에 대해  
- `--graph` : 시각화  
- `--oneline` : 한줄요약  


마지막 commit한 버젼과 working tree사이에 차이점을 파악할 수 있다. 버젼을 만들기 전에 뭐했는지 최종적으로 검토를 할 수 있다.
```
git diff
```
### 되돌리기
버전을 되돌린다.  
이때, working tree를 마지막 commit 버젼으로 리셋한다.   
```
git reset --hard
```
해당 (commit id) 버전으로 리셋 하겠다.  
```
$ git reset --hard (commit id)
```
reset 옵션의 `--hard` 옵션은 현재 하고있는 working tree의 작업까지 모두 지운다.  
reset은 보통 `(commit id)`를 인자로 사용하지만 `(branch name)`을 인자로 실행할 수도 있다. (대상 branch)가 가리키고 있던 commit을 원래의 branch가 가리키도록 하는 것이다.  
옵션 별로 reset 정도가 다르다.
- `--mixed` : 기본 옵션으로 현재 작업은 유지하고 commit 만 되돌린다.
- `--soft` : 현재 작업을 stage 에 올리 상태로 돌리고 commit 을 되돌린다.  


더 자세한 옵션을 보려면 `--help`
```
git reset --help
```


그 커밋 이전 버젼으로 복원하고 복원하는 과정을 다시 커밋으로 추가한다.
삭제의 목적과 보전의 목적을 모두 수행하기위해 사용한다. (commit id)에서의 변화를 취소한다. 즉, 타겟 커밋 작업의 변화량만큼만 취소하는 커밋을 한다.
```
git revert (commit id)
```
- cf) revert는 반드시 차근차근 역순으로 수행해야 한다.  

   
`head`가 `commit id`를 가리키도록 한다. 버전 기록은 그대로 유지한 채, `commit id` 로 이동만 한다.
```
git checkout (commit id)
```
이렇게 `head`가 `branch`를 가리키지 않는 상황을 `detached`상태라고 한다.  
- $cf)$ `git checkout master`는 가장 최근(앞)의 버젼으로 이동한다. `head`를 `master`로 이동시킨다.  

### 상태들
|#|상태|의미|
|:---:|----|:----|
| 1 |**Working Tree** | 아직 version 으로 만들어지기 전 단계이고 Stage에도 올라가지 않은 상태, 그냥 작업만 한 것들이 이 상태이다. |
| 2 |**Staging Area** | 복수의 파일 중 원하는 파일들을 선택하여 버젼으로 만들고 싶을 때 Staging Area에 선택한 파일들을 올려둔다. 그 다음 commit하면 동시에 commit된다.|
| 3 |**Repository** | version 이 저장되는 곳 (.git)|


### 폴더 및 파일

- `.git 디렉토리`에 version 정보들이 기록된다.  
- `.gitignore 파일`는 버젼관리를 하지말아야 할 파일들, 필요에 따라 임시파일을 만들지 않게하거나, 비밀번호나 개인정보를 올리고 싶지 않을 때  

## 2. [GIT CLI - Branch & Conflict](https://opentutorials.org/course/3840)
### Branch
브랜치 목록을 본다.
```
git branch
```
현재 branch name을 main으로 변경한다. git CLI에서는 기본 브랜치 명이 ```master``` 이고 github에서의 기본 브랜치 명은 ```main```이다.
```
git branch -M main
```

(brach name)으로 브랜치를 생성한다.
```
git branch (brach name)
git checkout -b (brach name)
```
현재 브랜치에서 나와 `(branch name)`으로 이동한다. `head`가 `(branch name)`을 가리키도록 한다.  
```
git checkout (branch name)
```
 


### Merge
- 서로 갈라졌던 Branch를 합치는 것을 병합(Merge)라고 한다. (merge commit)  
- 서로 다른 Branch에서 공통의 조상을 Base라고 한다.  

branch1에서 -> branch2으로 병합하려면, 일단 ```HEAD -> branch2``` 가 되도록 하고 병합하고 싶은 브랜치(branch1)을 지정하면 된다. 즉, branch2에서 branch1을 당겨 오는 것.
```
git merge branch1
```
 
- 중요 : 각각의 branch에서, 같은 파일의 다른 부분을 수정하고 commit한 뒤, 병합하면 각각의 수정사항이 모두 병합된다.  
그러나, 같은 파일의 같은 부분을 수정하고 commit한 뒤, 병합하면 Git은 자동으로 병합하지 못한다.  
 이를 **충돌(conflict)** 이라고 한다.  
 충돌이 일어난 부분에 아래와 같이 되고 이 부분을 수정후 `$ git commit`하면 충돌을 해결한다.
  ```
  <<<<< HEAD
  ....(HEAD에서의 내용)
  ======
  ....(당겨온 branch에서의 내용)
  >>>>> (branch name)
  ```
- 3 way merge : 하나의 파일에서 base(공통의 조상)과 비교했을 때 다른 브랜치에서 수정된 쪽으로 병합된다.
 
- detached : `head`가 `(branch name)`을 가리키도록 하지않고 `head`가 `commit id`를 가리키도록 할 때가 있다. 이렇게 `head`가 `branch`를 가리키지 않는 상황을 `detached`상태라고 한다.  

## 3. [GIT CLI - Backup](https://opentutorials.org/course/3841)
### git push (hosting)
원격 저장소 정보확인
```
git remote
```
- `-v ` : 원격 저장소의 상태와 주소도 표시  

지역 저장소(로컬 저장소, 내 컴퓨터에서 사용 중인 저장소)와 원격 저장소를 연결한다.
```
git remote add (원격 저장소 별명) (원격 저장소 주소)
```
원격 저장소와의 연결을 끊는다.  
```
git remote remove (원격 저장소 별명)
```


여러 개의 원격 저장소와 연결 될 수 있는데 어떤 원격 저장소와 기본적으로 연결 시킬 것인지 설정한다. 즉, 앞으로 origin에 master라는 브랜치로 기본적으로 push(업로드)한다. 최초에 한번만 하면된다.
로컬 가지가 원격가지를 트래킹하도록 설정하는 것이기도 하다.
```
git push [-u | --set-upstream] origin master
```
한번 만 하면 앞으로는 트래킹 된다.
```
git push
```


### git clone
원격 저장소를 복제해서 지역 저장소를 만든다.
```
git clone (원격 저장소 주소 https::/)
```
기본적으로 원격저장소 폴더명으로 저장소를 생성한다. 뒤에 폴더명을 지정하면 그 폴더명으로 생성시킨다.

### git pull
현재 저장소의 최신 commit을 가져온다.
```
git pull
```

## 4. [GIT CLI - 협업](https://opentutorials.org/course/3842)
### 협업(push & pull)
* github에서 private이건 public이건 settings-collaborators에 추가하여야지 push를 할 수 있다. 초대받으면 메일을 받게 되고 Accept invitation을 해야한다.    
  
1. 한사람 A가 `작업 --> commit --> push` 를 했는데
2. 다른 B가 `pull --> 작업 --> commit --> push`가 아니라 `작업 --> commit --> push`을 했다면
3. **rejected** 당하고 git은 `$ git pull`을 하라고 한다.
4. 하지만 같은 파일의 같은 라인을 수정한 경우, **conflict**가 일어난다.
5. 이때 B가 **conflict**가 생긴 부분을 수정, 해결한 뒤 `commit --> push`하면 merge된다.
6. 이후, A는 나중에 `pull`하여 가져와서 `$ git log --graph`를 보면 A는 이전에 B가 `merge`한 작업을 보게 된다.
 
* 우리는 위의 B와 달리 항상 `pull`을 하도록한다. 다른 사람이 업데이트 했는지 확인하는 것이 좋은 습관이다.

### pull VS fetch
pull 은 fetch 후 merge 한 것과 같다.
```
git pull
```
위의 명령은 다음 명령과 같다.
```
git fetch; git merge FETCH_HEAD;
```

원격 가지 정보를 가져온다. 아직 업데이트 하진 않는다.
```
git fetch
```
`git pull`과 다르다. 원격 저장소가 가지고 있는 `commit`들을 로컬 저장소로 update해준다. merge는 되지 않는다.  
이때, HEAD는 fetch 이전의 commit을 그대로 가리킨다.  
즉, 다음 두 명령은 같다.
```  
git pull
```
```
git fetch; git merge master/origin(원격저장소 이름)
```
내부적으로는 다음과 같이 동작한다.  
`fetch`를 할 때 마다 `.git/FETCH_HEAD`파일에 가장 최상위 `commit id`을 표시한다.  
따라서, 가장 최근의 `commit`으로 `HEAD`를 돌리기 위해 항상 `master/origin`와 `merge`할 필요 없이 아래와 같이 하면 된다.  
```
git fetch; git merge FETCH_HEAD;
```
 
- ∴ 신중하게 pull을 하고 싶으면 fetch 를 사용한 뒤 나중에 merge 하면 된다.

## 5. [GIT - CLI cherry-pick & rebase](https://opentutorials.org/course/3843/24443)

- cherry-pick, rebase, revert 들은 다른 가지에서 생성된 "커밋 혹은 커밋 들의 전체 워킹 디렉토리의 스냅샷을 가져오는 것이 아니라 "선택 커밋의 변화량"만 가져온다.

### cherry-pick
- 체리를 가져오듯이 특정 커밋만 가져오겠어
- 브랜치의 분기가 나뉘었는데 다른 브랜치의 특정 커밋에서 변화된 것을 가져오는 것
- 타겟 커밋이 그대로 복제되어 현제 브랜치에 커밋까지 된다.
- 가져오려는 커밋이 있는 브랜치가 아니라 병합하려는 타겟 브랜치로 이동한뒤 cherry-pick 한다.
    ```
    git cherry-pick 3b16f55
    ```

### rebase
- cherry-pick 의 연쇄작용이다.
- 두 가지의 공통 조상인 커밋 이후 모든 커밋을 옮길 가지로 가져온다.
- 공통 조상 이후부터 master 가지의 모든 커밋을 topic 으로 가져오고 싶은 경우 master 브랜치로 이동하고 다음을 실행한다.
    ```bash
    # topic 가지에 master 의 커밋 기록이 얹어진다.
    git rebase topic
    ```
- 공통조상 이후의 모든 커밋들은 순차적으로 옮겨진다. 즉, 동일한 커밋을 옮기더라도 옮겨진 베이스의 상황에 따라 리베이스 커밋의 변경사항 결과는 달라질 수 있다. -> test.txt 를 생성했다가 revert 하는 커밋이 뗴어서 옮길 브랜치에 있다고 하자. 현재 파일 상황에 test.txt 있다면 test.txt 를 생성하는 커밋은 무시되고 revert 하는 커밋만 적용된다. 그래서 파일상황이 그대로 옮겨지는 것은 아니다.

- 원격 저장소에 push 된 가지와 커밋은 rebase 하면 안된다. 하나씩 cherry-pick 을 하는 것과 같기 때문에 워킹 디렉터리 스냅샷은 완전히 다르다. 따라서 원격에 푸시된 가지를 리베이스 하면 엉망이 된다.

- 리베이스 후에 머지하여 HEAD 당기기, 아래의 경우 master 로 이동했다.
    ```
    * 543bcf7 (origin/rebase, rebase) t3
    * df35c13 t2
    * 7e33da2 t1
    * a9a245b (HEAD -> master, origin/master) m2
    * fabd9c4 m1
    * a635817 rebase 실습을 위한 초기화
    ```
    이런 상황에서 머지한다.
    ```
    git merge rebase
    ```

---

# 추가 학습

## Remote Branch

<https://git-scm.com/book/ko/v2/Git-브랜치-리모트-브랜치>

- Refs : `refs/heads/master`, `HEAD` 와 같은 것들이다.

  <https://git-scm.com/book/ko/v2/Git의-내부-Git-Refs>

- `origin/<브랜치 이름>` : 리모트 트래킹 브랜치라고 부른다. Refs 대신 더 자주 사용한다.

- `$ git remote show [원격저장소 이름]` : 모든 리모트 브랜치 정보를 볼 수 있다.

- `$ git branch -vv` : 트래킹 되고 있는 원격 저장소의 브랜치를 로컬 브랜치 별로 확인 할 수 있다.

- `$ git fetch orgin` 를 수행하게 되면 원격 저장소(origin)의 리모트 트래킹 브랜치들과 이 것들이 가리키는 로컬에는 없는 커밋들을 가져오지만 다른 브랜치로 가져오고 merge 되지 않는다.

- `$ git branch -u [리모트 저장소 별칭, origin]/[리모트 브랜치 이름]` : 로컬의 브랜치가 리모트 브랜치를 추적하게 하게한다.

### 로컬에서 branch 생성시 연동

- 기존 로컬에서 branch 생성하고나서 원격 저장소와 브랜치를 맞추려면 아래와 같이 수행해야 된다.

  참고  

  <https://moding.tistory.com/entry/Git-원격remote-브랜치-생성-및-local-브랜치-연동>

  ```bash
  # 아래와 원격저장소인 origin에 로컬 브랜치 추가
  $ git push origin feature/JR-25 

  # 아래와 같이 연동해둔다.
  # 해당 브랜치로 이동후
  $ git branch --set-upstream-to origin/feature/JR-25 
  # 또는
  $ git branch --set-upstream-to origin/feature/JR-25 feature/JR-25  

  # 또는 아래와 같이 원격 브랜치 생성 및 연동을 한번에 수행할 수 있다.
  $ git push -u origin feature/JR-25
  ```

### 원격 저장소를 로컬에 git clone 할 경우 연동

- 원격 저장소를 로컬에 git clone 후 branch 연동하려면 

- 아무것도 모를 때, 원격과 로컬의 branch를 연동을 안하고 origin에 master를 계속 push 해왔었는데 branch를 생성시킨 기록이 모두 원격 저장소에 push 되기는 한다. clone을 해왔는데 branch와 merge 기록이 모두 clone 되었다.

- 그런데, 각 branch 마다 연결될 원격 저장소의 branch를 따로 재설정 해주어야 했다.

  <https://moding.tistory.com/entry/Git-원격remote-브랜치-생성-및-local-브랜치-연동>

  ```bash
  # 아래와 같이 연동해둔다.
  $ git branch --set-upstream-to origin/<브랜치 이름> 
  $ git branch --set-upstream-to origin/<브랜치 이름> <로컬 브랜치 이름>  
  ```


- 그리고, clone으로 처음 가져왔을 때, log를 보면 ```origin/HEAD``` 가 생기는데, 이는 원격 저장소의 HEAD  없애고 싶으면 다음과 같이 제거 해준다.

  참고

  <https://devbirdfeet.tistory.com/180>

  ```
  remote set-head origin -d
  ```
- 읽어보기 좋은 글

  - [git 브랜치 전략에 대해서](https://tecoble.techcourse.co.kr/post/2021-07-15-git-branch/) - 우아한 테크코스 3기_샐리

  - [브랜치 전략 수립을 위한 전문가의 조언들](http://blog.hwahae.co.kr/all/tech/tech-tech/9507/) - 화해 


## SSH 연결

1. `ssh-keygen` 으로 키 생성

2. '/c/users/<유저명>/.ssh'에 생긴다. 공개키('*.pub') 내용을 github 설정에 추가한다.

3. ssh agent에 키를 등록한다. eval을 사용하지 않았을 때 잘 등록 되지 않았다.

    ```shell
    $ eval `ssh-add /c/users/<유저명>/.ssh/개인키파일`
    ```

4. 키가 잘 등록 됐는지 확인 할 수 있는 것이 몇가지 있는데 그 중 하나가 아래 명령이다. 
  키 정보가 반환되면 성공이다.

    ```shell
    $ ssh-add -l
    ```

5. 그 다음 원격 저장소와 연결

    ```
    $ git remote add origin git@github.com:[ssh연결경로]
    ```

- $cf)$ 윈도우에서 ssh-agent에 계속 등록해야하는 상황이 생기는데, `.bashrc`파일에 실행문을 추가해줘야 자동 실행 된다.  
  github에서 공식적으로 guide를 제시해준다.  
  <https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows>


- $cf)$ 윈도우라서 안되는 게 아닐 때는 다른 해결법도 있다.

  <https://gentlesark.tistory.com/153>

  <https://stackoverflow.com/questions/46661525/permanently-add-an-ssh-key>

  <https://docs.github.com/ko/developers/overview/using-ssh-agent-forwarding>

### References

- <https://velog.io/@igotoo/github-접속을-https에서-ssh-접속으로-변경하기>

## 깃허브 repositoy 이름 변경

깃허브 레포에서 settings 에서 rename 한 뒤에 로컬에서도 레포명 변경 전의 URL 에서 새로운 URL로 변경해주는 것이 좋다.

- [깃허브 공식 문서](https://docs.github.com/ko/repositories/creating-and-managing-repositories/renaming-a-repository)

```
git remote set-url origin NEW_URL
```