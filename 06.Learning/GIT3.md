# 6. 실습4

지금까지 학습한 깃/깃허브 협업 기능 및 명령어를 실제 업무에서 발생할 법한 시나리오 기반으로 실습해봅시다. 팀 프로젝트 내에 요구사항이 등록되고 해당 요구사항이 개발되어 반영되는 과정을 실제로 따라 해봅시다.

## 6.1 프로젝트 소개

프로젝트의 이슈 관리를 위해 깃허브 프로젝트 보드의 자동화된 칸반(autumated kanban)을 사용합니다.

사용자와 역할입니다.

|이름|계정|역할|
|---|---|---|
|미정|steadyin|팀관리자|
|기호|stead95user|이슈를 생성|
|희진|chillaxjh|등록된 이슈를 확인하고 이를 개발|

### 6.1.1 현재 프로젝트 확인

```HTMl
<html>

<head>
  <title>운영팀 웹 어드민 시스템 v1</title>
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v2</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템 v1</p>
</body>

<footer>
  <p>연락처: 1111-2222</p>
</footer>

</html>
```

![image](https://user-images.githubusercontent.com/79847020/146166828-02f27774-0348-42e1-a027-c98b3f34f94b.png)

## 6.2 협업 시나리오 준비 작업

협업자 계정으로 등록, 프로젝트 보드 생성, 이슈 라벨 생성 작업을 진행하겠습니다.

### 6.2.1 협업자 계정을 등록합니다.

깃허브 원격 저장소에서 [Setting] -> [Manage access] 메뉴에서 사용자를 모두 등록합니다.

![image](https://user-images.githubusercontent.com/79847020/146167257-64def29a-50cb-4935-89b4-691c1178d2d0.png)

### 6.2.2 Project, Label 생성

깃허브 원격 저장소에서 [Projects] -> [Create a project] 에서 Project board name을 입력하고 [Template]를 Automated kanban으로 선택 후 [Create project]를 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/146167614-bb71b9f6-0586-4b67-8647-0dc9b3807918.png)

생성 된 보드에 기본 제공되는 이슈들을 삭제합니다.

![image](https://user-images.githubusercontent.com/79847020/146167698-157f967d-38ef-4865-8c7a-165eff339931.png)

[Issues] -> [Labels] -> [New label] 클릭 -> Label name 입력 후 [Create label] 클릭

![image](https://user-images.githubusercontent.com/79847020/146171203-42464a4f-c95a-4cb2-a506-6f5ec82c4994.png)

## 6.3 우리팀 저장소에 이슈 등록하기 : 개발자 기호

기호는 운영팀으로부터 다음과 같은 요구사항을 받았습니다.

메인 페이지 설명 추가 : 접근 권한이 필요하신 분은 운영팀에 문의해주세요.
메인 페이지 연락처 추가 : 이메일 주소

현재 기호는 작업 중인 일이 있어서 프로젝트 원격 저장소에 이슈를 등록하여 동료들에게 공유하기로 결정했습니다. 참고로 기호가 속한 팀은 깃허브 원격 저장소에 자동화된 칸반 유형의 프로젝트 보드를 생성하여 일을 관리하고 있습니다. 또한 메인 페이지의 내용을 간단히 수정하는 요구사항이 때문에 2가지 요구사항을 1개의 이슈로 처리하기로 했습니다.

다음과같은 순서로 진행합니다.

1. 원격 저장소에 이슈 생성
2. 프로젝트 보드에서 생성된 이슈 확인

깃허브 원격 저장소에 이슈를 생성합니다.

기호로 로그인 후 [Issue] -> [New Issue] 를 클릭합니다.

제목과 내용을 작성 후 [Assigness] 를 클릭해 팀 관리자인 희진에게 이슈를 할당했습니다. stadyin은 이슈를 확인 후 개발을 진행할 팀원에게 해당 이슈를 다시 할당하겠죠. 기호는 해당 이슈가 운영팀에서 전달됐기 때문에 [Labels]에서 operation 라벨을 [Projects]에서 현재 업무를 관리하는 프로젝트 보드인 'Operation v1' 을 선택합니다. [Submit new issue]를 클릭해 이슈를 생성합니다.

![image](https://user-images.githubusercontent.com/79847020/146177277-2f014926-d122-4dee-9311-c397b2234abf.png)

다음과 같은 화면이 나타납니다.

![image](https://user-images.githubusercontent.com/79847020/146177364-df7f3d50-0ee2-4de1-8241-409e01507edc.png)

이슈 생성하면서 프로젝트 보드를 지정했기 때문에 [Projects] -> [Operation v1]을 클릭하면 생성된 이슈를 바로 확인할 수 있습니다. 

이제 팀의 의사결정을 기다리면 됩니다. 기호는 편한 마음으로 진행 중인 개발을 마무리하기로 합니다.

## 6.4 우리팀 저장소에 이슈 개발후 반영하기 : 개발자 희진

희진은 작업 중인 개발을 마무리 해서 다음 업무를 찾기 위해 프로젝트 보드에 접속합니다. 기호가 등록한 이슈가 개발자에게 할당되어 있지 않은 걸 발견했습니다. 본인이 처리하려고 마음을 먹었습니다. 희진이 어떻게 처리하는지 따라가 봅시다.

1. 원격 저장소 프로젝트 보드에서 미해결 이슈 확인
2. 원격 저장소를 지역 저장소에 복제 : git clone
3. 새로운 작업 브랜치 생성 및 코드 수정 : git branch, git add, git commit, git push
4. 풀 리퀘스트 생성 및 동료들에게 검토 요청

### 6.4.1. 원격 저장소 프로젝트 보드에서 미해결 이슈 확인

깃허브 원격 저장소에서 [Projects] -> [Operation v1] 을 클릭합니다.

To do에서 진행되지 않은 이슈를 클릭해 내용을 확인합니다.

![image](https://user-images.githubusercontent.com/79847020/146178294-ef8c6cac-7709-47d1-a903-f53ff83a5b28.png)

오른쪽 하단에 [Go to issue for full details] 버튼을 클릭하여 이슈 상세 정보를 확인합니다.

이슈 상세 정보에서 [Assignees]를 클릭해 희진 본인으로(must-have-developer-2) 변경하여 팀원들에게 본인이 진행하는 중임으로 알립니다.

![image](https://user-images.githubusercontent.com/79847020/146178604-9040cbad-fc53-41bc-bff7-b402ce601b6a.png)

[Projects] -> [Operation v1] 에서 [To Do]에 있는 해당 이슈를 마우스로 드래그하여 [In progress] 작업 상태열에 드롭합니다. 

(팀원들에게 작업이 시작되었음을 알리는 것입니다.)

![image](https://user-images.githubusercontent.com/79847020/146178746-5f47219f-b282-4205-a2cc-7982d713280b.png)

### 6.4.2 원격 저장소를 지역 저장소에 복제 : git clone

원격 저장소의 프로젝트를 지역 저장소에 복제합니다. mastering-git-github 원격 저장소는 기호가 생성하고 관리했습니다. 즉, 희진의 지역 저장소에는 아직 해당 프로젝트가 존재하지 않는 상황이죠. 그래서 희진은 해당 원격 저장소를 git clone 명령어로 지역 저장소에 복제합니다.

![image](https://user-images.githubusercontent.com/79847020/146178940-5113ea59-a648-4377-942b-f0b3d8869add.png)

지역 저장소로 사용할 경로에서 git clone 명령어를 사용합니다.

```CONSOLE
$ git clone https://github.com/steadyin/mastering-git-github.git
Cloning into 'mastering-git-github'...
remote: Enumerating objects: 453, done.
remote: Counting objects: 100% (453/453), done.
remote: Compressing objects: 100% (349/349), done.
remote: Total 453 (delta 97), reused 450 (delta 95), pack-reused 0
Receiving objects: 100% (453/453), 574.73 KiB | 6.84 MiB/s, done.
Resolving deltas: 100% (97/97), done.
```

지역 저장소에 mastering-git-github 원격 저장소 복제를 완료했습니다. 비주얼 스튜디오 코드에서 해당 프로젝트를 열고 다음 작업을 진행합니다.

### 6.4.3 새로운 작업 브랜치 생성 및 코드 수정 : git branch, git add, git commit, git push

실습 프로젝트의 터미널 창을 열고 해당 이슈를 처리할 작업 브랜치를 생성 후 이동합니다.

현재 브랜치를 확인합니다.

```CONSOLE
$ git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/body-change
  remotes/origin/main
  remotes/origin/test/local-branch
  remotes/origin/test/remote-branch
```

새로운 브랜치를 생성합니다.

```
$ git branch feature/change-main-content
```

생성한 브랜치로 작업 브랜치를 변경합니다.

```
$ git checkout feature/change-main-content
Switched to branch 'feature/change-main-content'
```

index.html을 수정합니다.

```HTML
<html>

<head>
  <title>운영팀 웹 어드민 시스템 v1</title>
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v2</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템 v1</p>
  <p>접근 권한이 필요하신 분은 운영팀에 문의해주세요.</p> <!--수정부분 1-->
</body>

<footer>
  <p>연락처: 1111-2222</p>
  <p>operator@abc.com</p> <!--수정부분 2-->
</footer>

</html>
```

코드를 수정 완료했으니 새로운 커밋을 생성합니다.

```CONSOLE
$ git add .

$ git commit -m "Change content in main page"
[feature/change-main-content 547a353] Change content in main page
 1 file changed, 2 insertions(+)
```

동료들의 피드백을 받기 위해 작업 브랜치를 원격 저장소에 반영합니다.

```CONSOLE
$ git push origin feature/change-main-content
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 565 bytes | 565.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote:
remote: Create a pull request for 'feature/change-main-content' on GitHub by visiting:
remote:      https://github.com/steadyin/mastering-git-github/pull/new/feature/change-main-content
remote:
To https://github.com/steadyin/mastering-git-github.git
 * [new branch]      feature/change-main-content -> feature/change-main-content
```

무사히 'feature/change-main-content' 브랜치를 반영했으니 풀 리퀘스트를 생성합니다. 

[Pull requests] -> [New pull request] 를 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/146181827-00029f07-8f5a-451d-ac53-b19778856bc3.png)

기준 브랜치 base : (main) 와 작업 브랜치 compare : (feature/change-main-content)를 선택하고 [Create pull request]를 클릭합니다.

[Create pull request] 에서 아래 아이콘을 클립하고 팝업창에서 관련 이슈(메인 페이지 실행 및 연락처 추가)를 클릭해 본문에 참조합니다.

![image](https://user-images.githubusercontent.com/79847020/146182337-d8cdba96-7e3e-4b7e-acba-1d2eeed6fc1f.png)

[Reviewers] 에서 리뷰할 동료 기호를 선택합니다. [Create pull request]를 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/146182522-31d9676e-9b4a-41ec-8bd6-a6d40fce9842.png)

풀 리퀘스트와 이슈를 연결해보겠습니다.

[Issues] 클릭 -> 해당 이슈를 클릭 합니다.

![image](https://user-images.githubusercontent.com/79847020/146182796-02459942-fe53-476c-80d2-3f1efdcd7b0a.png)

[Link a pull request from the repository] 클릭해서 생성한 풀 리퀘스트(Change content in main page)를 클릭합니다.

이렇게 이슈와 관련된 풀 리퀘스트를 연결하면 해당 이슈가 어떻게 진행되고 있는지를 동료들에게 공유할 수 있습니다.

이제 남은 일은 기호의 피드백을 받고 해당 풀 리퀘스트를 기준 브랜치에 병합하여 반영하는 일입니다.

## 6.5 풀 리퀘스트 검토 및 승인

풀 리퀘스트를 검토하고 승인해봅시다. 작업 순서는 다음과 같습니다.

1. 기호 : 희진이 생성한 풀 리퀘스트 검토
2. 희진 : 기호에게 승인 받은 풀 리퀘스트 변경 내역 병합
3. 원격 저장소의 최신 변경 내역 반영 : git pull

### 6.5.1 기호 : 희진이 생성한 풀 리퀘스트 검토

기호는 [Pull requests] 클릭 -> 희진이 생성한 풀 리퀘스트를 클릭합니다. -> 변경된 코드를 확인 후 [Files Changed] 를 사용해서 코멘트를 달거나 처리 후 [Review changes]에서 [Approve]를 클릭합니다. -> 리뷰한 내용을 적습니다. - [Submit review]를 클릭하여 풀 리퀘스트를 승인합니다.

![image](https://user-images.githubusercontent.com/79847020/146183845-c3c884d3-699c-47b9-982c-42b664accd1c.png)

### 6.5.2 희진 : 기호에게 승인 받은 풀 리퀘스트 변경 내역 병합

희진은 이제 승인받은 풀 리퀘스트를 기준 브랜치에 병합합니다.

[Pull requests] 클릭 

희진이 생성한 풀 리퀘스트(Change content in main page)를 클릭합니다. 

[Merge pull request]를 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/146184284-8c275edd-df17-4c10-8507-125217d9fea4.png)

[Confirm merge] 를 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/146184527-a7f8dcec-8756-4383-b6c1-16e39cd5f2df.png)

![image](https://user-images.githubusercontent.com/79847020/146184568-905557f6-126a-4c4b-9e42-063d4550de48.png)

풀 리퀘스트에 관련 이슈를 연결했기 때문에 병합이 완료되면 자동으로 해당 이슈가 닫힙니다. 또한 프로젝트 보드에서도 자동으로 [Done] 작업 상태열로 이동합니다.

### 6.5.3 원격 저장소의 최신 변경 내역 반영 : git pull

새로운 기능이 원격 저장소의 기준 브랜치에 반영되었으니 지역 저장소의 기준 브랜치에도 반영하며 작업을 마무리 합니다.

지역 저장소에서 기준 브랜치로 작업 브랜치를 변경합니다.

```CONSOLE
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

원격 저장소의 최신 변경 내역을 가져옵니다.

```CONSOLE
$ git pull origin main
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), 639 bytes | 213.00 KiB/s, done.
From https://github.com/steadyin/mastering-git-github
 * branch            main       -> FETCH_HEAD
   42e75c5..24b83cd  main       -> origin/main
Updating 42e75c5..24b83cd
Fast-forward
 public/index.html | 2 ++
 1 file changed, 2 insertions(+)
```

지역 저장소의 커밋 내역을 확인합니다.

```CONSOLE
$ git log --pretty=oneline --graph
*   24b83cdbfe3d22d164b6a5675e1f949ae52dd126 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #3 from steadyin/feature/change-main-content
|\
| * 547a35303a8246d45facaf6ca469779f4c4cc1c3 (origin/feature/change-main-content, feature/change-main-content) Change content in main page
|/
*   42e75c59f307aa3bfcd199537be336c548ac8e08 Merge pull request #1 from steadyin/feature/body-change
|\
| * ad1fdc9c99e1fb94c6b5ac40a30409873a2d82b2 (origin/feature/body-change) Change body
| *   cdee7661147aeb54da975cbbf5347a95bb8f6fea Resolve conflicts
| |\
| | * b948ab2097ac69841307dd1578af879cc27b9495 Change header
| |/
|/|
| *   bd4c5ac76c9fabf9eef181c5c6e2faf847612e6d Merge branch 'test/local-branch'
| |\
| | * 77b2492308b22f3b7f70597746c6c511145ebcab Change header
| |/
|/|
| * 69cf6a9a58a3b769ce74907becef147012c1f231 Change title
|/
* 9817fe4a0168fd16fdb0a0ace2cfff14b6051a5a (origin/test/remote-branch, origin/test/local-branch) Add hotline to main page
* 9561e568239ce23aba4a33bed05ec85ac5be3af1 Change the title of main page
* 832b2e0cfa4998867de8e9e564bc9f26c1a15696 Add initial files and .gitignore

stead@LAPTOP-07UNIOG3 MINGW64 /c/study/05.GIT/git-github-programming2/mastering-git-github (main)
```

수정 사항이 홈페이지에 잘 반영되었는지 확인할 차례입니다. 

![image](https://user-images.githubusercontent.com/79847020/146185160-a9552703-e666-4349-8584-45d87bcfb72d.png)

# 마무리

이번 장에서는 개발자가 협업하는 경우를 위한 일련의 과정을 실습했습니다.

1. 이슈 생성 및 프로젝트 반영
2. 이슈 담당자 지정
3. 작업 브랜치 생성
4. 기능 개발 후 풀 리퀘스트 생성
5. 풀 리퀘스트 검토 및 승인
6. 기준 브랜치에 병합







