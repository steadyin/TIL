실습을 위한 원본 파일 입니다.

```HTML
<html>

<head>
  <title>운영팀 엡 어드민 시스템</title> 
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

![image](https://user-images.githubusercontent.com/79847020/146132511-a996c8ad-5b72-4f6a-ba6e-935b4e8f64b6.png)



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
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (HEAD -> main, origin/main) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

![image](https://user-images.githubusercontent.com/79847020/146120349-0c0542ae-00db-4ba2-91a5-aa1e46f6863f.png)

main 브랜치는 가장 최근에 생성된 커밋을 `a78~~' 바라보고 있고 HEAD 포인터는 현재 작업하는 곳(브랜치)의 최종 커밋을 바라봅니다. 즉 현재 프로젝트의 HEAD 포인터는 main 브랜치에서 작업 중이며, main 브랜치는 가장 최근 커밋을 바라봅니다.

브랜치를 생성하는 방법은 2가지 입니다.
1. 깃허브 원격 저장소에서 생성 후, 지역 저장소로 가져오기
2. 지역 저장소에서 생성 후, 원격 저장소에 반영하기

## 5.1 원격 저장소에서 생성 후 지역 저장소로 가져오기

* 원격 저장소에서 branch를 생성합니다. from main 에서 알 수 있듯이 새로운 브랜치는 현재 main 브랜치의 최신 상태를 기준으로 생성됩니다.

