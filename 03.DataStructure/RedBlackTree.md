## Red Black Tree

* Leonidas Guibas와 Robert Sedgewick이 1978년에 발명한 균형 잡힌 데이터 구조입니다.

* 이 데이터 구조에는 O(logN) 실행 시간이 보장됩니다.

* 이진 탐색 트리의 실행 시간은 이진 탐색 트리의 h 높이에 따라 다릅니다.

* AVL 트리는 균형이 더 엄격하지만 더 많은 작업이 필요하기 때문에 적-검정 트리보다 빠릅니다.

* 그러나 적-검정 트리가 데이터 구조를 구성하는 시간이 더 빠릅니다.

* 데이터 구조 구성후에는 AVL Tree가 더 빠릅니다.

* 운영 체제는 이러한 데이터 구조에 크게 의존합니다. 

![image](https://user-images.githubusercontent.com/79847020/137850536-5f7e484e-74d4-46e2-9dc2-e9df2a9fefe9.png)

  |---|Average-case|Worst-case|
  |---|---|---|
  |space complexity|O(N)|O(N)|
  |insertion|O(logN)|O(logN)|
  |deletion(removal)|O(logN)|O(logN)|
  |search|O(logN)|O(logN)|
  
* 각 노드는 빨간색 또는 검은색입니다.

* 루트 노드는 항상 검은색입니다.
 
* 모든 리드 노드(NULL 포인터)는 검은색입니다.

* 모든 레드 노드에는 두 개의 검은색 자식 노드가 있어야 하며 다른 자식 노드는 없어야 합니다. 그것은 흑인 부모가 있어야합니다

* 주어진 노드에서 하위 NULL 노드까지의 모든 경로는 동일한 수의 블랙 노드를 포함합니다. 

![image](https://user-images.githubusercontent.com/79847020/137850653-ea98acc0-e346-428c-8a86-293f08cdbaf9.png)

1) 모든 레드 노드에는 두 개의 블랙 자식이 있어야 합니다.

2) 주어진 노드에서 모든 하위 NULL 노드까지의 모든 경로는 동일한 m개의 블랙 노드를 포함합니다.

![image](https://user-images.githubusercontent.com/79847020/137856486-9c8fe8bf-b198-4d1b-a115-f356af491d4d.png)

![image](https://user-images.githubusercontent.com/79847020/137857184-516560d6-ff6a-4fb4-b9d0-aaad0b7d0712.png)

루트에서 리프 노드까지의 최단 경로가 m개의 블랙 노드를 포함한다고 가정해 봅시다.

예를 들어 검은 노드만 존재하는 이 트리는 Red-Black 트리입니다.

따라서 레드-블랙트리에서 가능한 제일 짧은 리프 노드까지의 거리는 블랙 노드만 있는 경우라는 것을 알 수 있습니다.

더 긴 경로를 구성하려면 새 레드 노드를 삽입해야 하지만 1) 때문에 레드 노드만 삽입할 수 없습니다.

따라서 가능한 가장 긴 경로는 2m 노드를 포함합니다(빨간색과 검은색 교대) 2) 모든 최대 경로에는 동일한 수의 검은색 노드가 있습니다.

어떤 경로도 트리의 다른 경로보다 두 배 이상 길지 않습니다.

Case 1

![image](https://user-images.githubusercontent.com/79847020/138192079-398fba68-36bf-4ce8-8780-b9d3ed148eae.png)

이것은 일부 노드를 다시 칠해야 하는 비교적 간단한 경우입니다.

그러면 x 노드가 루트 노드가 됩니다.

그래서 우리는 이진 검색 트리의 다른 부분에서 red-black 속성을 위반할 수 있습니다.

O(logN) 실행 시간에 x부터 루트 노드까지 속성을 재귀적으로 확인해야 합니다. 

Case 2

![image](https://user-images.githubusercontent.com/79847020/138192120-c36c3931-a3b2-4089-ba1a-b6745405ffb7.png)

이것은 x의 부모가 레드 노드이고 삼촌이 블랙인 경우입니다.

노드 x의 부모에서 왼쪽 회전/(대칭형:오른쪽)을 했습니다.

따라서 회전 작업을 사용하면 이진 검색 트리의 다른 부분에서 빨강-검정 속성을 위반할 수 있습니다.

O(logN) 실행 시간에 x부터 루트 노드까지 속성을 재귀적으로 확인해야 합니다.

Case 3

이것은 x의 부모가 레드 노드이고 삼촌이 블랙인 경우입니다.

이 경우 x 노드는 왼쪽 자식입니다.

노드 x의 조부모를 오른쪽/(대칭형:왼쪽)으로 회전해야 합니다 + RECOLORING !!!

O(logN) 실행 시간에 x부터 루트 노드까지 속성을 재귀적으로 확인해야 합니다.

![image](https://user-images.githubusercontent.com/79847020/138196434-192362c1-3421-4167-8871-48aad82b7e4b.png)
 
Case 4

![image](https://user-images.githubusercontent.com/79847020/138196701-ee54e245-49c8-475a-9a66-a02c89454369.png)

------------------------------------------------------------------------------------------------------------------------------------------------

1. Insert(1)

![image](https://user-images.githubusercontent.com/79847020/138202123-29765cc5-aec3-4b15-9bc4-e942f3385afb.png)

삽입 한 새로운 노드는 항상 레드입니다. 하지만 레드-블랙트리에서 루트노드는 블랙 입니다. 따라서 Recolor 합니다.

2. Insert(2)

![image](https://user-images.githubusercontent.com/79847020/138202286-bb6ae4a6-d782-4314-928c-6372b4880466.png)

레드-블랙 트리도 접근시 루트노드를 통해서만 접근할 수 있습니다.

새로 삽입할 노드 2는 루트 노드 1 보다 크므로 오른쪽에 삽입합니다.

3. Insert(3)

![image](https://user-images.githubusercontent.com/79847020/138203529-5fa29dca-a6e5-4dc3-9c09-3b3ffb769636.png)

4. Insert(4)

![image](https://user-images.githubusercontent.com/79847020/138204098-dacbba55-9181-4946-98f1-e5a9ba9e5ab0.png)

5. Insert(5)

![image](https://user-images.githubusercontent.com/79847020/138204733-2044566e-79f0-4d00-880b-ac51df67ddd8.png)










