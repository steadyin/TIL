# 시간복잡도(Time Complexity)와 공간복잡도(Space Complexity)

시간복잡도와 공간복잡도는 서로 다른 알고리즘의 성능을 비교하는 방법이다.
시간 복잡도는 입력 데이터로 함수를 실행하기 위해 걸리는 시간을 수치화한다.
공간 복잡도는 입력 데이터로 함수를 실행하기 위해 사용되는 메모리을 수치화한다.

## 증가기준(Order of Growth)이란 무엇인가? 

증가기준은 자료의 수 N이 점점 증가할 때 해당 알고리즘이 수행을 완료하는데 얼만큼 시간 T(N)이 걸리는지 그 경향성을 나타냅니다.
일반적으로 N에 대한 함수 T(N)으로 표기합니다. 흔히 시간복잡도라고 일컬어집니다. 알고리즘의 쉬운비교를 위해 T(N)에 대해서 여러가지 단계가 있습니다.

![image](https://user-images.githubusercontent.com/79847020/133103959-bd1fcc25-413e-45e8-ac91-39e9774e8c0d.png)

1. 상수형, constant - `1` 
2. 로그형, logarithmic - `logN`
3. 선형, linear - `N` 
4. 선형 로그형, linearithmic - `N log N`
5. 2차형, quadratic - `N`<sup>`2`</sup>
6. 3차형, cubic - `N`<sup>`3`</sup>
7. 지수형, exponential - `2`<sup>`N`</sup>

## 시간복잡도(Time Complexity)

시간복잡도을 표현하기 위해 다양한 표기법이 존재한다.

O-notation: 점근 상항을 나타내기 위해 사용하는 표기법<br>
Ω-notation: 점근 하한을 나타내기 위해 사용하는 표기법<br>
Θ-notation: 점근 평균을 나타내기 위해 사용하는 표기법<br>

![image 4](https://user-images.githubusercontent.com/79847020/133105914-32e63725-fe52-4a39-ba31-3cb062415565.png)

알고리즘을 분석하는 동안 우리는 대개 O-notation(Big-O Notation)을 사용합니다.
왜냐하면 프로그래밍은 항상 최악을 고려해서 작성해야 되기 때문입니다. 
Big O Notation을 계산하려면 대규모 입력의 경우 낮은 차수항은 상대적으로 영향력이 낮기 때문에 낮은 차수항은 무시합니다.

![image (1)](https://user-images.githubusercontent.com/79847020/133109040-8ac0c69d-f13a-4954-8f9c-c450e56dde3c.png)
![image (2)](https://user-images.githubusercontent.com/79847020/133109046-7d4b44b6-f5a3-4136-80c7-ccbe7fd38aa4.png)

## 빅오표기법(Big-O) 이란 무엇인가?

빅오표기법은 특정 알고리즘 수행되는데 소요되는 시간의 상한선, 쉽게 말해 가장 최악의 경우 일 경우 대략적으로 얼마나 걸리는지 계산한 것입니다.
의미적으로 해당 알고리즘이 적어도 어느정도의 시간이 지나야 어떤 경우이건 간에 수행을 완료를 보증하는가에 대한 내용입니다.

쉽게 생각하면 각 단계는 특정 알고리즘이 수행을 완료하는데 소요되는 시간을 틸드표기법으로 표기한 뒤 계수를 제거했다고 이해하면 됩니다.
소요시간이 어떻게 변화하는지 경향성을 판단하려는 것이기 때문에 상수인 계수를 제거해도 무방한 것 입니다.

![image (1)](https://user-images.githubusercontent.com/79847020/133104859-b06eaefc-8358-4b91-8c25-3efb1a30d6c8.png)

## 시간복잡도 예제

```JAVA
int count = 0;
for (int i = 0; i < N; i++)
    for (int j = 0; j < i; j++)
        count++;
````

count++ 실행 횟수를 확인하자.

i = 0 일 때, 0번 <br>
i = 1 일 때, 1번 <br>
i = 2 일 때, 2번 <br>

총 실행횟수는 

![image (3)](https://user-images.githubusercontent.com/79847020/133109293-5c687c9e-6c66-450e-92be-d165e7e215c4.png)

Big O 표기법은

![image (4)](https://user-images.githubusercontent.com/79847020/133109356-25aca228-8303-4435-9270-5b08c976f598.png)

```JAVA
int count = 0;
for (int i = 0; i < N; i++)
    for (int j = 0; j < i; j++)
        count++;
```
![image (5)](https://user-images.githubusercontent.com/79847020/133109479-e95345d2-988d-4071-9e40-1354205bae0c.png)

으로 생각할 수도 있다. 다시 한번 살펴보자.

i = N 일 때, N번 <br>
i = N/2 일 때, N/2번 <br>
i = N/4 일 때, N/4번 <br>

![image (6)](https://user-images.githubusercontent.com/79847020/133109650-d2a36770-9041-4372-b60e-e78c36b7494e.png)

Big O 표기법은

![image (7)](https://user-images.githubusercontent.com/79847020/133109857-e523a361-a605-4268-9b17-086c9cfea3fc.png)

아래는 몇 가지 일반적인 시간 복잡도의 증가를 이해하는데 도움이 되는 자료이다. 알고리즘이 올바르다고 가정할 때 얻을 수 있을 정도로 충분히 빠른지 판단하는데 도움이된다.

![image (8)](https://user-images.githubusercontent.com/79847020/133109978-364e5287-017b-4bbf-ae07-e7069bcbe27a.png)


