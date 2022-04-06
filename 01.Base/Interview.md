# 1 Array는 어떤 자료 구조인가요 ?

Array는 연관된 Data를 `메모리상에 연속적`이며 `순차적`으로 미리 `할당된 크기`만큼 저장하는 자료구조 입니다.

## 1.1 Array와 LinkedList의 차이점은 ?

가장 큰 차이점은 메모리에 저장되는 방식과 Operation의 연산 속도(Time Complexity) 입니다.

```java
int array[4] = {1,2,3,4};
```
다음 코드는 메모리에는 다음과 같이 저장됩니다.

|주소|데이터|
|---|---|
|0x4AF55|00000001|
|0x4AF56|00000000|
|0x4AF57|00000000|
|0x4AF58|00000000|
|0x4AF59|00000010|
|0x4AF60|00000000|
|0x4AF61|00000000|
|0x4AF62|00000000|
|0x4AF63|00000011|
|0x4AF64|00000000|
|0x4AF65|00000000|
|0x4AF66|00000000|
|0x4AF67|00000100|
|0x4AF68|00000000|
|0x4AF69|00000000|
|0x4AF70|00000000|

그리고 주소의 첫번째 값이 해당 데이터의 명시적인 주소가 됩니다. int 타입이기 때문에 값을 표현하기 위해서 4byte가 사용되고 Array는 메모리에 연속적인 공간에 할당됩니다. 따라서 위와 같은 Array의 경우 0x4AF%% + 4를 계산으로 다음 배열 값의 주소를 직접 계산할 수 있습니다. 이를 Random Access라고 합니다.

## 1.2 Array의 특징
* 고정된 저장 공간(Fixed-Size)
* 순차적인 데이터 저장(Order)

## 1.3 Array의 Operation Time Complexity
* 조회(lookup) : O(1) - Random Access
* 마지막 인덱스에 추가(append) : O(1)
* 마지막 인덱스에 삭제 : O(1)
* 삽입(insert) : O(N)
* 삭제(delete) : O(N)
* 탐색(search) : O(N)

## 1.4 Array의 장점
* lookup과 append가 빠르다는 것입니다. 따라서 조회를 자주 해야하는 작업에는 Array 자료구조를 많이 사용합니다.
 
## 1.5 Array의 단점
* fixed-size 특성상 선언시에 Array의 크기를 미리 정해야 한다는 것입니다. 이는 메모리 낭비나 추가적인 overhead가 발생할 수 있습니다. (Overhead-처리비용)

## 1.6 미리 예상한 것보다 더 많은 수의 data를 저장하느라 Array의 sizes를 넘어서게 됐습니다. 이 때 어떻게 해결할 수 있을까요 ?

* 기존의 size보다 더 큰 Array를 선언하여 데이터를 옮겨 할당하니다. 모든 데이터를 옮겼다면 기존 Array는 메모리에서 삭제하면 됩니다. 이런식으로 동적으로 배열의 크기를 조절하는 자료구조를 Dynamic(Re-sizable) Array라고 합니다. 자바의 Array List는 Dynamic Array입니다.

* 또 다른 방법으로는 size를 예측하기 쉽지 않다면 Array대신 LinkedList를 사용함으로써 데이터가 추가될 때 마다 메모리 공간을 할당받는 방식을 사용하면 됩니다.

# 2. Dynamic Array는 어떤 자료구조인가요 ?

Array의 경우 size가 고정되었기 때문에 선언시에 설정한 size보다 많은 갯수의 dat가 추가되면 저장할 수 없습니다. 이에 반해 Dynamic Array는 저장 공간이 가득 차게 되면 resize를 하여 유동적으로 size를 조절하여 데이터를 저장하는 자료구조입니다.

Array의 특징 중에 fixed-size의 한계점을 보완하고자 고안된 자료구조인 Dynamic Array에 대해서 크게 두가지를 알아야 합니다. 
1. resize를 하는 방법
2. 데이터를 추가(append)할 때의 시간 복잡도

## 2.1 Dynamic Array Resize

