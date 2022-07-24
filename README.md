Git
======================
[Gitn](https://opentutorials.org/course/3838) - 생활코딩 강의     
    
----------------------
## 1. [Git CLI 버전 관리](https://opentutorials.org/course/3839)
> ### 명령어
> * ```$ git init (diretory)```: initialize repository, (diretory)에 리파지토리를 초기화 하여 생성    
> ex) ```$ git init .``` : 현재 디렉토리에 initialize
>    
> * ```$ rm -fr .git``` : 레파지토리 삭제  
> cf) -f : forced, -r : recursive
>  
> * ```$ git status``` : 현재 작업중인 repository가 어떤 상태인지 알려준다.
>    
> * ```$ git add``` : add to staging area   
> ex) ```$ git add (파일이름1) (파일이름2) ...``` : 파일들을 add하여 stage에 올린다.   
> ex) ```$ git add .``` or ```$ git add *``` : 현재 폴더 전부 add    
> ex) ```$ git add (폴더)``` : 해당 폴더 전부 add    
>     
> * ```$ git commit``` : commit the version to repository, Stage에 올라온 버전을 commit, 옵션을 택하지않으면 편집기에서 commit message를 입력한 후 저장하면 커밋된다.    
> cf) ```$ git commit -m (메세지)``` 로 메세지 입력   
> cf) ```$ git commit -am (메세지)``` add까지 한번에 수행하여 commit한다. 단, 한번이라도 add를 수행하여 tracked상태여야 수행한다.  
> cf) ```$ git commit --amend``` : 너무 일찍 커밋했거나 어떤 파일을 빼먹었을 때 그리고 커밋 메시지를 잘못 적었을 때 한다. 다시 커밋하고 싶으면 파일 수정 작업을 하고 Staging Area에 추가한 다음 --amend 옵션을 사용하여 커밋을 재작성 할 수 있다.  
>     
> * ```$ git log``` : commit 기록, 'q'버튼으로 종료     
> cf) ```$ git log --stat``` : 상태도 표시   
> cf) ```$ git log -p``` : patch, 더욱 상세하게 어떻게 수정되었는지 표시  
> cf) ```--all``` : 모든 브랜치에 대해  
> cf) ```--graph``` : 시각화  
> cf) ```--oneline``` : 한줄요약  
>     
> * ```$ git diff``` : 마지막 commit한 버젼과 working tree사이에 차이점을 파악할 수 있다. 버젼을 만들기 전에 뭐했는지 최종적으로 검토를 할 수 있다.
>     
> * ```$ git reset --hard``` : working tree를 마지막 commit 버젼으로 리셋한다.   
> cf) ```$ git reset --hard (commit id)``` : (commit id)로(이 버젼으로) 리셋 하겠다.   
> cf) reset 옵션의 --hard 모드는 현재하고있는 working tree의 작업까지 모두 지운다. (```$ git reset --help``` 참고)  
> cf) reset은 보통 ```(commit id)```를 인자로 사용하지만 ```(branch name)```을 인자로 실행 할 수 도 있다.  
 이 경우 ```(branch name)```을 인자로 실행하면 ```(branch name)```(대상 branch)가 가리키고 있던 commit을 원래의 branch(reset 실행 전 head가 가리키는 branch)가 가리키도록 하는 것이다.
