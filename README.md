# Git
[Gitn](#gitn)
- 생활코딩 (egoing) 강의를 들으며 필기한 내용.    
- [Gitn 강의](https://opentutorials.org/course/3838)
1. [Git CLI 버전 관리](#1-git-cli-버전-관리)
    - [명령어](#명령어)
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

[추가 학습](#추가-학습)
- [Remote Branch](#remote-branch)
    - [로컬에서 branch 생성시 연동](#로컬에서-branch-생성시-연동)
    - [원격 저장소를 로컬에 git clone 할때 연동](#원격-저장소를-로컬에-git-clone-할-경우-연동)
- [SSH 연결](#ssh-연결)

---
# Gitn
## 1. [Git CLI 버전 관리](https://opentutorials.org/course/3839)
### 명령어
* ```$ git init (diretory)```: initialize repository, (diretory)에 리파지토리를 초기화 하여 생성    
  $ex)$ ```$ git init .``` : 현재 디렉토리에 initialize
  
* ```$ rm -fr .git``` : 레파지토리 삭제  
  $cf)$ -f : forced, -r : recursive

* ```$ git status``` : 현재 작업중인 repository가 어떤 상태인지 알려준다.
  
* ```$ git add``` : add to staging area   
  $ex)$ ```$ git add (파일이름1) (파일이름2) ...``` : 파일들을 add하여 stage에 올린다.   
  $ex)$ ```$ git add .``` or ```$ git add *``` : 현재 폴더 전부 add    
  $ex)$ ```$ git add (폴더)``` : 해당 폴더 전부 add    
   
* ```$ git commit``` : commit the version to repository, Stage에 올라온 버전을 commit, 옵션을 택하지않으면 편집기에서 commit message를 입력한 후 저장하면 커밋된다.    
  $cf)$ ```$ git commit -m (메세지)``` 로 메세지 입력   
  $cf)$ ```$ git commit -am (메세지)``` add까지 한번에 수행하여 commit한다. 단, 한번이라도 add를 수행하여 tracked상태여야 수행한다.  
  $cf)$ ```$ git commit --amend``` : 너무 일찍 커밋했거나 어떤 파일을 빼먹었을 때 그리고 커밋 메시지를 잘못 적었을 때 한다. 다시 커밋하고 싶으면 파일 수정 작업을 하고 Staging Area에 추가한 다음 --amend 옵션을 사용하여 커밋을 재작성 할 수 있다.  
   
* ```$ git log``` : commit 기록, 'q'버튼으로 종료     
  $cf)$ ```$ git log --stat``` : 상태도 표시   
  $cf)$ ```$ git log -p``` : patch, 더욱 상세하게 어떻게 수정되었는지 표시  
  $cf)$ ```--all``` : 모든 브랜치에 대해  
  $cf)$ ```--graph``` : 시각화  
  $cf)$ ```--oneline``` : 한줄요약  
   
* ```$ git diff``` : 마지막 commit한 버젼과 working tree사이에 차이점을 파악할 수 있다. 버젼을 만들기 전에 뭐했는지 최종적으로 검토를 할 수 있다.
   
* ```$ git reset --hard``` : working tree를 마지막 commit 버젼으로 리셋한다.   
  $cf)$ ```$ git reset --hard (commit id)``` : (commit id)로(이 버젼으로) 리셋 하겠다.   
  $cf)$ reset 옵션의 --hard 모드는 현재하고있는 working tree의 작업까지 모두 지운다. (```$ git reset --help``` 참고)  
  $cf)$ reset은 보통 ```(commit id)```를 인자로 사용하지만 ```(branch name)```을 인자로 실행 할 수 도 있다.  
  이 경우 ```(branch name)```을 인자로 실행하면 ```(branch name)```(대상 branch)가 가리키고 있던 commit을 원래의 branch(reset 실행 전 head가 가리키는 branch)가 가리키도록 하는 것이다.
 
* ```$ git revert (commit id)``` : 삭제의 목적과 보전의 목적을 모두 수행하기위해 사용한다. (commit id)에서의 변화를 취소한다. 즉, 그 커밋 이전 버젼으로 복원된다.    
  $cf)$ revert는 반드시 차근차근 역순으로 수행해야한다. 그렇지 않고 **"임의의 지점으로 한번에 점프하도록 revert명령을 시키면 충돌이 일어나 대단히 복잡해진다?"**     
   