![img_2](https://user-images.githubusercontent.com/79847020/161904667-bdf6cb85-f7c2-46a2-b8ba-fc73e39e361b.png)

Dynamic Array는 size를 자동적으로 resizeng하는 Array입니다. 기존에 고정된 size를 가진 Static Array의 한계점을 보안하고자 고안되었습니다. Dynamic Array는 data를 계속 추가하다가 기존에 할당된 memory를 초과하게 되면 size를 늘린 배열을 선언하고 그곳으로 모든 데이터를 옮김으로써 늘어난 크기의 size를 가진 배열이 됩니다. 이를 resize라고 합니다. 이로써 새로운 data를 저장할 수 있게 됩니다. 따라서 Dyanamic Array는 Size를 미리 고민할 필요없다는 장점이 있습니다.

resizeing을 하는 방법을 여러 가지가 있는데 대표적으로 기존 Array size의 2배 size를 할당하는 Doubling이 있습니다. 

### Doubling

resize의 대표적인 방법으로 Doubling이 있습니다. 데이터를 추가 append(O(1)) 하다가 메모리를 초과하게 되며 기존 배열의 size보다 두배 큰 배열을 선언하고 데이터를 일일이 옮기는 (n개의 데이터를 일일이 옮겨야 하므로 O(n)) 방법 입니다.

### 분할 상환 시간 복잡도 Amortized time compexity

Dynamic Array에 데이터를 추가할 때마다 O(1)의 시간이 걸리게 됩니다. -> 추가를 하다가 미리 선언된 size를 넘어서는 순간에 resize를 하게 됩니다 -> 이 때 일일이 데이터를 옮겨야 되기 때문에 O(n)의 시간이 걸리게 됩니다. 

### 그렇다면 결과적으로 append의 시간복잡도는 O(1)일까요? 아니면 O(n)일까요?

append의 총 과정을 살펴보면 데이터를 마지막 인덱스에 추가하는 O(1)작업이 대다수이고 size를 넘어설 때는 size를 두 배 늘리고 데이터를 일일이 옮기는 과정 (resize O(n)) 이 가끔 발생합니다. 결론부터 말하자면 append의 전체적인 시간복잡도는 O(1) 입니다. 좀 더 정확히 말하면 amortized O(1)이라고 부릅니다.

쉽게 설명하자면 가끔 발생하는 O(n)의 resizes하는 시간을 자주 발생하는 O(1)의 작업들이 분담해서 나눠가짐으로서 전체적으로 O(1)의 시간이 걸린다고 생각하시면 됩니다.

## 2.2 Dynamic Array를 Linked List와 비교하여 장단점을 설명해주세요.

### Linked List와 비교했을 때 Dynamic Array의 장점

* 데이터 접근과 할당이 O(1)로 굉장히 빠릅니다. 이는 index 접근하는 방법이 산술적인 연산 [배열 첫 data의 주소값] + [offset] 으로 이루어져 있기 때문입니다. (Random Access)
* Dynamic Array의 맨 뒤에 데이터를 추가하거나 삭제하는 것이 상대적으로 빠릅니다.(O(1))

### Linked List와 비교했을 때 Dynamic Array의 단점

* Dynamic Array의 맨 끝이 아닌 곳에 data를 insert or remove 할 때 느린편입니다.(O(n)). 느린 이유는 메모리상에서 연속적인 데이터들일 저장되어 있기 때문에 데이터를 추가 삭제할 때 뒤에 있는 data들을 모두 한칸씩 shift 해야되기 때문입니다.
* resize를 해야할 때 예상치 못하게 현저히 낮은 performance가 발생합니다.
* resize에 시간이 많이 걸리므로 필요한 것 이상 memory 공간을 할당받습니다. 따라서 사용하지 않고 있는 낭비되는 메모리공간이 발생합니다.



# Process & Thread

# Process를 간단히 설명해주세요.

실행파일(Program)이 Memory에 적재되어 CPU를 할당받아 실행되는 것을 Process라고 합니다.

운영체제를 관통하는 핵심적인 단어 하나를 뽑는다면 바로 Process 입니다. 운영체제가 작동하는 다양한 원리들이 바로 process를 위해 존재하는 것입니다. Procee를 memory와 cpu관점으로 면접관에게 설명을 하시면 됩니다.

![img_3](https://user-images.githubusercontent.com/79847020/161904680-72178e79-aabc-4221-821b-28fd97b0465f.png)

Coding을 해서 Compile을 하면 Program이 생성됩니다. Program을 실행하면 CPU가 프로그램을 읽어야 하는데 하드디스크에 있는 Program을 읽을 수 없습니다. Ram 메모리에 올라가 있는 프로그램만 읽을 수 있습니다. 따라서 Program을 Ram 메모리에 적재를 합니다. 그렇게 되면 CPU가 Ram 메모리에 있는 명령어 한줄 한줄을 읽을 수 있게 됩니다. 

정리를 하면 Program이 메모리에 적재 된 다음에 CPU이 할당이 되면 Process가 되는 겁니다. 앞으로 우리가 언급하는 Memory는 Ram 메모리를 의미합니다. 

Process에서도 용도에 따라 메모리 영역이 구분되어 있습니다. code영역, data영역, heap영역, stack영역 4가지가 있습니다. 

code영역은 실제 코드를 저장하는 영역입니다. coding을 해서 compile한 후에 기계어로 번역한 코드를 적재를 해두면 CPU가 코드영역을 읽으면서 처리를 하게 되는 겁니다. 그리고 전역변수는 data영역에 저장을 하고 Stack영역에는 함수에서 쓰이는 지역변수나 매개변수들을 Stack영역에 저장하게 됩니다. Heap영역은 Runtime 중에 메모리에 할당되는 동적 메모리 할당을 Heap영역에 적재합니다. 

## Process

프로세스(Process)란 실행중인 프로그램(Program in Execution)을 뜻합니다. 즉 실행 파일 형태로 존재하던 Program이 Memory에 적재되어 CPU에 의해 실행(연산)되는 것을 Process라고 합니다.
(+ Program은 단순히 명령어 리스트를 포함하는 파일입니다.)

## Memory

Memory는 CPU가 직접 접근할 수 있는 컴퓨터 내부의 기억장치 입니다. Program이 CPU에서 실행되려면 해당 내용이 Memory에 적재된 상태여야만 합니다. 프로세스에 할당되는 Memory 공간은 Code, Data, Stack, Heap 4개의 영역으로 이루어져 있으며 각 Process마다 독립적으로 할당을 받습니다.

Code 영역 : 실행한 프로그램의 코드가 저장되는 메모리 영역
Data 영역 : 프로그램의 전역 변수와 static 변수가 저장되는 메모리 영역
Heap 영역 : 프로그래머가 직접 공간을 할당(malloc)/해제(free) 하는 메모리 영역
Stack 영역 : 함수 호출시 생성되는 지역 변수와 매개 변수가 저장되는 임시 메모리 영역

![img_4](https://user-images.githubusercontent.com/79847020/161904696-f9ceff5f-d2e1-4fc8-be9f-1b32da97b451.png)

## CPU의 연산과 PC Register

프로그램의 코드를 토대로 CPU가 실제로 연산을 해야 프로그램이 실행된다고 볼 수 있습니다. 그럼 어떤 코드를 읽어야 하는가를 정하는 것은 CPU 내부에 있는 PC(Program Counter) Register에 저장되어 있습니다. PC Register에는 다음에 실행될 코드(명령어, instruction)의 주소값이 저장되어 있습니다. 즉 Memory에 적재되어 있는 process code영역의 명령어중 다음 번 연산에서 읽어야할 명령어의 주소값을 PC Register가 순차적으로 가리키게 되고 해당 명령어를 읽어와서 CPU가 연산을 하게 되면 Process가 실행되는 것입니다.

## Process의 Memory영역(Code, Data, Stack, Head)에 대해서 설명해주세요

프로세스가 운영체제에서 할당받는 메모리 공간은 Code, Data, Stack, Heap 영역으로 구분됩니다. 

Code 영역 : 실행한 프로그램의 코드가 저장되는 메모리 영역
Data 영역 : 프로그램의 전역 변수와 static 변수가 저장되는 메모리 영역
Heap 영역 : 프로그래머가 직접 공간을 할당(malloc)/해제(free) 하는 메모리 영역
Stack 영역 : 함수 호출시 생성되는 지역 변수와 매개 변수가 저장되는 임시 메모리 영역

# Multi Process에 대해서 설명해 주세요.

Multi Process란 2개 이상의 Process가 동시에 실행되는 것을 말합니다. 동시에라는 말은 동시성(Concurrency)과 병렬성(parallelism) 두가지를 의미합니다. 동시성은 CPU core가 1개 일 때 여러 Process를 짧은 시간동안 번갈아 가면서 연산을 하게 되는 시분할 시스템(Time Sharing System)으로 실행되는 것입니다. 병렬성은 CPU Core가 여러개일 때, 각각의 core가 각각의 process를 연산함으로서 procee가 동시에 실행되는 것입니다.

요새 우리가 쓰는 노트북은 CPU core가 여러개 있습니다. core가 여러개여서 실제로 여러 process가 동시에 처리되는 것을 병렬성이라고 합니다. 병렬성은 의미만 이해하고 넘어가면 됩니다. 면접에서 병렬성보다 훨씬 중요한 것은 동시성입니다. 한 개의 CPU core는 당연히 한번에 하나의 연산밖에 못합니다. 그런데 도데체 어떻게 동시에 여러 process를 처리할까요? 정답은 동시성입니다. 동시성을 통해 multi process가 작동되는 원리를 잘 이해하시면 됩니다.면접에서는 시분할 시스템을 시작으로 context, PCB, context switching, process의 memory영역을 설명하시면 완벽한 답변이 될거에요.

## 동시성(Concurrency) vs 병렬성(Parallelism)

|동시성|병렬성|
|---|---|
|Single Core|Multi Core|
|동시에 실행되는 것 같이 보인다.|실제 동시에 여러 작업이 처리 된다.|

앞으로 설명할 모든 내용은 Single Core의 동시성에 초점이 맞춰져 있습니다.

## MultiProcess

MultiProcess란 2개 이상의 process가 동시에 실행되는 것을 말합니다. 이 때 process들은 CPU와 메모리를 공유하게 됩니다. memory의 경우에는 여러 porcess들이 각자의 memory영역을 차지하여 동시에 적재됩니다. 반면 하나의 cpu는 매 순간 하나의 process만 연산할 수 있습니다. 하지만 cpu의 처리 속도가 빨라서 수 ms 이내의 짧은 시간 동안 여러 process들이 CPU에서 번갈아 실행되기 때문에 사용자 입장에서는 여러 프로그램이 동시에 실행되는 것처럼 보입니다. 이처럼 CPU의 작업시간을 여러 process들이 조금씩 나누어 쓰는 시스템을 시분할 시스템(time sharing system)이라고 부릅니다.

## 메모리 관리

여러 process가 동시에 memory에 적재된 경우 서로 다른 process의 영역을 침범하지 않도록 각 process가 자신의 memory영역에만 접근하도록 운영체제가 관리해줍니다.

![img_5](https://user-images.githubusercontent.com/79847020/161904711-d0ba3e05-c20b-413d-a01c-37684d016886.png)

## CPU의 연산과 PC Register

CPU는 PC(Program Counter) Register가 가리키고 있는 명령어를 읽어들여 연산을 진행합니다. PC register에는 다음에 실행될 명령어의 주소값이 저장되어 있습니다. multi process 시스템에서는 process1이 진행되고 있을 때는 porcess1의 code영역을 PC register가 가리키다가 process2가 진행되면 process2의 code영역을 가리키게 됩니다. CPU는 PC register가 가리키는 곳에 따라 process를 변경해가면서 명령어를 읽어들이고 연산을 하게 됩니다.

Memory의 경우에는 여러 process들이 각자의 memory영역을 차지하여 동시에 적재 됩니다. 반면 하나의 CPU는 매 순간 하나의 process만 연산할 수 있습니다. 하지만 CPU의 처리 속도가 워낙 빨라서 수 ms 이내의 짧은 시간 동아 녀얼 process들이 CPU에서 번갈아 실행되기 때문에 

![img_6](https://user-images.githubusercontent.com/79847020/161904722-9a70dc87-e502-4af7-9d22-0a960a7c6cca.png)

### Context

시분할 시스템에서는 한 process가 매우 짧은 시간동안 CPU를 점유하여 일정부분의 명령을 수행하고, 다른 process에게 넘깁니다. 그 후 차례가 되면 다시 CPU를 점유하여 명령을 수행합니다. 따라서 `이전에 어디까지 명령을 수행했고, register에는 어떤 값이 저장되어 있었는지에 대한 정보`가 필요하게 됩니다. `process가 현재 어떤 상태로 수행되고 있는지에 대한 총체적인 정보가 바로 context입니다.` context 정보들은 PCB(Process Control Block)에 저장을 합니다.

### PCB(Process Control Block)

PCB는 운영 체제가 프로세스를 표현한 자료구조 입니다. PCB에는 프로세스의 중요한 정보가 포함되어 있기 때문에, 일반 사용자가 접근하지 못하도록 보호된 메모리 영역 안에 저장이 됩니다. 일부 운영 체제에서 PCB는 커널 스택에 위치합니다. 이 메모리 영역은 보호를 받으면서도 비교적 접근하기가 편리하기 때문입니다.

참고로 커널 스택은 운영체제 자체를 커널 스택이라고 표현해도 무방합니다.

PCB에는 일반적으로 다음과 같은 정보가 포함됩니다.

| PCB |  |
| --- | --- |
| Process State | new, running, waiting, halted 등의 state가 있다. |
| Process Number | 해당 process의 number |
| Program counter(PC) | 해당 process가 다음에 실행할 명령어의 주소를 가리킨다 |
| Registers | 컴퓨터 구조에 따라 다양한 수와 유형을 가진 register 값들 |
| Memory limits | base register, limit register, page table 또는 segment table 등 |
| ... |  |

Process State 이 프로세스가 실행되고 있는지, CPU 할당을 받지 못해서 기다리고 있는지, 따로 어떤 작업을 해서 멈춰 있는지에 대한 정보를 나타냅니다. Process Number은 Process의 고유한 식별 번호, PC, Registers, Memory limits 등이 저장되어 있습니다.

![img_9](https://user-images.githubusercontent.com/79847020/161904730-8ef9344f-937f-4ef7-abf4-fbdd41fbe241.png)

메모리에 프로세스들이 올라가 있습니다. 그런데 kernel이 맨 위에 있습니다. kernel은 운영체제를 의미합니다. 운영체제도 하나의 프로세스 입니다. 운영체제라는 프로세스가 돌아갈 때도 정보와 코드들이 메모리에 올라가야 실행이 되겠죠. 그래서 커널영역은 컴퓨터를 키는 순간부터 메모리에 적재되서 실행됩니다.     

Process의 Context를 데이터로 저장한 PCB1과 PCB2, PCB정보를 커널에 저장합니다. 

### Context switch

Context switch란 한 프로세스에서 다른 프로세스로 `CPU 제어권을 넘겨`주는 것을 말합니다.

이 때 이전의 프로세스의 상태를 `PCB에 저장하여 보관`하고 `새로운 프로세스`의 `PCB를 읽어서 보관된 상태를 복구`하는 작업이 이루어집니다.

 ![img_10](https://user-images.githubusercontent.com/79847020/161904739-31edd9af-8963-4439-bf9b-b0bf8b522f5e.png)

### Register ?

실제로 컴퓨터에서 데이터를 영구적으로 저장하기 위해서는 하드디스크를 이용해야 하고, 임시적으로 저장하는 장소를 메모리(RAM) 이용합니다. 하지만, 메모리로 연산의 결과를 보내고 영구적으로 저장할 데이터를 하드디스크에 저장해야 하는 등의 명령을 처리하기 위해서는 이들에 대한 주소와 명령의 종류를 저장할 수 있는 기억 공간이 하나 더 필요하다. 그리고 이 공간은 무리 없이 명령을 수행하기 위해 메모리보다 빨라야 한다. 바로 이런 역할들을 하는 것이 CPU옆에 붙어있는 레지스터이다.

레지스터는 공간은 작지만 CPU와 직접 연결되어 있으므로 연산 속도가 메모리보다 실제 수십 배에서 수백 배까지 빠르다. 그리고 CPU는 자체적으로 데이터를 저장할 방법이 없기 때문에 메모리로 직접 데이터를 전송할 수 없다.
때문에 연산을 위해서는 반드시 레지스터를 거쳐야 하며, 이를 위해서 레지스터는 특정 주소를 가리키거나 값을 읽어올 수 있다.

### process의 context는 무엇인가요?

**[핵심 답변]**
context란 process가 현재 어떤 상태로 수행되고 있는지에 대한 정보입니다. 해당 정보는 PCB(Process Control Block)에 저장을 합니다.

### PCB(Process Control Block)에 저장되는 것들은 무엇이 있나요?

**[핵심 답변]**
PCB는 운영체제가 process에 대해 필요한 정보를 모아놓은 자료구조입니다. PCB에는 일반적으로 다음과 같은 정보가 포함됩니다.

- Process number
- Process state
- Program Counter (PC), 레지스터
- CPU 스케쥴링 정보, 우선순위
- 메모리 정보 (해당 process의 주소 공간 등)

### Context switch에 대해서 설명해 주세요.

**[핵심 답변]**

Context switch란 한 프로세스에서 다른 프로세스로 `CPU 제어권을 넘겨`주는 것을 말합니다.

이 때 이전의 프로세스의 상태를 `PCB에 저장하여 보관`하고 새로운 프로세스의 `PCB를 읽어서 보관된 상태를 복구`하는 작업이 이루어집니다.

### Process의 state에는 어떤 것들이 있나요?

**[핵심 답변]**

프로세스는 `실행`(running), `준비`(ready), `봉쇄`(wait, sleep, blocked) 세 가지 상태로 구분됩니다.

| 실행 | 프로세스가 CPU를 점유하고 명령을 수행중인 상태 |
| --- | --- |
| 준비 | CPU만 할당받으면 즉시 명령을 수행할 수 있도록 준비된 상태 |
| 봉쇄 | CPU를 할당받아도 명령을 실행할 수 없는 상태 - ex. I/O 작업을 기다리는 경우 등 |

### Multi Thread 

**[핵심 답변]**

thread는 한 process 내에서 실행되는 `동작(기능 function)의 단위`입니다. 각 thread는 속해있는 process의 Stack 메모리를 제외한 나머지 memory 영역을 공유할 수 있습니다.

Multi thread란 하나의 process가 동시에 여러개의 일을 수행할수 있도록 해주는 것입니다. 즉, 하나의 process에서(실행이 된 하나의 program에서) 여러 작업을 병렬로 처리하기 위해 multi thread를 사용합니다. multi thread에서는 한 process 내에 여러 개의 thread가 있고, 각 thread들은 Stack 메모리를 제외한 나머지 영역(Code, Data, Heap) 영역을 공유하게 됩니다.

💡 thread는 process내에서 독립적인 기능을 수행합니다. 즉, 독립적으로 함수를 호출함을 의미하고 이를 위해 stack memory가 각자 필요한 거에요. thread가 무엇인지 이해하고, multi process와 어떤 점이 다른지를 생각해보면서 공부해보시길 바랍니다.
또한, 독립적인 stack memory와 PC Register가 필요하다는 점을 잘 기억해주세요.

### Thread와 multi thread

thread는 process 내에서 독립적인 기능을 수행합니다. 각 thread가 독립적인 기능을 수행한다는 것은 독립적으로 함수를 호출함을 의미합니다.

### Stack memory &  PC register

thread가 함수를 호출하기 위해서는 인자 전달, Return Address 저장, 함수 내 지역변수 저장 등을 위한 독립적인 stack memory 공간을 필요로 합니다. 결과적으로 thread는 process로부터 Stack memory 영역은 독립적으로 할당받고, Code, Data, Heap 영역은 공유하는 형태를 갖게 됩니다.

또한 multi thread에서는 각각의 thread마다 PC register를 가지고 있어야 합니다. 그 이유는 한 process 내에서도 thread끼리 context switch가 일어나게 되는데, PC register에 code address가 저장되어 있어야 이어서 실행을 할 수 있기 때문입니다.

![img_12](https://user-images.githubusercontent.com/79847020/161904750-88866e32-445e-42f5-a299-3d2455f38948.png)

### thread는 왜 독립적인 stack memory영역이 필요한가요?


# 5. 데이터베이스 - DB구조와 설계

## Primary Key가 무엇인지 설명해주세요.

```java
Key와 관련된 기본적인 용어를 묻는 면접관들도 꽤 있습니다. 데이터베이스에서 가장 기초적인 용어들이기 때문에 대답을 하지 못하면 안됩니다. 확실하게 이해하고 넘어가면 틀릴 일이 없으므로 이번 시간에 확실히 짚고 넘어 가보겠습니다. 
```

Candidate Key(후보키) 중 선택한 Main Key로서 각 Row를 구분하는 유일한 Column을 말합니다. 그래서 기본키는 Null 값을 가질 수 없고 중복된 값을 가질 수 없습니다. 기본키는 Table당 1개만 지정해야 합니다. Candidate Key 중 선택했으므로 유일성과 최소성을 만족합니다.

### Relation

Table 중 데이터베이스에서 사용되기 위한 조건을 갖춘 것이 Relation 입니다. Relation의 제약 조건 중 가장 자주 등장하는 조건은 다음과 같습니다.

1. Table의 Cell은 단일 값을 갖는다.
2. 어떤 두 개의 Row도 동일하지 않다.

하지만 통상적으로 Relation과 Table이란 용어를 구분하지 않고 사용하기도 합니다.

![img_13](https://user-images.githubusercontent.com/79847020/161904765-8d3bc02b-aa84-417c-86cd-1361a8e51a40.png)

Table = Relation
Column = Attribute
Row = Tuple = Record

### Primary Key

Super Key(슈퍼키)는 각 Row를 유일하게 식별할 수 있는 하나 또는 그 이상의 Column 들의 집합입니다. Super Key는 `유일성`만 만족하면 슈퍼키가 될 수 있습니다.
* 유일성 : Key 값으로 특정 row을 유일하게 식별할 수 있어야 합니다.
* 예씨
  * (학번)
  * (학번, 이름)
  * (학번, 이름, 학과)
  * (주민등록번호)
  * (주민등록번호, 학과, 성별)
  * 등등

Cnadidate Key(후보키)는 Super Key 중에서 더 이상 쪼개질 수 없는 Super Key를 Cnadidate Key라고 합니다. 즉 각 row를 유일하게 식별할 수 있는 최소한의 속성들의 집합입니다.
* 최소성 : 모든 row를 유일하게 식별하는데 꼭 필요한 속성만으로 구성되어야 합니다.
* 예시
  * (학번)
  * (주민등록번호)

Primary Key(기본키)는 Candidate Key 중 선택한 Main Key로서 각 row를 구분하는 유일한 Column(Column 그룹)을 말합니다. 그래서 기본키는 Null 값을 가질 수 없고 중복된 값을 가질 수 없습니다. 기본키는 Table 당 1개만 지정해야 합니다.

Alternative Key(대체키)는 후보키가 두 개 이상일 경우 기본키로 지정이 되지 못하고 남은 후보키들을 말합니다.

![img_14](https://user-images.githubusercontent.com/79847020/161904786-09c071cc-0150-4b99-b1df-9d778bd066fd.png)

Q. Primary Key와 Foreign Key에 대해 설명 해주세요.

Primary Key는 Candidate Key 중 선택한 Main Key로서 Null값을 가질 수 없고 중복된 값을 가질 수 없습니다. Candidate Key 중 선택했으므로 유일성과 최소성을 만족합니다.

Foreign Key는 다른 Table의 Primary Key Column과 연결되는(참조되는) Table의 Column을 의미합니다.

Q. Candidate Key에 대해 설명하시오.

Candidate Key는 Table을 구성하는 Column들 중에서 row를 유일하게 식별하기 위해 사용하는 Column들, 즉 primary key로 사용할 수 있는 column들을 말합니다.

Q. Alternate Key에 대해 설명하시오.

Primary Key를 제외한 나머지 Candidate Key들을 말합니다. 대체키, 보조키라고도 부릅니다.

Q. Composite Key에 대해 설명하시오.

COmposite Key란 Table에서 각 row를 식별할 수 있는 두 개 이상의 Column으로 구성된s Candidate Key를 말합니다.

![img_15](https://user-images.githubusercontent.com/79847020/161904801-2032a7d9-35cc-4d05-a9c1-62eb63650957.png)

## 관계형 데이터베이스의 N:M 관계에 대해서 설명해 주세요.

```
N:M관계는 실제 프로젝트에서 자주 쓰이면서도 제대로 이해하기 어려울 수 있는 내용입니다. 실제 프로젝트에서 마주할 수 있는 상황을 예시로 주면서 N:M 관계를 데이터베이스를 설계해 보라는 지문을 종종 받게 됩니다.
```

관계형 데이터베이스에서 양쪽 entity 모두가 서로에게 1:N(일대다) 관계를 갖는 구조를 말합니다.

### 1:N

관계형 데이터베이스에서 하나의 entity(Table)가 관계를 맺은 entity의 여러 객체를 가질 수 있는 구조를 말합니다. 두 Table 간의 관계를 Mapping Cardinality로 표현하고 종류는 크게 다음과 같습니다.
* 1:1
* 1:N
* N:M

실무에서 가장 자주 등장하는 1:N 구조인 고객-주문 관계를 살펴보겠습니다.

![img_16](https://user-images.githubusercontent.com/79847020/161904810-4127a637-7d3d-498c-a57d-a94dd32eacb8.png)

![img_17](https://user-images.githubusercontent.com/79847020/161904825-26484446-c089-4168-8d10-aa72d525768b.png)

1:N 구조에서는 보통 Primary Key - Foreign Key를 사용하여 관계를 맺습니다.

Foreign Key(외래키)는 다른 Table의 Primary Key Column과 연결되는(참조되는) Table의 Column을 의미합니다. 즉 두 Table을 연결할 때 한 Table의 외래키가 다른 하나의 Table의 기본키가 됩니다.

### N:M

관계형 데이터베이스에서 양쪽 Entity 모두가 서로에게 1:N 관계를 갖는 구조를 말합니다.

N:M 구조에서는 보통 새로운 Table(Mapping Table)을 통해서 관계를 맺습니다. 가장 친숙한 N:M 구조인 학샙-수업 관계를 살펴보겠습니다.

![img_18](https://user-images.githubusercontent.com/79847020/161904835-5e632f73-f1be-4953-8504-036e2b105ede.png)

![img_19](https://user-images.githubusercontent.com/79847020/161904845-07775d14-935c-416b-af79-11d0da931fb5.png)

## 1:N 관계를 대해서 설명해주세요.

관계형 데이터베이서 하나의 Entity(Table)가 관계를 맺은 Entity의 여러 객체를 가질 수 있는 구조를 말합니다.

## Left Outer Join, Inner Join 차이를 설명해주세요.

Join이란 두 개 이상의 테이블을 서로 연결하여 하나의 결과를 만들어 보여주는 것을 말합니다.

`inner join`(또는 join)은 두 테이블에 모두 있는 내용만 join되는 방식입니다.

`left outer join`(또는 left join)은 왼쪽 table의 모든 행에 대해서 join을 진행합니다.

## Inner Join (내부조인)

```sql
select * from vedio inner join youtuber on vedio.y_id = youtuber.id;
```
두 테이블을 연결할 때 가장 많이 사용되는 것이 inner join 입니다. inner join은 줄여서 join으로 부르기도 합니다. 두 테이블을 join하기 위해서는 두 테이블이 1:N 관계로 연결되어야 합니다. 1:N 관계는 주로 primary key와 foreign key 관계로 맺어져 있습니다. (상호조인의 경우에는 PK-FK 관계까 아니여도 됩니다.)

![img_20](https://user-images.githubusercontent.com/79847020/161904853-0eb20113-63b9-4611-803f-172959afa8d0.png)

두 table에 공통된 데이터가 존재하는 행에 대해서만 데이터를 검색합니다.

## Left Outer Join(외부조인)

```sql
select * from vedio left join youtuber on vedio.y_id = youtuber.id;
```

왼쪽 table의 모든 데이터를 포함한 데이터를 검색합니다.

# RDB - NoSQL를 비교 설명해주세요.

```sql
 RDB를 아직 많이 쓰고 있지만 점차 NoSQL를 도입하는 영역이 넓어지고 있습니다. 따라서 프로젝트마다 RDB vs NoSQL중에 더 유리한 것을 채택해서 사용해야 하는데, 이를 위해서는 장단점과 어떤 차이가 있는지를 명확히 알아야 합니다. 
```

관계형 데이터베이스(RDB)는 사전에 엄격하게 정의된 DB Schema를 요구하는 Table 기반 데이터 구조를 갖습니다. NoSQL(비관계형 데이터베이스)은 TAble 형식이 아니라 비정형 데이터를 저장할 수 있도록 지원합니다.

RDB는 엄격한 Schema로 인해 데이터 중복이 없기 때문에 데이터 Update가 많을 때 유리합니다. NoSQL의 경우 데이터 중복으로 인해 데이터 Update시 모든 컬렉션에서 수정이 필요하기 때문에 Update가 적고 조회가 많을 때 유리합니다.

### Key-Value STorage System (NoSQL)

기존의 관계형 Database의 경우에는 단일 기업의 데이터를 다루는데 최적화 되어 있습니다. 하지만 최신 데이터들을 꼭 관계형으로 처리될 필요가 없는 경우도 많고 다뤄야 하는 데이터의 양도 훨씬 커졌습니다. 즉 Big Data라고 일컬어 지는 많은 양의 데이터를 처리하기 위한 방법으로 다양한 해결책이 나왔는데 그 중 하나의 방법이 Key-Value Storage System 입니다.

Key-Value Stores는 NoSQL System이라고 불립니다. 그 이유는 SQL을 보통 지원하지 않고 Transaction을 지원하지 않는 등 SQL을 사용하는 기존의 RDB와의 차이점 때문입니다.

### MongoDB

NoSQL의 종류는 Bigtable, DynamoDB, Cassandra, MongoDB등이 있습니다. 그 중 MongoDB의 예시를 보면서 NoSQL이 어떻게 동작하는지 살펴보겠습니다.

```sql
```sql
db.createCollection("student")

db.student.insert({"id": 2022394, "name": "Nossi", "class": ["Math", "Eng"]})
db.student.insert({"id": 2021921, "name": "Bob", "class": ["Eng"]})

db.student.find() // Fetch all students in JSON format
db.student.findOne({"id": 2022394}) // Find one matching student

db.student.remove({"name": "Nossi"}) // Delete matching students
db.student.drop() // Drops the entire collection
```

|  | RDB (SQL) | NoSQL |
| --- | --- | --- |
| 데이터 저장 모델 | table | json document / key-value / 그래프 등 |
| 개발 목적 | 데이터 중복 감소 | 애자일 / 확장가능성 / 수정가능성 |
| 예시 | Oracle, MySQL, PostgreSQL 등 | MongoDB, DynamoDB 등 |
| Schema | 엄격한 데이터 구조 | 유연한 데이터 구조 |
| ⭐장점⭐ | - 명확한 데이터구조 보장 <br> - 데이터 중복 없이 한 번만 저장 (무결성) <br> - 데이터 중복이 없어서 데이터 update <br> 용이 | - 유연하고 자유로운 데이터 구조 <br> - 새로운 필드 추가 자유로움 <br> - 수평적 확장(scale out) 용이 |
  | ⭐단점⭐ | - 시스템이 커지면 Join문이 많은 복잡한 query가 필요 <br> - 성능 향상을 위해 수직적 확장(Scale up)만 가능하여 비용이 큼 <br> - 데이터 구조가 유연하지 못함 | - 데이터 중복 발생 가능 <br> - 중복 데이터가 많기 때문에 데이터 변경 시 모든 컬렉션에서 수정이 필요함 <br> - 명확한 데이터구조 보장 X |
  | ⭐사용⭐ | - 데이터 구조가 변경될 여지가 없이 명확한 경우 <br> - 데이터 update가 잦은 시스템 (중복 데이터가 없으므로 변경에 유리) | - 정확한 데이터 구조가 정해지지 않은 경우 <br> - Update가 자주 이루어지지 않는 경우 (조회가 많은 경우) <br> - 데이터 양이 매우 많은 경우 (scale out 가능) |

Q. Nosql은 언제 사용하면 좋을까요?

NoSQL은 정확한 데이터 구조가 정해지지 않은 경우, 데이터 update가 자주 이루어지지 않고 조회가 많은 경우, 또 scale out이 가능하므로 데이터 양이 매우 많은 경우에 사용하면 좋습니다.

Q. RDB는 언제 사용하면 좋을까요?

RDB는 데이터 구조가 명확하여 변경될 여지가 없는 경우, 또 데이터 중복이 없으므로 데이터 update가 잦은 시스템에서 사용하면 좋습니다.


# Transaction

Transaction은 데이터베이스 내에서 수행되는 작업의 최소 단위로 데이터베이스의 무결성을 유지하며 DB의 상태를 변화시키는 기능을 수행합니다. Transaction은 하나 이상의 Query를 포함해야 하고 ACID라고 칭해지는 원자성, 일관성, 고립성, 지속성의 4가지 규칙을 만족해야 합니다.

```sql
데이터베이스 면접질문에서 Index다음으로 가장 자주 나오는 질문이 Transaction과 Deadlock 입니다. 데이터베이스의 transaction이 안전하게 수행된다는 것을 보장하기 위한 성질 4가지 ACID를 종종 물어봅니다. Transaction에 대한 설명과 ACID가 무엇을 뜻하는지를 정확하게 이해하고 가시면 됩니다.
```

## Transaction

은행 시스템에서 A가 100만원을 출금해서 B에게 입금하는 상황을 생각해보겠습니다. A의 잔고에서 100만원을 출금하였는데 이 때 전산오류가 생겨서 B의 계좌에는 100만원이 입금 되지 않았습니다. 이런 상황은 전산시스템의 치명적인 오류입니다. 이렇게 예상치 못한게 오류가 발생하여 데이터의 부정합이 발생하는 경우 다시 원상 복귀해야 합니다. 따라서 모든 입출금은 하나의 묶음 형태로 동작해야 합니다. 출금을 했으면 입금을 마치던지 아니면 아예 없던 일로 되어야 합니다. 이런 식으로 두 행위를 아예 분리될 수 없는 하나의 거래로 처리돼야 하는 단일 업무 입니다. 이러한 업무 처리의 최소 단위를 데이터베이스에서 Transaction이라고 합니다.

데이터베이스에서 Transaction이 필요한 이유는 데이터를 다룰 때 장애가 일어나는 경우 Transaction은 장애 발생시 데이터를 복구하는 작업의 단위가 되기 때문입니다. 또한 데이터베이스에서 여러 작업이 동시에 같은 데이터를 다룰 때가 있습니다. Transaction을 통해 이 작업을 서로 분리하고 이를 통해 오류가 발생하지 않게 합니다. DBMS는 Transaction이 이러한 규칙을 유지하도록 지원합니다.

```sql
START TRANSACTION

// (1) A 계좌 잔액 가져옴 A = 1000
// (2) B 계좌 잔액 가져옴 B = 1000

// (3) A 출금  A = A - 100
// (4) B 입금  B = B + 100
UPDATE Customer SET balance = balance - 100 WHERE name='A';
UPDATE Customer SET balance = balance + 100 WHERE name='B';

//COMMIT

// (5) A 계좌 잔액 저장 A = 900
// (6) B 계좌 잔액 저장 B = 1100

COMMIT
```

## ACID