![image](https://user-images.githubusercontent.com/79847020/144547457-87a8063f-2e36-422a-824f-27482da79891.png)

### 5.1.1 git remote update

지역 저장소에 원격 저장소의 상태르 갱신합니다. 해당 명령어는 우리가 지역 저장소에 등록한 원격 저장소의 최신 정보를 가져옵니다. 

```CONSOLE
$ git remote update
Fetching origin
From https://github.com/steadyin/mastering-git-github
* [new branch]      test/remote-branch -> origin/test/remote-branch
```

### 5.1.2 git branch -a

git branch 명령어를 통해 지역 저장소와 원격 저장소의 브랜치 정보를 확인합니다. -a 옵션은 지역 저장소와 원격 저장소의 브랜치 정보를 모두 보여줍니다. 

```CONSOLE
$ git branch -a
* main
remotes/origin/main
remotes/origin/test/remote-branch
```

main 은 지역 저장소의 main 브랜치를 의미하며 * 표시는 현재 작업 중인 브랜치를 의미합니다. 'remotes/origin' 접두사가 붙은 브랜치는 원격 저장소, 특히 origin 식별자로 등록한 원격 저장소의 브랜치를 의미합니다. 원격 저장소에는 main과 test/remote-branch 브랜치가 존재한다는 사실을 확인할 수 있습니다.

### 5.1.3 git checkout -t <브랜치명>

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

## 5.2 지역 저장소에서 깃을 통해 브랜치 생성하기

이제 지역 저장소에서 깃 명령어로 새로운 브랜치를 생성한 후 원격 저장소에 반영해보겠습니다.

```CONSOLE
$ git branch -l
main
* test/remote-branch
```

현재 작업 브랜치는 test/remote-branch 입니다. main 브랜치를 기준으로 새로운 브랜치를 생성하려면 현재 작업 브랜치를 main 브랜치로 변경해야 합니다.

### 5.2.1 git checkout 명령어를 실행해 작업 브랜치를 main 브랜치로 변경합니다.

```CONSOLE
$ git checkout main
Switched to branch 'main'

```

```CONSOLE
$ git branch -l
* main
test/remote-branch
```

### 5.2.2 git branch 명령어를 실행하여 새로운 브랜치를 생성합니다.

```CONSOLE
$ git branch test/local-branch
```

```CONSOLE
$ git branch -a
* main
  test/local-branch
  test/remote-branch
  remotes/origin/main
  remotes/origin/test/remote-branch
```

test/local-branch라는 새로운 브랜치가 정상적으로 생성됐습니다. 현재 작업 브랜치는 main 브랜치입니다. 또한 'remotes'origin' 접두사가 붙은 'test/local-branch'는 없는 것으로 보아 원격 저장소에는 아직 반영되지 않았습니다.

### 5.2.3 git checkout 명령어를 실행해 새로운 브랜치를 작업 브랜치로 변경하고 원격 저장소에 반영해봅시다.

```CONSOLE
$ git checkout test/local-branch
Switched to branch 'test/local-branch'
```

```CONSOLE
$ git push origin test/local-branch
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'test/local-branch' on GitHub by visiting:
remote:      https://github.com/steadyin/mastering-git-github/pull/new/test/local-branch
remote:
To https://github.com/steadyin/mastering-git-github.git
 * [new branch]      test/local-branch -> test/local-branch
```

origin 식별자로 저장되 원격 저장소에 test/local-branch를 반영했습니다. 

![image](https://user-images.githubusercontent.com/79847020/144549250-b8bed6a1-6fe6-4038-9b02-65841d8753c7.png)

## 5.3 브랜치 삭제하기

### 5.3.1 지역 저장소의 브랜치를 삭제합니다.

```CONSOLE
git branch -d {브랜치명}
```

### 5.3.2 원격 저장소의 브랜치를 삭제합니다.

```CONSOLE
git branch origin -d {브랜치명}
```

### 5.3.3 브랜치 삭제 실습

```JAVA
$ git branch test/deleted

$ git branch -a
  main
  test/deleted
* test/local-branch
  test/remote-branch
  remotes/origin/main
  remotes/origin/test/local-branch
  remotes/origin/test/remote-branch

$ git branch -d test/deleted
Deleted branch test/deleted (was a78a0ca).

$ git branch -a
  main
* test/local-branch
  test/remote-branch
  remotes/origin/main
  remotes/origin/test/local-branch
  remotes/origin/test/remote-branch
```

## 5.4 브랜치 병합하기

작업이 완료되면 기준 브랜치에 반영하는 작업이 필요하겠죠. 이 작업을 브랜치 병합이라고 합니다.

### 5.4.1 브랜치 병합이란 ?

두 브랜치를 비교하여 파일의 변경 내용을 비교하고 합칩니다. 

```CONSOLE
$ git checkout main
Switched to branch 'main'

$ git log --pretty=oneline --graph
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (HEAD -> main, origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch, test/local-branch) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

해당 커밋내역 상태를표현하면 다음 그림과 같습니다.

![image](https://user-images.githubusercontent.com/79847020/146131199-b73ff505-9b69-411c-8216-668c66fc4a58.png)

그림에서 보는 것처러 지역 저장소의 test/local-branch 역시 main 브랜치와 동일한 커밋을 바라보고 있습니다. 그렇다면 test/local-branch를 작업 브랜치로 변경하여 새로운 커밋을 생성한 후 우리는 어떤 작업을 해야할까요?

![image](https://user-images.githubusercontent.com/79847020/146131930-27c09823-dbc2-4a38-a74d-eb5b84a1d571.png)

test/local-branch에서 새로운 커밋을 생성했다고 가정하겠습니다.. 이 커밋은 기준 브랜치인 main 브랜치가 아직 알 수 없는 커밋이죠. 따라서 작업 브랜치(test/local-branch)에서 작업을 완료한 후 새로운 커밋을 main 브랜치에 반영하는 작업이 필요합니다. 반영 작업을 진행하면 머지 커밋(merge commit)이라는 새로운 커밋이 생성됩니다. 이것이 병합입니다. 기준 브랜치에 작업 브랜치의 새로운 커밋을 반영하는 방법은 크게 2가지이며 병합을 위한 추가적인 커밋 생성 여부를 기준으로 세웠습니다.

### 5.4.2 빨리감기 병합 : fast forward

패스트 포워드 병합은 main 브랜치를 기준으로 작업 브랜치를 생성 후 작업을 완료하여 main 브랜치에 병합을 시도합니다. 이때 main 브랜치에 새로운 커밋이 없다면 빨리감기 병합으로 진행됩니다. 즉 기준 브랜치에 작업 브랜치의 새로운 커밋이 단순히 최신 커밋으로 더해지고 기준 브랜치가 바라보는 최신 커밋만 변경됩니다.

```CONSOLE
$ git branch test/fast-forward

$ git checkout test/fast-forward
Switched to branch 'test/fast-forward'
```

test/fast-forward 브랜치를 생성 후 파일을 수정하겠습니다. 

```HTML
<html>
<head>
  <title>운영팀 엡 어드민 시스템 v1</title> 
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

파일을 수정한 후 새로운 커밋을 생성합니다.

```CONSOLE
$ git add .
warning: LF will be replaced by CRLF in public/index.html.
The file will have its original line endings in your working directory

$ git commit -m "Change title"
[test/fast-forward 68228dd] Change title
 1 file changed, 3 insertions(+), 1 deletion(-)
 ```
 
 현재 커밋 내역의 상태를 확인합시다.
 
 ```CONSOLE
 $ git log --pretty=oneline --graph
* 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (HEAD -> test/fast-forward) Change title
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch, test/local-branch, main) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

현재 작업 브랜치 test/fast-forward만 가장 최근 생성한 커밋을 바라보고 있습니다. main 브랜치를 포함한 다른 브랜치들은 이전의 커밋을 바라보고 있습니다. test/fast-forward 브랜치의 작업 내용을 main 브랜치에 병합해보겠습니다.

main 브랜치를 작업 브랜치로 변경합니다.

```CONSOLE
$ git checkout main
Switched to branch 'main'

$ git log --pretty=oneline --graph
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (HEAD -> main, origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch, test/local-branch) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

test/fast-forward 에서 생성한 커밋은 보이지 않습니다. 새로운 브랜치의 작업 내용을 병합하지 않았기 때문입니다.

test/fast-forward 브랜치의 작업 내용을 main 브랜치에 병합합니다.

```CONSOLE
$ git merge test/fast-forward
Updating a78a0ca..68228dd
Fast-forward
 public/index.html | 4 +++-
 1 file changed, 3 insertions(+), 1 deletion(-)
```
test/fast-forward를 main 브랜치를 기준으로 생성하고 test/fast-forward 브랜치의 작업 내용을 다시 main 브랜치에 병합할 때까지 main 브랜치에는 아무런 변경 내용이 없었기 때문에 빨리감기 병합 방식으로 이루어졌습니다.

* 커밋 내역을 다시 살펴봅니다.

```CONSOLE
$ git log --pretty=oneline --graph
* 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (HEAD -> main, test/fast-forward) Change title
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch, test/local-branch) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

기존에 main 브랜치에 존재하지 않았던 커밋이 추가됐고 main 브랜치와 test/fast-forward 브랜치가 같은 커밋을 바라보고 있습니다.

![image](https://user-images.githubusercontent.com/79847020/146136475-cfd7a0b4-7be2-4024-8a85-d8e7bf625433.png)

### 5.4.3 병합 커밋 생성 : merge commit

두 번째 병합 방법은 커밋 생성입니다. fast forward 병합과 다르게 기준 브랜치에 변경이 존재하는 경우에 사용하는 방법입니다. 기준 브랜치와 새로운 작업 브랜치의 변경 내용을 하나로 합치는 작업이 필요하고 이를 병합이라고 합니다. 

'test/local-branch'를 작업 브랜치로 설정하겠습니다.

```CONSOLE
$ git checkout test/local-branch
Switched to branch 'test/local-branch'

$ git log --pretty=oneline --graph --all
* 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (test/fast-forward, main) Change title
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (HEAD -> test/local-branch, origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

이전 커밋 내역을 살펴보면 test/local-branch는 현재 main 브랜치와 다른 커밋을 바라보고 있습니다. 즉 main 브랜치를 기준으로 test/local-branch 브랜치를 생성했을 때와 다르게 main 브랜치에 변경 내용이 존재합니다. 

![image](https://user-images.githubusercontent.com/79847020/144558576-ec0b2b4f-22c4-4d08-b14f-60e847e944f2.png)

변경 내용을 만들기 위해 파일을 임의로 수정합니다. index.html을 열어보면 test/local-branch 브런치를 생성할 당시에는 title 내용을 수정한 커밋이 반영되지 않은 상태였기 때문에 title의 값에 여전히 변경이 없습니다. 이번에는 Body부분을 "운영팀 웹 어드민 시스템 v1"로 수정 합니다.

```HTML
<html>
<head>
  <title>운영팀 엡 어드민 시스템</title>
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v1</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

변경한 파일 추가 및 커밋 생성 명령을 실행합니다.

```CONSOLE
$ git add .

$ git commit -m "Change body"
[test/local-branch c04532b] Change body
 1 file changed, 1 insertion(+), 1 deletion(-)
```

다시 main 브랜치를 작업 브랜치로 변경합니다.

```CONSOLE
$ git checkout main
Switched to branch 'main'

$ git log --pretty=oneline --graph --all
* c04532bc094fe1891e66c83f9972d24533ea8cef (test/local-branch) Change body
| * 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (HEAD -> main, test/fast-forward) Change title
|/
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

get merge 명령어를 이용하여 "test/local-branch" 브랜치의 작업 내용을 main 브랜치에 병합합니다.

```CONSOLE
$ git merge test/local-branch
Auto-merging public/index.html
Merge made by the 'recursive' strategy.
 public/index.html | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

빨리감기 병합과는 다르게 git merge 명령어를 실행하면 터미널 결과와 같이 커밋 작성 에디터가 나타납니다. 바로 이 부분이 기준 브랜치인 main 브랜치와 작업 브랜치인 test/local-branch 양쪽의 변경 내용을 하나로 합치는 과정입니다. 커밋 작성 에디터를 저장하고 빠져나오면 Fast-forward 대신 Auto-mergin 이라는 결과를 확인할 수 있습니다.

커밋 내역을 다시 살펴봅니다.

```CONSOLE
*   c2e8b4bf76f619cc437b6e7e55d2bd4a2623d1a9 (HEAD -> main) Merge branch 'test/local-branch'
|\
| * c04532bc094fe1891e66c83f9972d24533ea8cef (test/local-branch) Change body
* | 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (test/fast-forward) Change title
|/
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (origin/test/remote-branch, origin/test/local-branch, origin/main, test/remote-branch) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

test/fast-forward 브랜치에서 작업 후 main 브랜치에 병합된 커밋과 test/local-branch 브랜치에서 작업 후 main 브랜치에 병합된 커밋이 하나의 병합 커밋으로으로 묶여 생성됐습니다. 다시 한번 말씀 드리면 test/local-branch 브랜치를 main 브랜치 기준으로 생성할 당시에는 main 브랜치에 test/fast-forward 브랜치의 작업 커밋이 병합되지 상태였습니다. 따라서 test/local-branch 브랜치의 작업 커밋을 반영할 때 main 브랜치에 양쪽 변경 내용이 반영된 것입니다.

수정한 파일 내용을 확인합니다. 

```HTML
<html>
<head>
  <title>운영팀 엡 어드민 시스템 v1</title> 
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v1</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

![image](https://user-images.githubusercontent.com/79847020/146138407-a38e41e0-91b1-448f-a8d9-4498c064700a.png)

## 5.5 충돌 해결하기

지금까지 병합을 살펴보았고 특별한 문제 없이 병합이 완료됐습니다. 하지만 여러 개발자가 프로젝트를 진행하다 보면 병합시 충돌이 발생할 수 있습니다. 충돌이란 깃이 자동으로 병합을 완료 할 수 없는 상황을 말합니다. 대부분 두 명 이상이 각자의 작업 브랜치에서 동일한 코드를 수정한 후 이를 병합할 때 충돌이 발생합니다. 깃 입장에서는 두 브랜치가 같은 파일의 동일한 코드를 수정했기 때문에 어떤 변경 내용을 최종적으로 반영해야 하는지 알 수 없습니다.

앞에서 생성한 /test/remote-branch를 작업 브랜치로 설정하겠습니다.

```CONSOLE
$ git checkout test/remote-branch
Switched to branch 'test/remote-branch'
Your branch is up to date with 'origin/test/remote-branch'.
```

test/remote-branch는 기준 브랜치인 main 브랜치와 다른 커밋을 바라보고 있습니다. main 브랜치를 기준으로 test/remote-branch 브랜치를 생성한 이후 main 브랜치에는 새로운 커밋이 반영되었기 때문입니다.

html 파일에서 test/local-branch에서 수정했던 Body 부분을 재수정합니다.

```HTML
<html>

<head>
  <title>운영팀 엡 어드민 시스템</title>
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v2</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

커밋을 생성합니다.

```CONSOLE
$ git add .

$ git commit -m "Change header"
[test/remote-branch eea0e1b] Change header
 1 file changed, 1 insertion(+), 1 deletion(-)
```

다시 작업 브랜치를 main 브랜치로 변경합니다.

```CONSOLE
$ git log --pretty=oneline --graph --all
* eea0e1b1b781d10553b15a0dfedf191dc208c329 (HEAD -> test/remote-branch) Change header
| *   c2e8b4bf76f619cc437b6e7e55d2bd4a2623d1a9 (main) Merge branch 'test/local-branch'
| |\
| | * c04532bc094fe1891e66c83f9972d24533ea8cef (test/local-branch) Change body
| |/
|/|
| * 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (test/fast-forward) Change title
|/
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (origin/test/remote-branch, origin/test/local-branch, origin/main) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore

$ git checkout main
Switched to branch 'main'
```

test/remote-branch 브랜치의 작업 내용을 main 브랜치에 병합합니다.

```CONSOLE
$ git merge test/remote-branch
Auto-merging public/index.html
CONFLICT (content): Merge conflict in public/index.html
Automatic merge failed; fix conflicts and then commit the result.
```

충돌이 발생하여 자동 병합에 실패했습니다.

```HTML
<html>

<head>
  <title>운영팀 엡 어드민 시스템 v1</title> 
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
<<<<<<< HEAD
  <h1>운영팀 웹 어드민 시스템 v1</h1>
=======
  <h1>운영팀 웹 어드민 시스템 v2</h1>
>>>>>>> test/remote-branch
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

<<<<<<< HEAD 부터 ======= 까지는 현재 브랜치의 변경 내용입니다. main 브랜치

======= 부터 >>>>>>> test/remote-branch 까지는 병합하려는 브랜치의 내용입니다. test/remote-branch 브랜치

test/remote-branch 브랜치의 변경사항만 남겨두고 저장하겠습니다.

```HTML
<html>

<head>
  <title>운영팀 엡 어드민 시스템 v1</title> 
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v2</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템</p>
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

파일을 수정했으므로 커밋을 다시 생성합니다.

```CONSOLE
$ git add .

$ git commit -m "Resolve Conflictes"
[main 4bff91a] Resolve Conflictes

*   4bff91a6c55aeb8b33a691706788801e93c14485 (HEAD -> main) Resolve Conflictes
|\
| * eea0e1b1b781d10553b15a0dfedf191dc208c329 (test/remote-branch) Change header
* |   c2e8b4bf76f619cc437b6e7e55d2bd4a2623d1a9 Merge branch 'test/local-branch'
|\ \
| * | c04532bc094fe1891e66c83f9972d24533ea8cef (test/local-branch) Change body
| |/
* / 68228dd8b81abd5bc008e6a82dc1fe2971aa385f (test/fast-forward) Change title
|/
* a78a0ca1a432d9eb0b78fbe4a2e0605494ecc0bd (origin/test/remote-branch, origin/test/local-branch, origin/main) Add hotline to main page
* b782fcf64466f365c0f50d80769b2fba6b5d169e Change the title of main page
* 334dc7f044ed4720cad279434a18f72e46d954c7 Add initial files and .gitignore
```

충돌을 해결한 후 생성한 커밋(Resolve conflicts)이 main 브랜치가 바라보는 가장 최신 커밋으로 잘 생성이 되었습니다. 또한 test/remote-branch의 커밋 내역이 포함된 것을 확인할 수 있습니다. 

이제 병합을 하기 전, 작업 브랜치의 변경 내용을 동료들에게 공유하고 리뷰를 받는 풀 리퀘스트(pull request)를 살펴보겠습니다.

## 5.6 풀 리퀘스트 요청하기

지금까지 브랜치를 생성하고 병합하고 충돌을 해결하는 방법을 살펴보았습니다. 하지만 실제로 한 프로젝트에서 여러 개발자가 함께 작업할 때 변경 내역에 대한 동료들의 피드백 없이 기준 브랜치에 병합하는 일은 위험할 수 있습니다. 동일한 프로젝트를 기반으로 작업하기 때문에 변경 내역이 동료들의 작업에 어떻게 영향을 주는지 함께 만들어가는 프로젝트에 어떠한 변경이 생기는지 서로 확인하는 작업이 필요하기 때문이죠.

깃허브는 풀 리퀘스트 기능을 제공합니다. 이 기능을 통해 함께 작업하는 동료들에게 브랜치 병합 예정인 변경 내역 검토를 요청할 수 있습니다.

### 5.6.1 풀 리퀘스트 요청하기

풀 리퀘스트를 요청하기 위해 main 브랜치를 기준으로 새로운 브랜치를 생성한 후 작업 브랜치로 변경하겠습니다. 

```
$ git branch feature/body-change

$ git checkout feature/body-change
Switched to branch 'feature/body-change'
```

Body 영역에 <p></p> 사이에 v1를 추가했습니다.

```HTMl
<html>
<head>
  <!-- <title>운영팀 엡 어드민 시스템</title>  -->
  <title>운영팀 엡 어드민 시스템 v1</title> 
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>운영팀 웹 어드민 시스템 v2</h1>
  <p>Welcome to 운영팀 웹 어드민 시스템 v1</p> 
</body>

<footer>
  <p>연락처 : 1111-2222</p>
</footer>
</html>
```

새로운 커밋을 생성합니다.

```CONSOLE
$ git add .

$ git commit -m "Change body"
[feature/body-change 240a018] Change body
 1 file changed, 1 insertion(+), 1 deletion(-)
```

변경 내역을 포함한 feature/body-change 브랜치를 원격 저장소에 반영합니다.

```CONSOLE
$ git push origin feature/body-change
Enumerating objects: 27, done.
Counting objects: 100% (27/27), done.
Delta compression using up to 8 threads
Compressing objects: 100% (24/24), done.
Writing objects: 100% (24/24), 2.01 KiB | 1.00 MiB/s, done.
Total 24 (delta 14), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (14/14), completed with 2 local objects.
remote:
remote: Create a pull request for 'feature/body-change' on GitHub by visiting:
remote:      https://github.com/steadyin/mastering-git-github/pull/new/feature/body-change
remote:
To https://github.com/steadyin/mastering-git-github.git
 * [new branch]      feature/body-change -> feature/body-change
```

깃허브 원격 저장소(mastering-git-github) 의 메인 페이지를 클릭해 feature/body-change 브랜치가 반영되었나 확인합니다.

![image](https://user-images.githubusercontent.com/79847020/146148846-b41c7296-e03d-4eac-9ef3-2cb60b5ee64a.png)

깃허브 원격 저장소에서 풀 리퀘스트를 생성합시다. [Pull requests] 탭 -> [New pull request] 버튼을 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/146149013-e803c231-c5db-41eb-aa2d-d507f166b0d0.png)

base는 변경 내역을 병합할 브랜치로, compare는 변경 내역이 있는 작업 브랜치를 선택하는 곳입니다.

base는 main 브랜치로, compare는 feature/body-change를 선택했습니다.

하단에 파일 단위로 변경 내역이 나타납니다.

![image](https://user-images.githubusercontent.com/79847020/146154453-e77b23c0-93dd-4779-9df5-d201c0ed5794.png)

변경 내역을 확인하고 [Create pull request] 버튼을 클릭합니다.

풀 리퀘스트 내용을 적는 화면이 나옵니다. 제목과 설명을 작성 후 해당 풀리퀘스트의 변경 내용을 검토할 Reviewers를 지정합니다.

[Create pull request] 버튼을 클릭하여 풀 리퀘스트를 생성을 완료합니다.

![image](https://user-images.githubusercontent.com/79847020/146154887-32a634e6-7d6d-44d2-8162-870a8f14374f.png)

풀 리퀘스트를 생성을 완료했습니다. 이제 Reviewers로 지정된 사람은 풀 리퀘스트를 검토한 후 피드백을 주게됩니다.

풀 리퀘스트 생성 시 Reviewers를 지정하지 않아도 생성 및 기준 브랜치에 병합하기 기능을 수행할 수 있습니다. 하지만 동료들의 피드백을 받기 위해 풀 리퀘스트를 생성하기 때문에 Reviewers를 지정하고 동료들의 피드백을 받은 후 병합하는 과정이 꼭 필요합니다.

### 5.6.2 풀 리퀘스트 검토하기

Reviewer로 지정된 다른 계쩡으로 깃허브에 접속합니다.

깃허브 프로젝트에서 [Pull requests] 탭을 클릭하면 Pull Request 목록을 확인할 수 있습니다. Change Body content 를 클릭합니다.

풀 리퀘스트의 상세 페이지가 나타납니다. 

![image](https://user-images.githubusercontent.com/79847020/146155537-68a5d193-9cfb-4184-9898-dcc8651f8c8a.png)

[Files changed] 탭을 선택하면 변경내역을 파일 기준으로 확인할 수 있습니다. 이 때 + 버튼을 클릭하여 각 변경 내역에 대한 코멘트를 작성할 수도 있습니다.

![image](https://user-images.githubusercontent.com/79847020/146155814-419e402f-cc0a-440a-8590-ba5c47361df7.png)

오른쪽 상단에 [Review changes] 버튼을 클릭합니다. 

![image](https://user-images.githubusercontent.com/79847020/146155982-faf15cd7-5ab6-4924-b8a7-fc6b29be5b23.png)

코멘트를 작성하고 [Approve]을 선택하고 [Submit review] 버튼을 클릭하며 해당 풀 리퀘스트를 승인합니다.

풀 리퀘스트 승인 후 상세 페이지에서 [Reviewers]에서 승인이 완료되었다는 V 표시를 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/146156156-e7abce3c-a1a3-4476-a66f-ec41010e60b9.png)

### 5.6.3 풀 리퀘스트를 기준 브랜치에 병합하기

해당 풀 리퀘스트를 기준 브랜치에 병합해봅시다.

다시 풀 리퀘스트 생성 계정으로 전환합니다. [Pull requests]를 선택하여 상세 페이지로 이동합니다. 상세 페이지에 아래에서 [Merge pull request] 버튼이 보입니다. 병합 작업을 한느 버튼입니다. [Merge pull request] 버튼을 클릭하여 병합을 시작합니다.

![image](https://user-images.githubusercontent.com/79847020/146156543-f04f3e17-3ab5-4824-841a-7254b4523c16.png)

병합 메시지를 적고 [Confirm merge] 버튼을 클릭하여 병합 작업을 완료합니다.

![image](https://user-images.githubusercontent.com/79847020/146156590-ba5976f2-1848-452b-9f65-2234386fbe0f.png)

'Pull request successfully merged and closed' 메시지가 나타납니다. 병합이 완료되었고 풀 리퀘스트가 닫혔다는 메시지 입니다. 

![image](https://user-images.githubusercontent.com/79847020/146156712-2448c589-47ca-446b-b081-953abf3dce70.png)

깃허브 원격 저장소 > [<>Code] 에서 main 브랜치를 선택해 커밋 이력을 확인합니다. 풀 리퀘스트 내용이 정상적으로 반영되었습니다.

![image](https://user-images.githubusercontent.com/79847020/146156906-deceb8ef-c342-46c7-8885-8c8d9ca89507.png)


### 5.6.4 원격 저장소 최신 내역을 지역 저장소 브랜치 반영하기

다음 작업을 위해 지역 저장소의 기준 브랜치를 원격 저장소의 기준 브랜치와 동일하게 맞춰야 합니다. 

원격 저장소 main 브랜치에 최신 변경 내역을 반영했으니 지역 저장소 main 브랜치에도 반영합니다.

git pull 명령어는 원격 저장소의 변경 내역을 가져옵니다. origin 식별자가 가리키는 원격 저장소의 main 브랜치 변경 내역을 가져오라는 뜻이 되겠죠.

```CONSOLE
$ git checkout main
Switched to branch 'main'

$ git pull origin main
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), 638 bytes | 638.00 KiB/s, done.
From https://github.com/steadyin/mastering-git-github
 * branch            main       -> FETCH_HEAD
   9817fe4..42e75c5  main       -> origin/main
Updating cdee766..42e75c5
Fast-forward
 public/index.html | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

git commit 내역을 확인합니다.

```CONSOLE
$ git log --pretty=oneline --graph
*   42e75c59f307aa3bfcd199537be336c548ac8e08 (HEAD -> main, origin/main) Merge pull request #1 from steadyin/feature/body-change
|\
| * ad1fdc9c99e1fb94c6b5ac40a30409873a2d82b2 (origin/feature/body-change, feature/body-change) Change body
| *   cdee7661147aeb54da975cbbf5347a95bb8f6fea Resolve conflicts
| |\
| | * b948ab2097ac69841307dd1578af879cc27b9495 (test/remote-branch) Change header
| |/
|/|
| *   bd4c5ac76c9fabf9eef181c5c6e2faf847612e6d Merge branch 'test/local-branch'
| |\
| | * 77b2492308b22f3b7f70597746c6c511145ebcab (test/local-branch) Change header
| |/
|/|
| * 69cf6a9a58a3b769ce74907becef147012c1f231 (test/fast-forward) Change title
|/
* 9817fe4a0168fd16fdb0a0ace2cfff14b6051a5a (origin/test/remote-branch, origin/test/local-branch) Add hotline to main page
* 9561e568239ce23aba4a33bed05ec85ac5be3af1 Change the title of main page
* 832b2e0cfa4998867de8e9e564bc9f26c1a15696 Add initial files and .gitignore
```

### 원격 저장소의 변경 내역을 지역 저장소로 가져오는 2가지 방법 : get fetch, git pull

원격 저장소의 변경 내역을 지역 저장소로 가져오는 방법은 크게 2가지 입니다. 첫번째는 이미 학습한 git pull 그리고 다루지 않은 git fetch 입니다. git pull 은 원격 저장소의 변경 내역을 가져와서 지역 저장소의 작업 브랜치에 병합까지 완료합니다. 반면 git fetch는 원격 저장소의 변경 내역을 가져올 뿐 지역 저장소의 작업 브랜치에 병합 작업은 하지 않습니다. 즉 git fetch 후 직접 git merge 를 수행해 현재 작업 브랜치에 반영해야 합니다.