* ```$ git checkout (commit id)``` : 버전을 ```commit id```였을 때로 돌린다. 즉, ```head```가 ```commit id```를 가리키도록 한다. 이렇게 ```head```가 ```branch```를 가리키지 않는 상황을 ```detached```상태라고 한다.  
  $cf)$ ```$ git checkout master``` : 가장 최근(앞)의 버젼으로 돌아간다. ```head```를 ```master```로 이동시킨다.  

### 상태들
|#|상태|의미|
|:---:|----|:----|
| 1 |<span style="color:black; background-color:yellow;">**Working Tree**</span> | 아직 version 으로 만들어지기 전 단계이고 Stage에도 올라가지 않은 상태, 그냥 작업만 한 것들이 이 상태이다. |
| 2 |<span style="color:black; background-color:yellow;">**Staging Area**</span> | 복수의 파일 중 원하는 파일들을 선택하여 버젼으로 만들고 싶을 때 Staging Area에 선택한 파일들을 올려둔다. 그 다음 commit하면 동시에 commit된다.|
| 3 |<span style="color:black; background-color:yellow;">**Repository**</span> | version 이 저장되는 곳 (.git)|


### 폴더 및 파일

* ```.git 디렉토리```에 version 정보들이 기록된다.
    
* ```.gitignore 파일```는 버젼관리를 하지말아야 할 파일들, 필요에 따라 임시파일을 만들지 않게하거나, 비밀번호나 개인정보를 올리고 싶지 않을 때

## 2. [GIT CLI - Branch & Conflict](https://opentutorials.org/course/3840)
### Branch
* ```$ git branch``` : 브랜치를 본다.  
  $cf)$ ```$ git branch -M main``` : 현재 branch name을 main으로 변경한다. git CLI에서는 기본 브랜치 명이 ```master``` 이고 github에서의 기본 브랜치 명은 ```main```이다.
 
* ``` git branch (brach name)``` : (brach name)으로 브랜치를 생성한다.  
 
* ```$ git checkout (branch name)``` : 현재 브랜치에서 나와 ```(branch name)```으로 이동한다. ```head```가 ```(branch name)```을 가리키도록 한다.  
 


### Merge
* 서로 갈라졌던 Branch를 합치는 것을 병합(Merge)라고 한다. (merge commit)  
 
* 서로 다른 Branch에서 공통의 조상을 Base라고 한다.  
 
* ```$ git merge branch1``` : branch1에서 -> branch2으로 병합하려면, 일단 ```HEAD -> branch2``` 가 되도록 하고 병합하고 싶은 브랜치(branch1)을 지정하면 된다. 즉, branch2에서 branch1을 당겨 오는 것.  
 
* <span style="color:black; background-color:yellow;">중요</span> : 각각의 branch에서, 같은 파일의 다른 부분을 수정하고 commit한 뒤, 병합하면 각각의 수정사항이 모두 병합된다.  
그러나, 같은 파일의 같은 부분을 수정하고 commit한 뒤, 병합하면 Git은 자동으로 병합하지 못한다.  
 이를 <span style="color:red">**충돌(conflict)**</span> 이라고한다.  
 충돌이 일어난 부분에 아래와 같이 되고 이 부분을 수정후 ```$ git commit```하면 충돌을 해결한다.
  ```
  <<<<< HEAD
  ....(HEAD에서의 내용)
  ======
  ....(당겨온 branch에서의 내용)
  >>>>> (branch name)
  ```
* <span style="color:black; background-color:yellow;">3 way merge</span> : 하나의 파일에서 base(공통의 조상)과 비교했을 때 다른 브랜치에서 수정된 쪽으로 병합된다.
 
* detached : ```head```가 ```(branch name)```을 가리키도록 하지않고 ```head```가 ```commit id```를 가리키도록 할 때가 있다. 이렇게 ```head```가 ```branch```를 가리키지 않는 상황을 ```detached```상태라고 한다.  

## 3. [GIT CLI - Backup](https://opentutorials.org/course/3841)
### git push (hosting)
* ```$ git remote``` : 원격 저장소가 있는지 확인한다.  
  $cf)$ ``` -v ``` : 원격 저장소의 상태와 주소도 표시  
  