>   
> * ```$ git revert (commit id)``` : 삭제의 목적과 보전의 목적을 모두 수행하기위해 사용한다. (commit id)에서의 변화를 취소한다. 즉, 그 커밋 이전 버젼으로 복원된다.    
> cf) revert는 반드시 차근차근 역순으로 수행해야한다. 그렇지 않고 **"임의의 지점으로 한번에 점프하도록 revert명령을 시키면 충돌이 일어나 대단히 복잡해진다?"**     
>     
> * ```$ git checkout (commit id)``` : 버전을 ```commit id```였을 때로 돌린다. 즉, ```head```가 ```commit id```를 가리키도록 한다. 이렇게 ```head```가 ```branch```를 가리키지 않는 상황을 ```detached```상태라고 한다.  
> cf) ```$ git checkout master``` : 가장 최근(앞)의 버젼으로 돌아간다. ```head```를 ```master```로 이동시킨다.  
> --------------------     
> ### 상태들
> |#|상태|의미|
> |:---:|----|:----|
> | 1 |<span style="color:black; background-color:yellow;">**Working Tree**</span> | 아직 version 으로 만들어지기 전 단계이고 Stage에도 올라가지 않은 상태, 그냥 작업만 한 것들이 이 상태이다. |
> | 2 |<span style="color:black; background-color:yellow;">**Staging Area**</span> | 복수의 파일 중 원하는 파일들을 선택하여 버젼으로 만들고 싶을 때 Staging Area에 선택한 파일들을 올려둔다. 그 다음 commit하면 동시에 commit된다.|
> | 3 |<span style="color:black; background-color:yellow;">**Repository**</span> | version 이 저장되는 곳 (.git)|
> 
> ----------------------
> ### 폴더 및 파일
> 
> * ```.git 디렉토리```에 version 정보들이 기록된다.
>     
> * ```.gitignore 파일```는 버젼관리를 하지말아야 할 파일들, 필요에 따라 임시파일을 만들지 않게하거나, 비밀번호나 개인정보를 올리고 싶지 않을 때
----------------------
## 2. [GIT CLI - Branch & Conflict](https://opentutorials.org/course/3840)
> ### Branch
> * ``` git branch``` : 브랜치를 본다.  
>  
> * ``` git branch (brach name)``` : (brach name)으로 브랜치를 생성한다.  
>  
> * ```$ git checkout (branch name)``` : 현재 브랜치에서 나와 ```(branch name)```으로 이동한다. ```head```가 ```(branch name)```을 가리키도록 한다.  
>  
> 
> ----------------------
> ### Merge
> * 서로 갈라졌던 Branch를 합치는 것을 병합(Merge)라고 한다. (merge commit)  
>  
> * 서로 다른 Branch에서 공통의 조상을 Base라고 한다.  
>  
> * ```$ git merge branch1``` : branch1에서 -> branch2으로 병합하려면, 일단 ```HEAD -> branch2``` 가 되도록 하고 병합하고 싶은 브랜치(branch1)을 지정하면 된다. 즉, branch2에서 branch1을 당겨 오는 것.  
>  
> * <span style="color:black; background-color:yellow;">중요</span> : 각각의 branch에서, 같은 파일의 다른 부분을 수정하고 commit한 뒤, 병합하면 각각의 수정사항이 모두 병합된다.  
그러나, 같은 파일의 같은 부분을 수정하고 commit한 뒤, 병합하면 Git은 자동으로 병합하지 못한다.  
 이를 <span style="color:red">**충돌(conflict)**</span> 이라고한다.  
 충돌이 일어난 부분에 아래와 같이 되고 이 부분을 수정후 ```$ git commit```하면 충돌을 해결한다.
 > ```
 > <<<<< HEAD
 > ....(HEAD에서의 내용)
 > ======
 > ....(당겨온 branch에서의 내용)
 > >>>>> (branch name)
 > ```
 > * <span style="color:black; background-color:yellow;">3 way merge</span> : 하나의 파일에서 base(공통의 조상)과 비교했을 때 다른 브랜치에서 수정된 쪽으로 병합된다.
 >  
 > * detached : ```head```가 ```(branch name)```을 가리키도록 하지않고 ```head```가 ```commit id```를 가리키도록 할 때가 있다. 이렇게 ```head```가 ```branch```를 가리키지 않는 상황을 ```detached```상태라고 한다.  
----------------------------
## 3. [GIT CLI - Backup](https://opentutorials.org/course/3841)
> ### git hosting (git push)
> * ```$ git remote``` : 원격 저장소가 있는지 확인한다.  
> cf) ``` -v ``` : 원격 저장소의 상태와 주소도 표시  
>   
> * ```$ git remote add (원격 저장소 별명) (원격 저장소 주소)``` : 지역 저장소(로컬 저장소, 내 컴퓨터에서 사용 중인 저장소)와 원격 저장소를 연결한다. 이 때, ```(원격저장소 별명)```은 origin으로 사용하는 것이 관습화 되어있다.?  
> cf) ```$ git remote remove (원격 저장소 별명)``` : 원격 저장소와의 연결을 끊는다.
> 
> * ```$ git push --set-upstream origin master``` : 여러 개의 원격 저장소와 연결 될 수 있는데 어떤 원격 저장소와 기본적으로 연결 시킬 것인지 설정한다. 즉, 앞으로 origin에 master라는 브랜치로 기본적으로 push(업로드)한다.
> ---------------
> ### git clone
> * ```$ git clone (원격 저장소 주소 https::/)``` : 원격 저장소를 복제해서 지역 저장소를 만든다. 기본적으로 원격저장소 폴더명으로 저장소를 생성한다. 뒤에 폴더명을 지정하면 그 폴더명으로 생성시킨다.
> ----------------
> ### git pull
> * ```$ git pull``` : 현재 저장소의 최신 commit을 가져온다. 따라서 작업할 때 <span style = "color:black; background-color:yellow;">"pull --> 작업 --> commit --> push"로 작업을 하면된다.<span>
