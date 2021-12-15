![image](https://user-images.githubusercontent.com/79847020/146065149-49caaa33-c506-430f-879a-9be6cfaff0ff.png)

# GIT / GITHUB

리눅스 커널을 개발한 리누스 토르발스 개발

GIt - 분산형 버전 관리시스템 / Github - 프로젝트 호스팅 서비스

## 기능 

### GIT 

1. 이력 기록 및 추적
2. 원격 저장소 및 공유
3. 변경 이력 병합

### GITHUB

1. 호스팅 서비스
2. 공개 및 비공개 저장소
3. 고급 기능 - 깃허브 액션, 깃허브 디플로이먼트 API

## GITHUB 인증 방식

깃허브에서 제공하는 개인용 액세스 토큰을 발급받아 해당 토큰을 패스워드로 사용해야 합니다.

깃허브 [Settings] -> [Developer setting] -> [Personal access tokens] 

# 실습 1

# 1. 지역 저장소 생성

## 1.1 깃 지역 저장소 설정 - git init
 
  ```CONSOLE
  $ git init
  Initialized empty Git repository in C:/study/git-github-programming/chapter2-basic/.git/
  ```

## 1.2 .git 폴더 확인

  .git 폴더에는 깃 지역 저장소에서 관리하는 파일, 브랜치, 설정 정보 등이 담겨 있습니다.

  ```CONSOLE
  $ ls -a
  ./  ../  .git/

  $ cd .git
  ./  ../  .git/

  $ ls -l
  total 7
  -rw-r--r-- 1 stead 197609  21 Nov 15 11:45 HEAD
  -rw-r--r-- 1 stead 197609 130 Nov 15 11:45 config
  -rw-r--r-- 1 stead 197609  73 Nov 15 11:45 description
  drwxr-xr-x 1 stead 197609   0 Nov 15 11:45 hooks/
  drwxr-xr-x 1 stead 197609   0 Nov 15 11:45 info/
  drwxr-xr-x 1 stead 197609   0 Nov 15 11:45 objects/
  drwxr-xr-x 1 stead 197609   0 Nov 15 11:45 refs/
  ```

## 1.3 git init 취소

  git init를 취소하고자 한다면 .git 폴더를 삭제하면 됩니다.

  ```CONSOLE
  $ rm -rf .git
  ```

# 2. 환경 설정 하기

## 2.1 사용자 등록
 
  ```CONSOLE
  $ git config user.name "steadyin"
  $ git config user.email "steadyin@naver.com"
  ```
  
  --global 옵션으로 모든 프로젝트에 적용될 사용자 정보를 등록할 수 있습니다.
  
  ```CONSOLE
  $ git config --global user.name "사용자 이름"
  $ git config --global user.email "이메일 주소"
  ```

## 2.2 깃 설정 파일 확인 - cat .git/config

  ```CONSOLE
  $ cat .git/config
  [core]
          repositoryformatversion = 0
          filemode = false
          bare = false
          logallrefupdates = true
          symlinks = false
          ignorecase = true
  [remote "origin"]
          url = https://github.com/steadyin/chapter2-basic.git
          fetch = +refs/heads/*:refs/remotes/origin/*
  ```

## 2.3 원격 저장소 설정 - git remote 

  ```CONSOLE
  $ git remote add origin https://github.com/steadyin/chapter2-basic.git
  ```

## 2.4 .gitignore 파일 설정

  깃으로 관리하지 않는 파일 설정
  
  gitignore.io 도구를 활용 해서 생성 가능

  .gitignore 파일 작성
  
  ```CONSOLE
  # Logs
  Logs
  *.log
  snpm-debug.log*

  # Dependency directories
  node_modules/
  ```

# 3. 파일 상태 확인하기

## 3.1 깃 작업 트리(working tree)

  ![image](https://user-images.githubusercontent.com/79847020/146063770-38a4cc46-6e3d-4f38-b3ea-a5bd354ff5d6.png)

  깃은 관리하는 프로젝트의 작업을 효율적으로 처리하기 위해 작업 트리라는 개념을 사용합니다. 작업 트리란 깃이 추적(관리)하는 파일과 추적하지 않는 파일을 구분하고 추적하는 파일들의 상태를 구분 짓는 영역이라고 생각하면 됩니다.

  * 작업 디렉터리 : 실제 작업 중인 파일들이 존재하는 영역
  * 스테이징 영역 : 작업 디렉터리에서 작업 중인 파일 중 깃이 추적하는 파일들
  * 지역 저장소 : 스테이징 영역에 추적되는 파일이 커밋으로 등록되는 영역

그리고 이 영역들 사이에서 다음 단계로 진행시키기 위해 git add 와 git commit 을 사용합니다.

  * git add : 작업 디렉터리에서 작업 중인 파일을 git add 명령어를 이용하여 스테이징 영역에 추적하는 파일로 등록
  * git commit : 스테이징 영역에서 식별된 파일을 지역 저장소에 등록하게 됩니다.

## 3.2 깃으로 파일 상태 확인  

  ![image](https://user-images.githubusercontent.com/79847020/146107410-ce9778d5-1e4e-4550-b168-1e123b28bcc4.png)

  깃에서 관리하는 파일은 Untracked와 Tracked 상태로 나뉩니다. 현재 작업 진행 중인 작업 디렉터리에서 새로 생성된 파일은 Untracked 상태가 됩니다. 주의할 점은 한 번 Tracked 상태가 되었다가 작업 디렉터리에서 수정된 파일은 Untracked 상태가 아니라는 점입니다.
  
  * 작업 디렉터리/Untracked : 아직 작업 디렉터리에 있는 상태. git add로 스테이징 영역에 등록할 수 있다.
  * 스테이징 영역/Unmodified : commit으로 으로 지역 저장소에 기록할 수 있는 최종 상태. git add로 스테이징 영역에 최초 등록시에도 이 상태이다.
  * 스테이징 영역/Modified : 스테이징 영역에 있지만 파일에 변화가 있는 상태. git add로 Unmodified 상태로 전환할 수 있다.
  
### 3.2.1 깃 상태 확인 - git status

  현재 작업 진행 중인 작업 디렉터리에서 새로 생성한 파일이 이기 때문에 Untracked 상태로 존재합니다.

  ```CONSOLE
  $ git status
  On branch main

  No commits yet

  Untracked files:
    (use "git add <file>..." to include in what will be committed)
          .gitignore

  nothing added to commit but untracked files present (use "git add" to track)
  ```

### 3.2.2 커밋에 포함될 파일 추가 - git add 
  
  git add 명령어를 이용하여 .gitignore 파일을 커밋 대상으로 등록했습니다. 해당 파일이 깃에서 관리하는 파일일 Tracked 상태가 되었고 스테이징 영역에서 커밋으로 기록될 준비가 되어있다는 의미입니다.

  ```CONSOLE 
  $ git add .

  $ git status
  On branch main

  No commits yet

  Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
          new file:   .gitignore
  ```

  * 참고 : warning : LF will be replaced by CRLF in .gitignore.

    해결방법 : git config core.autocrlf true

    운영체제 마다 개행문자를 인식하는 방법이 달라서 발생하는 경고입니다. 

## 3.3 커밋 생성 후 푸시  
 
### 3.3.1 커밋 생성하기 - git commit 

  #으로 시작하는 줄은 커밋 메시지에 반영되지 않습니다. on branch main라는 메시지는 현재 커밋으로 기록하는 브랜치가 main이라는 의미합니다. 

  'Add .gitignore file'를 추가하고 저장합니다.

  ```CONSOLE
  $ git commit
  
  Add .gitignore file

  # Please enter the commit message for your changes. Lines starting
  # with '#' will be ignored, and an empty message aborts the commit.
  #
  # On branch main
  #
  # Initial commit
  #
  # Changes to be committed:
  #       new file:   .gitignore
  #
  ```

### 3.3.2 커밋 이해하기

  git log 명령어로 현재 작업하는 브랜치의 커밋 로그를 확인할 수 있습니다. 브랜치란 특정 기능을 독립적으로 개발하는 용도의 공간이라고 생가갛면 됩니다.
  
  깃에는 HEAD라는 특별한 포인터가 존재합니다. HEAD 포인터는 현재 작업하는 브랜치의 최종 커밋을 가리킵니다. HEAD -> main은 HEAD 포인터가 main 브랜치를 가리키고 있다는 의미입니다.
  
  ```CONSOLE
  $ git log
  commit 2c4292c4189118eae8ea48cab7c959b64139e2e7 (HEAD -> main)
  Author: jihoon <79847020+steadyin@users.noreply.github.com>
  Date:   Mon Nov 15 14:04:52 2021 +0900

  Add .gitignore file
  ```
  
  첫줄은 커밋 체크섬을 나타냅니다. 체크섬은 커밋을 식별하는 고유한 데이터 단위입니다. 또한 HEAD 포인터가 main 브랜치를 가리키고 있는 것을 확인할 수 있습니다.
  
  * git log

    커밋 로그를 조회하는 가장 기본적인 명령어입니다.
   
  * git log -p / git log --patch

    -p 옵션은 patch의 약자입니다. 파일 단위에서 변경 내용을 보여줍니다.
    
  * git log -[숫자]

    최근 몇 개의 커밋을 보여줄지 지정합니다.
    
  * git log --stat

    각 커밋의 통계 정보를 볼 수 있습니다. 통계란 어떤 파일이 수정되었고 각 파일에 몇 줄이 추가 혹은 삭제되었는지를 의미합니다.
    
  * git log --pretty={option}

    --pretty 옵션을 사용하면 커밋 로그를 보여주는 형식을 지정할 수 있습니다. 
    
    * git log --pretty=oneline
    * git log --pretty=format: "%h %an %s"
  
  * git log --pretty=oneline --graph
      
      graph 옵션을 사용하면 가시적으로 커밋 로그의 흐름을 파악할 수 있습니다.

### 3.3.3 커밋 수정하기

  반영 된 변경사항을 최근 커밋에 반영합니다.

  * git commit --amend / git commit --amend -m "수정 메시지"
    
  * git commit --amend --author "username <email>"
  
  * 기존 커밋에 파일 추가

    1. git add '파일"

    2. gt commit --amend 

## 3.4 커밋 푸시하기
  
  커밋을 푸시한다는 것은 지역 저장소에 있는 커밋을 원격 저장소에 등록한다는 의미입니다.
  
  * git push {저장소} {브랜치}  
    
  ```CONSOLE
  $ git push origin main
  Enumerating objects: 3, done.
  Counting objects: 100% (3/3), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), 299 bytes | 299.00 KiB/s, done.
  Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
  To https://github.com/steadyin/chapter2-basic.git
   * [new branch]      main -> main
  ```

## 3.5 원격 저장소 복제하기
  
  원격 저장소를 지역 저장소에 복제하여 사용하는 방법이 있습니다.
 
  * git clone "원격 저장소 주소" "새로운 지역 저장소 이름"
  
  ```CONSOL
  root $ git clone http://github.com/steadyin/chapter2-basic.git chapter2-basic-clone
  ```
 
  지역 저장소 이름을 지정하지 않고 git clone 원격 저장소 주소 만 입력하면 동일한 이름으로 지역 저장소가 생성됩니다.
 
   
# 4. 실습 2 ( 협업자, 이슈, 라벨, 프로젝트 보드 )
 
## 4.1 협업자 등록
 
  원격 저장소 메인 페이지에서 Setting > Manage Access 에서 협업자를 등록합니다.
 
## 4.2 이슈
  
  이슈는 깃허브에서 제공하는 도구 입니다. 프로젝트 작업, 개선사항, 오류추적 기능을 제공합니다. 실제 프로젝트에서는 새로운 기능 기존 기능 개선, 오류 해결 등의 작업이 계속 몰려듭니다. 이러한 일을 이슈 단위로 작성하고 관리할 수 있습니다. 
  
  깃허브에서는 이슈의 성격에 맞게 템플릿을 지정하는 기능도 제공하고 있습니다.
  
  http://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests
  
### 4.2.1 라벨

  라벨은 이슈의 성격을 구분짓는 도구 입니다. 

  ![image](https://user-images.githubusercontent.com/79847020/141807731-1c8b6c2d-25e8-4d52-bbd9-5d521552dfcd.png)

### 4.2.2 마일스톤
  
  [Milestone]은 특정 프로젝트가 달성해야 하는 목표 시점과 작업을 관리할 수 있도록 돕는 도구 입니다. 마일스톤은 종료일을 설정후 특정 이슈를 마일스톤에 포함시켜 진행 상황을 파악하는 도구입니다.
  
  깃허브에서는 이슈의 성격에 맞게 템플릿을 지정하는 기능도 제공하고 있습니다.
  
  http://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests
  
### 4.2.3 프로젝트
  
  [Projects]는 깃허브에서 제공하는 프로젝트 보드 기능입니다. 해당 기능은 특정 프로젝트의 작업 관리를 돕는 도구입니다.
  
  다음은 프로젝트 보드에서 수행할 수 있는 작업 입니다.
  
  * Soft tasks : 프로젝트 보드에 이슈 및 풀 리퀘스트를 추가하고 우선순위를 지정할 수 있습니다.
  * Plan your project : 작업 상태별로 열을 생성하고 작업의 상태를 관리할 수 있습니다.
  * Automate your workflow : 이벤트를 설정하여 작업의 상태 관리를 자동으로 수행할 수 있습니다.
  * Track progress : 프로젝트 보드에서 일어난 모든 일을 추적하고 확인할 수 있습니다.
  * Share status : 프로젝트 보드의 작업 단위인 카드는 고유한 URL을 갖고 있어서 다른 팀원들에게 공유하고 논의하는 데 사용됩니다.
  * Wrap up : 특정 프로젝트 보드의 모든 작업을 마무리한 후 활성 프로젝트 목록에서 제거하여 다음 프로젝트 보드를 생성할 수 있습니다.
  
  프로젝트 보드에서 제공하는 5가지 템플릿
  
  * None : 빈 템플릿을 생성합니다.
  * Basic kanban : To do, In progress, Done 작업 상태열을 기본적으로 생성해줍니다.
  * Automated kanban : Basic kanban 보드와 동일하게 To do, In progress, Done 작업 상태열이 기본적으로 생성됩니다. 추가로 카드의 작업 상태가 프로젝트에 포함된 이슈의 상태에 따라 자동으로 변경됩니다.
  * Automated kanban with reviews : Automated kanban 보드와 동일하지만 작업 상태 변경 요소에 풀 리퀘스트의 상태가 추가로 반영됩니다.
  * Bug triage : 버그를 분류하기 위한 작업 상태열을 생성합니다.
  
# 5. 실습 3 ( 브랜치 )
  
  브랜치(branch)란 프로젝트 기준 코드인 main 브랜치로부터 독립적인 작업 공간을 만들어주는 기능입니다. 여러 개발자가 서로 다른 버전의 코드를 만들 때 서로의 직업에 영향을 주고받지 않기 위해필요합니다. 
  
  ```CONSOLE
  $ git status
  On branch main
  nothing to commit, working tree clean
  ```  
  On branch main 우리는 지금까지 main 브랜치에서 작업하고 커밋을 생성했습니다. 
  
  ```CONSOLE
  $ git log --pretty=oneline --graph
  * dec243fa9fd9d6b1df666e35759abeb5820da2e6 (HEAD -> main, origin/main) Add hotline to main page
  * 5a30b25c2887bacfe5177677a569153854817742 Change the title of main page
  * e47e1b2ae3df3d3711fb34ec6140e1f24506fdd0 Add initial files and .gitignore
  ```
  
![image](https://user-images.githubusercontent.com/79847020/144547077-727e8edc-7cf1-41fa-8bda-91a78a179266.png)

  main 브랜치는 가장 최근에 생성된 커밋을 `dec243~~' 바라보고 있고 HEAD 포인터는 현재 작업하는 곳(브랜치)의 최종 커밋을 바라봅니다. 즉 현재 프로젝트의 HEAD 포인터는 main 브랜치에서 작업 중이며, main 브랜치는 가장 최근 커밋을 바라봅니다.
  
  브랜치를 생성하는 방법은 2가지 입니다.
  1. 깃허브 원격 저장소에서 생성 후, 지역 저장소로 가져오기
  2. 지역 저장소에서 생성 후, 원격 저장소에 반영하기
  
## 5.1 원격 저장소에서 생성 후 지역 저장소로 가져오기
  
  * 원격 저장소에서 branch를 생성합니다. from main 에서 알 수 있듯이 새로운 브랜치는 현재 main 브랜치의 최신 상태를 기준으로 생성됩니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144547457-87a8063f-2e36-422a-824f-27482da79891.png)
  
  * git remote update
  
    지역 저장소에 원격 저장소의 상태르 갱신합니다.

    ```CONSOLE
    $ git remote update
    Fetching origin
    From https://github.com/steadyin/mastering-git-github
    * [new branch]      test/remote-branch -> origin/test/remote-branch
    ```
  
  * git branch -a
  
    git branch 명령어를 통해 지역 저장소와 원격 저장소의 브랜치 정보를 확인합니다. -a 옵션은 지역 저장소와 원격 저장소의 브랜치 정보를 모두 보여줍니다. 

    ```CONSOLE
    $ git branch -a
    * main
    remotes/origin/main
    remotes/origin/test/remote-branch
    ```
  
    * main'은 지역 저장소의 main 브랜치를 의미하며 * 표시는 현재 작업 중인 브랜치를 의미합니다. 'remotes/origin' 접두사가 붙은 브랜치는 원격 저장소, 특히 origin 식별자로 등록한 원격 저장소의 브랜치를 의미합니다. 원격 저장소에는 main과 test/remote-branch 브랜치가 존재한다는 사실을 확인할 수 있습니다.
  
  * git checkout -t <브랜치명>
  
    git checkout 명령어로 원격 저장소에서 생성한 브랜치를 지역 저장소의 작업 브랜치로 설정합니다.
  
    ```CONSOLE
    $ git checkout -t origin/test/remote-branch
    Switched to a new branch 'test/remote-branch'
    Branch 'test/remote-branch' set up to track remote branch 'test/remote-branch' from 'origin'.

    $ git branch -a
    main
    * test/remote-branch
    remotes/origin/main
    remotes/origin/test/remote-branch
    ```
  
  origin 원격 저장소의 test/remote-branch 브랜치를 지역 저장소의 작업 브랜치로 설정했습니다. 이제 지역 저장소에도 test/remote-branch 브랜치가 생성되었으며 필요한 작업을 진행할 수 있습니다. "remotes" 접두사를 제외하고 명령어를 실행한 점을 주의해야한다. 
  
  ```CONSOLE
  $ git checkout -> 사용할 브랜치를 지정합니다.
  $ git checkout -b -> 브랜치를 생성하고 사용할 브랜치로 지정합니다.
  $ git checkout -t -> 원격 저장소에서 생성한 브랜치를 지역 저장소에서 사용할 브랜치로 지정합니다.
  ```

  git branch 명령어의 몇 가지 옵션을 함께 살펴봅시다.
  
  ```CONSOLE
  $ git branch -a -> 지역 저장소와 원격 저장소의 브랜치 정보를 함께 보여줍니다.
  $ git branch -d <브랜치명> -> 브랜치 삭제
  $ git branch -l -> 지역 저장소의 브랜치 정보를 보여줍니다.
  $ git branch -r -> 원격 저장소의 브랜치 정보를 보여줍니다.
  $ git branch -v -> 지역 저장소와 브랜치 정보를 최신 커밋 내역과 함께 보여줍니다.
  ```
  
  ## 지역 저장소에서 깃을 통해 브랜치 생성하기
  
  이제 지역 저장소에서 깃 명령어로 새로운 브랜치를 생성한 후 원격 저장소에 반영해보겠습니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144548421-485ff252-bc06-4fb3-9771-bcb56ecde5da.png)
  
  현재 작업 브랜치는 test/remote-branch 입니다. main 브랜치를 기준으로 새로운 브랜치를 생성하려면 현재 작업 브랜치를 main 브랜치로 변경해야 합니다.
  
  * git checkout 명령어를 실행해 작업 브랜치를 main 브랜치로 변경합니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144548589-865beb5a-88e7-4d21-a26d-eda35e0b7922.png)
  ![image](https://user-images.githubusercontent.com/79847020/144548658-c22933fa-4212-43aa-a709-b534286160c3.png)
  
  * git branch 명령어를 실행하여 새로운 브랜치를 생성합니다.

  ![image](https://user-images.githubusercontent.com/79847020/144548770-9d6b65a9-088b-49ed-97ac-2ac32fa0da00.png)
  
  test/local-branch라는 새로운 브랜치가 정상적으로 생성됐습니다. 현재 작업 브랜치는 main 브랜치입니다. 또한 'remotes'origin' 접두사가 붙은 'test/local-branch'는 없는 것으로 보아 원격 저장소에는 아직 반영되지 않았습니다.
  
  * git checkout 명령어를 실행해 새로운 브랜치를 작업 브랜치로 변경하고 원격 저장소에 반영해봅시다.

  ![image](https://user-images.githubusercontent.com/79847020/144548931-4fbb0141-c080-4349-bdfd-3510c1022be4.png)

  origin 식별자로 저장되 원격 저장소에 test/local-branch를 반영했습니다. 
  
  ![image](https://user-images.githubusercontent.com/79847020/144549250-b8bed6a1-6fe6-4038-9b02-65841d8753c7.png)

  ## 브랜치 삭제하기
  
  * 지역 저장소의 브랜치를 삭제합니다.
  
  ```CONSOLE
  git branch -d {브랜치명}
  ```
  
  * 원격 저장소의 브랜치를 삭제합니다.
  
  ```CONSOLE
  git branch origin -d {브랜치명}
  ```
  
  ## 브랜치 병합하기
  
  작업이 완료되면 기준 브랜치에 반영하는 작업이 필요하겠죠. 이 작업을 브랜치 병합이라고 합니다.
  
  ### 브랜치 병합이란 ?
  
  두 브랜치를 비교하여 파일의 변경 내용을 비교하고 합칩니다. 

  ![image](https://user-images.githubusercontent.com/79847020/144549740-7cd00794-b524-47ed-803b-99dc624b61f6.png)
  
  해당 커밋내역 상태를표현하면 다음 그림과 같습니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144550065-88de4a0a-06a8-4f29-ac6f-66dca8fb0767.png)

  그림에서 보는 것처러 ㅁ지역 저장소의 test/local-branch 역시 main 브랜치와 동일한 커밋을 바라보고 있습니다. 그렇다면 test/local-branch를 작업 브랜치로 변경하여 새로운 커밋을 생성한 후 우리는 어떤 작업을 해야할까요?
  
  ![image](https://user-images.githubusercontent.com/79847020/144553084-b33ff67b-a0cc-474e-91b7-464587026738.png)

  test/local-branch에서 새로운 커밋을 생성했다고 가정하겠습니다.. 이 커밋은 기준 브랜치인 main 브랜치가 아직 알 수 없는 커밋이죠. 따라서 작업 브랜치(test/local-branch)에서 작업을 완료한 후 새로운 커밋을 main 브랜치에 반영하는 작업이 필요합니다. 반영 작업을 진행하면 머지 커밋(merge commit)이라는 새로운 커밋이 생성됩니다. 이것이 병합입니다. 기준 브랜치에 작업 브랜치의 새로운 커밋을 반영하는 방법은 크게 2가지이며 병합을 위한 추가적인 커밋 생성 여부를 기준으로 세웠습니다.
  
  ### 빨리감기 병합 : fast forward
  
  패스트 포워드 병합은 main 브랜치를 기준으로 작업 브랜치를 생성 후 작업을 온료하여 main 브랜치에 병합을 시도합니다. 이때 main 브랜치에 새로운 커밋이 없다면 빨리감기 병합으로 진행됩니다. 즉 기준 브랜치에 작업 브랜치의 새로운 커밋이 단순히 최신 커밋으로 더해지고 기준 브랜치가 바라보는 최신 커밋만 변경됩니다.

  ![image](https://user-images.githubusercontent.com/79847020/144553570-ea87d169-5e13-4611-96e2-7704f5391f43.png)
  
  파일을 수정한 후 새로운 커밋을 생성합니다.

  ![image](https://user-images.githubusercontent.com/79847020/144553923-572d9315-62cb-45a4-9391-2e82423f9b52.png)
  
  ![image](https://user-images.githubusercontent.com/79847020/144553989-0eff34c9-093f-473f-a4c6-34634d897387.png)

  현재 작업 브랜치 test/fast-forward만 가장 최근 생성한 커밋을 바라보고 있습니다. main 브랜치를 포함한 다른 브랜치들은 이전의 커밋을 바라보고 있습니다. test/fast-forward 브랜치의 작업 내용을 main 브랜치에 병합해보겠습니다.
  
  * main 브랜치를 작업 브랜치로 변경합니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144554134-b18111ef-ea16-4a3c-b8f7-0dba830c50da.png)

  test/fast-forward 에서 생성한 커밋은 보이지 않습니다. 새로운 브랜치의 ㅈ가업 내용을 병합하지 않았기 때문입니다.
  
  * test/fast-forward 브랜치의 작업 내용을 main 브랜치에 병합합니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144554253-74d6ee34-969c-494b-be92-8b7fdc3f26ed.png)
  
  test/fast-forward를 main 브랜치를 기준으로 생성하고 test/fast-forward 브랜치의 작업 내용을 다시 main 브랜치에 병합할 때까지 main 브랜치에는 아무런 변경 내용이 없었기 때문에 빨리감기 병합 방식으로 이루어졌습니다.
  
  * 커밋 내역을 다시 살펴봅니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144554463-f3309851-d909-4288-ac7a-569326255b5d.png)

  기존에 main 브랜치에 존재하지 않았던 커밋이 추가됐고 main 브랜치와 test/fast-forward 브랜치가 같은 커밋을 바라보고 있습니다.
  
  ### 병합 커밋 생성 : merge commit
  
  두 번째 병합 방법은 커밋 생성입니다. fast forward 병합과 다르게 기준 브랜치에 변경이 존재하는 경우에 사용하는 방법입니다. 기준 브랜치와 새로운 작업 브랜치의 변경 내용을 하나로 합치는 작업이 필요하고 이를 병합이라고 합니다. 
  
  * 'test/local-branch'를 작업 브랜치로 설정하겠습니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144558352-714e864f-edad-4d78-8645-307afc380a38.png)
  
  이전 커밋 내역을 살펴보면 test/local-branch는 현재 main 브랜치와 다른 커밋을 바라보고 있습니다. 즉 main 브랜치를 기준으로 test/local-branch 브랜치를 생성했을 때와 다르게 main 브랜치에 변경 내용이 존재합니다. 
  
  ![image](https://user-images.githubusercontent.com/79847020/144558576-ec0b2b4f-22c4-4d08-b14f-60e847e944f2.png)

  변경 내용을 만들기 위해 파일을 임의로 수정합니다. index.html을 열어보면 test/local-branch 블내치를 생성할 당시에는 title 내용을 수정한 커밋이 반영되지 않은 상태였기 때문에 title의 값에 여전히 변경이 없습니다. "운영팀 웹 어드민 시스템 v2"로 수정 합니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144558745-a189568f-84c2-4fd3-9dd8-46d38c23aaca.png)

  * 다시 main 브랜치를 작업 브랜치로 변경합니다.

  ![image](https://user-images.githubusercontent.com/79847020/144558838-5453ad47-a164-46a8-9efe-8a55f9bb26f7.png)
  
  * get merge 명령어를 이용하여 "test/local-branch" 브랜치의 작업 내용을 main 브랜치에 병합합니다.

  ![image](https://user-images.githubusercontent.com/79847020/144558925-24aba58b-1897-492a-82ff-8bb8a4d3b077.png)
  
  빨리감기 병합과는 다르게 git merge 명령어를 실행하면 터미널 결과와 같이 커밋 작성 에디터가 나타납니다. 바로 이 부분이 기준 브랜치인 main 브랜치와 작업 브랜치인 test/local-branch 양쪽의 변경 내용을 하나로 합치는 과정입니다. 커밋 작성 에디터를 저장하고 빠져나오면 Fast-forward 대신 Auto-mergin 이라는 결과를 확인할 수 있습니다.
  
  * 커밋 내역을 다시 살펴봅니다.
  
  ![image](https://user-images.githubusercontent.com/79847020/144559243-b19c46a8-8766-47ef-9056-ddc7fc17d532.png)
  
  test/fast-forward 브랜치에서 작업 후 main 브랜치에 병합된 커밋과 test/local-branch 브랜치에서 작업 후 main 브랜치에 병합된 커밋이 하나의 병합 커밋으로으로 묶여 생성됐습니다. 다시 한번 말씀 드리면 test/local-branch 브랜치를 main 브랜치 기준으로 생성할 당시에는 main 브랜치에 test/fast-forward 브랜치의 작업 커밋이 병합되지 상태였습니다. 따라서 test/local-branch 브랜치의 작업 커밋을 반영할 때 main 브랜치에 양쪽 변경 내용이 반영된 것입니다.

  