* ```$ git remote add (원격 저장소 별명) (원격 저장소 주소)``` : 지역 저장소(로컬 저장소, 내 컴퓨터에서 사용 중인 저장소)와 원격 저장소를 연결한다. 이 때, ```(원격저장소 별명)```은 origin으로 사용하는 것이 관습화 되어있다.?  
  $cf)$ ```$ git remote remove (원격 저장소 별명)``` : 원격 저장소와의 연결을 끊는다.

* ```$ git push [-u | --set-upstream] origin master``` : 여러 개의 원격 저장소와 연결 될 수 있는데 어떤 원격 저장소와 기본적으로 연결 시킬 것인지 설정한다. 즉, 앞으로 origin에 master라는 브랜치로 기본적으로 push(업로드)한다. 최초에 한번만 하면된다.
* ```$ git push```만 하면 된다.

### git clone
* ```$ git clone (원격 저장소 주소 https::/)``` : 원격 저장소를 복제해서 지역 저장소를 만든다. 기본적으로 원격저장소 폴더명으로 저장소를 생성한다. 뒤에 폴더명을 지정하면 그 폴더명으로 생성시킨다.

### git pull
* ```$ git pull``` : 현재 저장소의 최신 commit을 가져온다. 따라서 작업할 때 <span style = "color:black; background-color:yellow;">"pull --> 작업 --> commit --> push"로 작업을 하면된다.</span>

## 4. [GIT CLI - 협업](https://opentutorials.org/course/3842)
### 협업(push & pull)
* github에서 private이건 public이건 settings-collaborators에 추가하여야지 push를 할 수 있다. 초대받으면 메일을 받게 되고 Accept invitation을 해야한다.    
  
* 한사람 <span style = "color:red;">A</span>가 ```작업 --> commit --> push``` 를 했는데  
다른 <span style = "color:blue;">B</span>가 ```pull --> 작업 --> commit --> push```가 아니라  
```작업 --> commit --> push```을 했다면  
**rejected**당하고 git은 ```$ git pull```을 하라고 한다.  
하지만 같은 파일의 같은 라인을 수정한 경우, **conflict**가 일어난다.  
이때 <span style = "color:blue;">B</span>가 **conflict**가 생긴 부분을 수정, 해결한 뒤 ```commit --> push```하면 merge된다.  
이후, <span style = "color:red;">A</span>는 나중에 ```pull```하여 가져와서 ```$ git log --graph```를 보면 <span style = "color:red;">A</span>는 이전에 <span style = "color:blue;">B</span>가 ```merge```한 작업을 보게 된다.  
 
* 우리는 위의 <span style = "color:blue;">B</span>와 달리 항상 ```pull```을 하도록한다. 다른 사람이 업데이트 했는지 확인하는 것이 좋은 습관이다.

### pull VS fetch
```
git pull = git fetch; git merge FETCH_HEAD;
```
 
* ```$ git fetch``` : ```$ git pull```과 다르다. 원격 저장소가 가지고 있는 ```commit```들을 로컬 저장소로 update해준다. merge는 되지 않는다.  
이때, HEAD는 fetch 이전의 commit을 그대로 가리킨다.  
즉,  
  ```  
  git pull = git fetch; git merge master/origin(원격저장소 이름)
  ```

* ```fetch```를 할 때 마다 ```.git/FETCH_HEAD```파일에 가장 최상위 ```commit id```을 표시한다.  
따라서, 가장 최근의 ```commit```으로 ```HEAD```를 돌리기위해 항상 ```master/origin```와 ```merge```할필요 없이 아래와 같이 하면된다.
  ```
  git fetch; git merge FETCH_HEAD;
  ```
 
* ∴ 신중하게 pull을 하고 싶으면 fetch를 사용한뒤 나중에 merge하면 된다.

## 5. [GIT - CLI cherry-pick & rebase](https://opentutorials.org/course/3843/24443)
### cherry-pick
- 




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
  $ git push origin dev/result_with_map 

  # 아래와 같이 연동해둔다.
  # 해당 브랜치로 이동후
  $ git branch --set-upstream-to origin/dev/result_with_map 
  # 또는
  $ git branch --set-upstream-to origin/dev/result_with_map dev/result_with_map  
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

- 