# Binary Search Trees(이진 탐색 트리)

Array는 마지막 항목을 O(1) 상수 시간 복잡도로 빠르게 조작할 수 있습니다. LinkedList는 첫번째 항목을 O(1) 상수 시간 복잡도로 빠르게 조작할 수 있습니다. 하지만 특정 항목 검색은 Array, LinkedList 모두 O(N) 선형 시간 복잡도를 가지게 됩니다. <br>

만약에 Array가 정렬되어 있다면 ?
    
O(logN) 로그 시간 복잡도로 특정 항목을 검색할 수 있습니다. Binary Search 알고리즘의 기본 아이디어 입니다. 중간값을 비교 후 왼쪽 작은 값 또는 오른쪽 큰 값들을 비교대상에서 제외할 수 있기 때문에 O(logN) 로그 시간 복잡도로 임의의 항목을 찾을 수 있습니다.
    
# 1. Tree ?

## 1.1 Tree 정의

* Tree는 G(Vertice,Edge) 무방향 그래프로, 임의의 두 정점이 정확히 하나의 경로로 연결되거나, 동등하게 연결된 비순환 무방향 그래프입니다.

* Tree는 루드(Root) Node를 시작으로 다른 Node에 액세스 할 수 있습니다. LinkedList와 유사한 점입니다.

* 루트는 자식(Child) Node를 가지며, 간선(Edge)으로 연결되어 있다.

* 자식 Node의 개수는 차수(Degree)라고 하며, 크기(Size)는 자신을 포함한 모든 자식 Node의 개수다.

* 높이(Height)는 현재 위치에서부터 리프(Leaf)까지의 거리, 깊이(Depth)는 루트에서부터 현재 Node까지의 거리다.

* Tree는 그 자식도 Tree인 Subtree 구성을 띤다.

* 레벨(Level)은 0에서부터 시작한다. 논문에 따라 1에서부터 시작하는 경우도 있으나 현재 대부분의 문서에서는 0에서부터 시작하는 것이 좀 더 일반적이다.

* Tree는 항상 단방향(Uni-Directional)이기 때문에, 간선의 화살표는 생략 가능하다. 

## 1.2 기본 용어

![image](https://user-images.githubusercontent.com/79847020/136685773-ebb2a3c6-df2f-4f8b-bcdb-71461941097a.png)

노드(Node): Tree를 구성하는 기본 원소

루트 노드(Root Node): Tree에서 부모가 없는 최상위 Node, Tree의 시작점

부모 노드(Parent Node): 루트 Node 방향으로 직접 연결된 Node

자식 노드(Child Node): 루트 Node 반대방향으로 직접 연결된 Node

형제 노드(Siblings Node): 같은 부모 Node를 갖는 Node들

리프 노드(Leaf Node): 자식이 없는 Node. 단말 Node라 부르기도 한다.

내부 노드(Internal Node): 부모가 있으며 하나 이상의 자식이 있는 Node

Node의 깊이(Depth): 루트에서 어떤 Node에 도달하기 위해 거쳐야 하는 간선의 수

Node의 레벨(Level): Tree의 특정 깊이를 가지는 Node의 집합

Node의 차수(Degree): 각 Node의 자식의 개수

Node의 크기(Size): 자신을 포함한 모든 자식(자손!) Node 개수

Tree의 차수(Degree Of Tree): Tree의 최대 차수 = max[deg1, deg2, ..., degn]

Tree의 너비(Width): 가장 많은 Node를 갖고 있는 레벨의 크기

Tree의 높이(Height): 가장 긴 루트 경로의 길이

경로(Path): 한 Node에서 다른 한 Node에 이르는 길 사이에 있는 Node들의 순서

길이(Length): 출발 Node에서 도착 Node까지 거치는 간선의 개수

## 1.3 Not Tree

![image](https://user-images.githubusercontent.com/79847020/136287832-921afa3b-0304-4cdb-a073-ab19dc438285.png)

그림은 Tree가 아닙니다. A에서 C로 가는 경로, C에서 D로 가는 경로가 있기 때문에 A에서 D로 가는 2가지 경로가 발생합니다. 앞서 말했듯이 Tree는 정점은 한가지 경로로 연결되어야 합니다. 다시 한번 정리하면 무방향 비순환 그래프라는 것이 Tree의 정의이기 때문에 그래프에서 사이클이 존재하면 Tree가 아닙니다.

비순환 그래프이기 때문에 부모 자식과 관계를 정의할 수 있습니다. 부모 Node A가 있고 세개의 자식 Node B, C, D가 있습니다. 그리고 B가 F의 부모 Node이고 F가 자식 Node 입니다. 

# 2. Binary Search Trees 특징

![image](https://user-images.githubusercontent.com/79847020/136715106-d388ce80-31f7-4e6d-9f80-a5465fcdf435.png)

* Binary Search Tree는 방향성을 가지고 있고 부모 Node의 간선은 자식 Node를 가리킵니다. 

* Binary Search Tree는 모든 Node가 최대 두 개의 자식을 가질 수 있다는 것이 중요합니다.

* Node를 액세스 하기 위해서는 루트 Node부터 시작해야 합니다. 

* 왼쪽 자식 Node는 항상 부모 Node보다 작은 값을 가지고 있습니다.

* 오른쪽 자식 Node는 항상 부모 Node보다 큰 값을 가지고 있습니다.

* 왼쪽 하위 Tree는 루트 Node 보다 작은 값으로 구성됩니다.

* 오른쪽 하위 Tree는 루트 Node 보다 큰 값으로 구성됩니다.

* Binary Search Tree는 기본적으로 정렬된 데이터 구조 입니다. 

* O(logN) 로그 시간 복잡도로 임의의 항목을 찾을 수 있습니다. 매 결정으로 Binary Search Tree의 절반을 제거할 수 있기 때문입니다.

* Tree의 높이는 루트 Node와 리프 Node 사이의 가장 긴 아래 경로에 까지의 간선의 수 입니다. Tree의 계층 레벨이라고 볼 수 있습니다.

* 완전 Binary Search Tree는 모든 Node가 왼쪽 Node와 오른쪽 Node를 포함하는 Binary Search Tree입니다.

* Binary Search Tree는 데이터 구조이므로 항목을 효율적으로 저장할 수 있습니다. 

* Binary Search Tree는 정렬된 상태를 유지해야 한다는 원칙을 가집니다. Binary Search Tree의 연산을 O(logN) 시간복잡도로 수행하기 위해

* 각 비교를 통해 작업은 데이터의 절반을 건너뛸 수 있으므로 Binary Search Tree의 연산은 항목 수의 로그에 비례하는 시간이 걸립니다.
 
* 정렬되지 않은 배열에서 key로 항목을 찾는데 필요한 시간은 선형 시간 O(N) 보다 훨씬 낫지만 O(1)을 사용하는 해시 테이블에서 해당 작업보다 느립니다.


# 3. 균형 / 불균형

![image](https://user-images.githubusercontent.com/79847020/136287721-59d5da71-d914-488d-b69b-78ffb763f331.png)

O(logN) 로그 실행 시간은 Tree 구조가 균형을 이룰 때 보장됩니다. 

완전 Binary Search Tree에서 시간 복잡도 O(logN)으로 임의의 항목을 찾는다는 의미는 계층에서 단계별 하나씩만 거쳐서 항목을 찾는다는 의미입니다.

따라서 H는 Tree의 높이, N은 데이터 갯수 일 때 Tree의 높이는 h<=logN 을 유지해야 O(logN) 시간복잡도를 유지할 수 있습니다.

Tree구조는 불균형해질 수 있습니다. Tree가 불균형하여 h<=logN 관계가 더 이상 유효하지 않은 경우 시간 복잡도는 O(N) 입니다


# 4. 연산

## 4.1 Insert

![insert](https://user-images.githubusercontent.com/79847020/136289238-6d8854eb-f011-4320-815e-5bb120337897.gif)

삽입하는 첫번째 항목은 Binary Search Tree의 루트 Node가 됩니다.

모든 액세스는 루트로 부터 시작됩니다. 그후에는 입력되는 값과 비교해서 삽입 될 곳을 정해야 합니다.

루트 Node와 비교 후 작으면 왼쪽 하위 Node에, 크면 오른쪽 하위 Node에 삽입한다.

모든 왼쪽 자식 Node는 부모 Node 보다 작고 모든 오른쪽 자식 Node는 부모 Node 보다 크면 유효한 Binary Search Tree 입니다.

## 4.2 Search

모든 액세스의 시작은 루트 Node로 부터 시작합니다. 

단계별 비교 마다 대상 항목들의 반을 배제시킬 수 있기 때문에 Binary Search Tree를 O(logN) 시간 복잡도를 가질 수 있습니다.

![search8](https://user-images.githubusercontent.com/79847020/136285487-96ff6960-197f-412d-a6ae-12c8f6075a69.gif)

Binary Search Tree에서 가장 큰 항목은 맨 오른쪽에 존재합니다.

![searchMax](https://user-images.githubusercontent.com/79847020/136286228-20ba4d93-3263-4f79-ad20-faf487499b56.gif)

Binary Search Tree에서 가장 작은 항목은 맨 왼쪽에 존재합니다.

![searchMin](https://user-images.githubusercontent.com/79847020/136286250-53de56d6-815b-4981-bb61-0fd2ec06e281.gif)

불균형 Tree에서는 비교를 해도 데이터의 절반을 배제하지 못하기 때문에 O(logN) 시간 복잡도를 가질 수 없습니다.

Binary Search Tree는 균형을 게속해서 유지해야 언급한 특성을 갖게 됩니다.

## 4.3 Remove

![image](https://user-images.githubusercontent.com/79847020/136290176-1402f904-e70d-413d-8201-b680702f8e08.png)

Node를 제거하는 케이스는 자식 Node 수를 기준으로 분류할 수 있습니다.

알아두어야 할 것은 모든 액세스는 루트 Node로부터 액세스 해야 합니다.

* Removing a Leaf Node

가장 단순한 케이스 입니다. Leaf Node 제거는 부모 Node의 참조만 Null처리 하면 됩니다. 참조가 제거 된 리프Node는 GC에 의해 소거됩니다.

* Removing a Node With A Single Child

그 다음 단순한 케이스 입니다. 하나의 자식 Node를 가지고 있는 Node를 제거할 때는 부모 Node의 참조를 손자 Node로 수정하면 됩니다. 

* Removing a Node With Two Children

가장 복잡한 케이스 입니다. 루트 Node를 제거하는 경우를 생각해봅시다. 

루트 Node의 후임자 또는 전임자를 찾아야 합니다. 

왼쪽 하위 Tree에서 가장 큰 값을 전임자라고 합니다.

오른쪽 하위 Tree에서 가장 작은 값을 후임자라 합니다.

루트가 제거 되엇을 때 정렬된 값 기준으로 루트 Node에 고려될 수 있는 것들이 전임자, 후임자이기 때문입니다.

전임자는 왼쪽 하위 Tree에서 가장 큰 값이기 때문에 리프 Node입니다.

우리는 리프 Node를 제거하는 방법을 이미 알고 있습니다. (첫번째 케이스)

복잡한 문제가 있을 떄 이미 해결한 문제로 문제를 해결하려는 것을 수학적 환원이라고 합니다. 

전임자 Node를 제거 하고 루트 Node에 전임자 Node 값을 이동시켜주므로 해당 값을 제거할 수 있습니다.

제거 후에도 우리는 Tree가 유효한 바이너리 Tree임을 알 수 있습니다.

---

# 5. Binary Search Tree 순회

Binary Search Tree의 순회는 모든 단일 Node를 정확히 한번 방문하는 것을 의미합니다. 이는 O(N) 선형 시간 복잡도 임을 의미합니다.

순회는 세가지 유형이 있습니다. 

## 5.1 pre-order traversal

Root Node - Left Subtree - Right Subtree 순으로 재귀적으로 순회합니다.

![traverse_preorder](https://user-images.githubusercontent.com/79847020/136496287-9b989e7e-7e60-4734-b109-829c3f3aaada.gif)

## 5.2 post-order traversal

Left Subtree - Right Subtree - Root Node 순으로 재귀적으로 순회합니다.

![traverse_postorder](https://user-images.githubusercontent.com/79847020/136496298-e130bae5-a435-4ae1-9e06-df5e98bff4e2.gif)

## 5.3 in-order traversal (SORTED ORDER)

항목들이 정렬된 순서대로 순회하게 됩니다. 

Left Subtree - Root Node - Right Subtree 순으로 재귀적으로 순회합니다.

![traverse_inorder](https://user-images.githubusercontent.com/79847020/136496310-064d5724-7aed-47fa-b1a9-23016d0d993f.gif)

# 6. 시간복잡도

||Average-Case|Worst-case|
|---|---|---|
|space complexity|O(N)|O(N)|
|insertion|O(logN)|O(N)|
|deletion(removal)|O(logN)|O(N)|
|search|O(logN)|O(N)|

Binary Search Tree의 공간 복잡도는 모든 항목을 저장하기 때문에 O(N) 입니다. 

기본적으로 삽입, 삭제, 검색 연산에서 단계별로 모든 데이터에서 절반을 버릴 수 있으므로 단일 반복의 경우 로그 시간 복잡도 O(logN)을 가지게 되고 이것이 장점입니다.

그리고 우리가 Binary Search Tree를 좋아하는 두 번째 이유는 이러한 데이터 구조가 예측이 가능하기 때문입니다.

연산에 상관없이 같은 시간복잡도를 가집니다. 다른 자료 구조형은 연산이 같은 시간복잡도를 가지기 않기 때문에 예측하기 어렵습니다.

물론 최악의 경우 긴 Tree가 생성되어 O(N) 선형 시간 복잡도를 가지게 됩니다. 

* 왜 선형 시간 복잡도를 가지게 될까요 ?

    ![image](https://user-images.githubusercontent.com/79847020/136298991-ae271970-2e2a-4932-9f4e-630d98cd8851.png)

    다음과 같은 Tree 구조에서는 데이터의 절반을 배제하지 못하기 때문입니다.

Binary Search Tree는 매우 빠르기 빠르고 모든 작업의 시간 복잡도가 동일하기 때문에(연산시간 예측가능하기 때문에) 좋은 데이터 구조입니다.

하지만 Binary Search Tree는 불균형이 발생할 수 있고, 최악의 경우 이 경우 로그 실행 시간 복잡성이 선형 실행 시간 복잡도로 수행될 수 있습니다. 

해결책은 Binary Search Tree가 불균형해지지 않도록 하는 것입니다. 

따라서 로테이션 작업이 필요하고 이것이 AVR Tree와 Red-Black Tree가 고려되는 이유입니다.

# 7. Binary Search Tree With Recursive

## GetMax

![getmax_with_stack](https://user-images.githubusercontent.com/79847020/136495476-6d71d381-e06c-4c18-9f41-aafd059989e2.gif)

## Traverse

![traverse_with_stack](https://user-images.githubusercontent.com/79847020/136494988-c81081cb-0820-4925-911f-e918bcf3426b.gif)

# 8. Tree Applications 

1. Tree는 계층적 데이터를 표현하려는 경우 매우 강력합니다.

![image](https://user-images.githubusercontent.com/79847020/136683187-1c78881e-3d49-4d78-ac25-e33c29d77ce2.png)

- 운영체제, 파일시스템 <br> 
- 게임 Tree(체스와 틱택토) <br>
- 머신 러닝 : 의사 결정 Tree를 사용하고 부스팅은 매우 간단한 Tree 구조를 사용합니다. <br>

# 9. Quiz

* Why did binary search trees come to be?

      The basic idea is that maybe we are able to reduce O(N) operations to O(logN)
      
* How many children can a node have at most?

      2
      
* What does in-order traversal mean?

      Visit left subtree + visit root node + visit rightsubtree
      
* Is it possible for a binary search tree to become unbalanced?

      Yes
      
      : Thats why balanced trees came to be such as red-black trees or AVL trees
      
* What is the problem with unbalanced binary trees?

      The favourable O(logN) running time may even be reduced to O(N) running time.

---

# 10. 구현


## Interface Tree
```JAVA
package datastructure.binarysearchtree;

public interface Tree<T> {
    public void insert(T data);
    public void remove(T data);

    // in-order traversal yields tha natural ordering
    public void traversal();

    public T getMin();
    public T getMax();
}
```

## Class Person
```JAVA
package datastructure.binarysearchtree;

public class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(int age, String name) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String data) {
        this.name = data;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public int compareTo(Person target) {
        return Integer.compare(age, target.getAge());
    }

    @Override
    public String toString() {
        return name + "(" + age + ")";
    }
}
```


## Class Node
```JAVA
package datastructure.binarysearchtree;

public class Node<T> {

    private T data;

    private Node<T> leftChild;

    private Node<T> rightChild;

    //when implementing the remove method - 제거 시 부모노드읯 참조를 수정해야 하므로
    private Node<T> parentNode;

    public Node(T data, Node<T> parentNode) {
        this.data = data;
        this.parentNode = parentNode;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public Node<T> getLeftChild() {
        return leftChild;
    }

    public void setLeftChild(Node<T> leftChild) {
        this.leftChild = leftChild;
    }

    public Node<T> getRightChild() {
        return rightChild;
    }

    public void setRightChild(Node<T> rightChild) {
        this.rightChild = rightChild;
    }

    public Node<T> getParentNode() {
        return parentNode;
    }

    public void setParentNode(Node<T> parentNode) {
        this.parentNode = parentNode;
    }

    @Override
    public String toString() {
        return "" + data;
    }
}
```

## BinarySearchTree
```JAVA
package datastructure.binarysearchtree;

public class BinarySearchTree<T extends Comparable<T>> implements Tree<T> {

    private Node<T> root;

    @Override
    public void insert(T data) {
        // this is when we insert the first node into the BST (parent is null)
        if (root == null) {
            root = new Node<T>(data, null);
        } else {
            // there are already items in the BST
            insert(data, root);
        }
    }

    private void insert(T data, Node<T> node) {

        // this is the case when the data is SMALLER then the value in the node
        // GO TO THE LEFt SUBTREE
        // node -> inserted node, data -> newly insert node
        if (node.getData().compareTo(data) > 0) {
            // there is a valid (not NULL) left child so we go there
            if (node.getLeftChild() != null)
                insert(data, node.getLeftChild());
                // the left child is a NULL so we create a left child
            else
                node.setLeftChild(new Node<>(data, node));
        }
        // this is the case when the data is GREATER then the value in the node
        // GO TO THE RIGHT SUBTREE
        else {
            // there is a valid (not NULL) right child so we go there
            if (node.getRightChild() != null)
                insert(data, node.getRightChild());
                // the right child is a NULL so we create a right child
            else
                node.setRightChild(new Node<>(data, node));
        }
    }

    @Override
    public void remove(T data) {
        if (root == null) return;

        remove(data, root);
    }

    private void remove(T data, Node<T> node) {

        if (node == null) return;

        // first we have to search for the item we want to remove
        if (data.compareTo(node.getData()) < 0) {
            remove(data, node.getLeftChild());
        } else if (data.compareTo(node.getData()) > 0) {
            remove(data, node.getRightChild());
        } else {
            // WE HAVE FOUND THE ITEM WE WANT TO REMOVE !!
            // if the node is a leaf node(without left and right children)
            // THIS IS THE CASE 1)
            if (node.getLeftChild() == null && node.getRightChild() == null) {
                System.out.println("Removing a leaf node...");

                // wheather the node is a left child or right child
                Node<T> parent = node.getParentNode();

                //the noede is a left child
                if (parent != null && parent.getLeftChild() == node) {
                    parent.setLeftChild(null);
                    // the node is a right child
                } else if (parent != null && parent.getRightChild() == node) {
                    parent.setRightChild(null);
                }
                // maybe the root node is the one we want to remove
                if (parent == null) {
                    root = null;
                }

                // remove the node and makes it eligible for GC
                node = null;
            }

            // case 2) when we remove items with a single child
            // a single right child
            else if(node.getLeftChild()==null && node.getRightChild()!=null) {
                System.out.println("Removing a node with a single right child...");
                Node<T> parent = node.getParentNode();
                if(parent!=null) {

                    //the noede is a left child
                    if (parent != null && parent.getLeftChild() == node) {
                        parent.setLeftChild(node.getRightChild());
                    // the node is a right child
                    } else if (parent != null && parent.getRightChild() == node) {
                        parent.setRightChild(node.getRightChild());
                    }
                    // when we deal with the root node
                    if(parent==null) {
                        root = node.getRightChild();
                    }

                    // have to update the right child's parent
                    node.getRightChild().setParentNode(parent);
                    node =null;
                }
            }
            // is is approximatly the same CASE 2) BUT we have to deal with left child
            // a single left child
            else if(node.getLeftChild() != null && node.getRightChild()==null) {
                System.out.println("Removing a node with a single left child...");
                Node<T> parent = node.getParentNode();
                if(parent!=null) {

                    //the noede is a left child
                    if (parent != null && parent.getLeftChild() == node) {
                        parent.setLeftChild(node.getLeftChild());
                    // the node is a right child
                    } else if (parent != null && parent.getRightChild() == node) {
                        parent.setRightChild(node.getLeftChild());
                    }
                    // when we deal with the root node
                    if(parent==null) {
                        root = node.getLeftChild();
                    }

                    // have to update the right child's parent
                    node.getLeftChild().setParentNode(parent);
                    node =null;
                }
            }

            // remove 2 children
            else {
                System.out.println("Removing a node with 2 children...");

                // find the predecessor (max item in the left subtree)
                Node<T> predecessor = getPredecessor(node.getLeftChild());

                // swap just the value !!!
                T temp = predecessor.getData();
                predecessor.setData(node.getData());
                node.setData(temp);

                //we have to call the delete method recursively on the predecessor
                remove(data, predecessor);
            }
        }
    }

    private Node<T> getPredecessor(Node<T> node) {
        if(node.getRightChild() != null) return getPredecessor(node.getRightChild());
        return node;
    }

    @Override
    public void traversal() {
        //in-order traveral is O(N) linear running time
        if (root == null) return;

        traversal(root);
    }

    //O(N)
    private void traversal(Node<T> node) {
        if (node.getLeftChild() != null)
            traversal(node.getLeftChild());

        System.out.print(node + " - ");

        if (node.getRightChild() != null)
            traversal(node.getRightChild());
    }

    @Override
    public T getMin() {
        // the min item is the left item in the tree
        if (root == null) {
            System.out.println("BinarySearchTree is null");
            return null;
        }
        return getMin(root);
    }

    public T getMin(Node<T> node) {
        if (node.getLeftChild() != null) return getMax(node.getLeftChild());
        return node.getData();
    }

    @Override
    public T getMax() {
        // the max item is the rightmost item in the tree
        if (root == null) {
            System.out.println("BinarySearchTree is null");
            return null;
        }
        return getMax(root);
    }

    public T getMax(Node<T> node) {
        if (node.getRightChild() != null) return getMax(node.getRightChild());
        return node.getData();
    }
}
```

## Client App
```JAVA
package datastructure.binarysearchtree;

public class App {
    public static void main(String[] args) {
        BinarySearchTree<Integer> bst1 = new BinarySearchTree<>();

        bst1.insert(12);
        bst1.insert(4);
        bst1.insert(20);
        bst1.insert(1);
        bst1.insert(8);
        bst1.insert(16);
        bst1.insert(27);

        bst1.remove(16);
        bst1.remove(20);
        bst1.remove(12);
        bst1.traversal();

        BinarySearchTree<Person> bst2 = new BinarySearchTree<>();
        bst2.insert(new Person(12, "Adam"));
        bst2.insert(new Person(5, "Kevin"));
        bst2.insert(new Person(78, "Ana"));
        bst2.insert(new Person(56, "Michael"));
        bst2.insert(new Person(34, "Daniel"));
        bst2.insert(new Person(41, "Bill"));

        bst2.traversal();
    }
}
```

# 11. Question

## Compare binary trees overview

Interview Question #1:

두 개의 Binary Search Tree를 비교할 수 있는 효율적인 알고리즘을 작성하십시오. 

Tree가 동일하다는 것은 Tree의 Node와 Tree의 Topology가 같다는 것을 의미합니다. 

For example : 두개의 트리는 같은 값을 가지고 있습니다. 그러나 Topology는 같지 않습니다.

 ![image](https://user-images.githubusercontent.com/79847020/136684128-fe77ad72-2e5d-4e3c-a556-30a8941d33ca.png)
 
 Tree.java getRoot() 추가
 ```JAVA
 package datastructure.binarysearchtree;

public interface Tree<T> {
    public Node<T> getRoot();
    // in-order traversal yields tha natural ordering
    public void traversal();
    public void insert(T data);
    public void remove(T data);
    public T getMin();
    public T getMax();
}
``` 
 
 BinarySearchTree.java getRoot() 추가
 ```JAVA
    @Override
    public Node<T> getRoot() {
        return root;
    }
```    

 TreeCompareHeloer.java 생성
 ```JAVA
 package datastructure.binarysearchtree;

public class TreeCompareHeloer<T extends Comparable<T>> {

    public boolean compareTrees(Node<T> node1, Node<T> node2) {

        //we have to check the base cases (it may be leaf node so we have to use ==)
        if((node1==null || node2==null) return node1 == node2;
        
        //if the values within the nodes are not the same we return false (trees are not the same)
        if (node1.getData().compareTo(node2.getData()) != 0) return false;

        //the left subtree values AND the right subtree values must match as well !!!
        return compareTrees(node1.getLeftChild(), node2.getLeftChild()) 
                                  && compareTrees(node1.getRightChild(), node2.getRightChild());
    }
}
```

```JAVA
        //we have to check the base cases (it may be leaf node so we have to use ==)
        if (node1 == null || node2 == null) return node1 == node2;
```        

첫번째 케이스, 한쪽 자식 노드라도 null이면 다른 트리의 자식 노드도 null이여야 한다. 그렇지 않으면 false다.

```JAVA
        //if the values within the nodes are not the same we return false (trees are not the same)
        if (node1.getData().compareTo(node2.getData()) != 0) return false;
```        

두번째 케이스, 두 노드다 null이 아닌 경우 값이 같은지 비교해야 합니다

```JAVA
        //the left subtree values AND the right subtree values must match as well !!!
        return compareTrees(node1.getLeftChild(), node2.getLeftChild()) 
                                  && compareTrees(node1.getRightChild(), node2.getRightChild());
```

마지막으로 왼쪽 하위 트리와 오른쪽 하위 트리에 메서드를 재귀적으로 호출해야 합니다.

App.java
```JAVA
package datastructure.binarysearchtree;

public class App {
    public static void main(String[] args) {
        BinarySearchTree<Integer> bst3 = new BinarySearchTree<>();

        bst3.insert(30);
        bst3.insert(20);
        bst3.insert(10);
        bst3.insert(40);
        bst3.insert(70);
        bst3.insert(25);
        bst3.insert(35);
        bst3.insert(15);
        bst3.insert(5);
        bst3.insert(45);

        BinarySearchTree<Integer> bst4 = new BinarySearchTree<>();

        bst4.insert(30);
        bst4.insert(20);
        bst4.insert(10);
        bst4.insert(40);
        bst4.insert(70);
        bst4.insert(25);
        bst4.insert(35);
        bst4.insert(15);
        bst4.insert(5);
        bst4.insert(45);
        bst4.insert(75);
        bst4.insert(90);

        TreeCompareHeloer<Integer> heloer = new TreeCompareHeloer<>();
        System.out.println(heloer.compareTrees(bst3.getRoot(), bst4.getRoot()));
    }
}
```

## k-th smallest element in a search tree overview

Interview Question #2:

이번에 원하는 것은 원하는 k번째로 가장 작거나, 가장 큰 항목을 Binary Search Tree에서 찾는 것입니다. (in-place)

추가 메모리를 사용하지 않아야 합니다. 

사실 순회를 하고 주어진 항목을 순서대로 저장 후 찾으면 됩니다. 

하지만 그 방법은 추가 메모리에 필요하며, 재귀호출을 통해서 좀 더 나은 방법을 고려해볼 수 있습니다.

이 알고리즘의 기본 아이디어는 주어진 노드 T의 왼쪽 하위 트리에 다음 항목이 포함된 다는 것입니다.

-> k가 왼쪽 서브트리의 노드 수보다 적으면, k번째 작은 값은 왼쪽 서브트리에 존재합니다.

-> k가 왼쪽 서브트리의 노드 수보다 크면, k번째 작은 값은 오른쪽 서브트리에 존재합니다.

![image](https://user-images.githubusercontent.com/79847020/138787773-23cd6559-9b2f-4b96-ac02-3c8c8dea33bf.png)

The Algorithm :

int n = number of nodes in the left subtree + 1

if(n==k) return node;

if(n > k) return kthSmallest(leftSubtree, k)

if(n < k) return kthSmallest(rightSubtree, k-n)

Tree.java
```JAVA
package datastructure.binarysearchtree;

public interface Tree<T> {
    public Node<T> getKSmallest(Node<T> node, int k);
    public Node<T> getRoot();
    // in-order traverse yields tha natural ordering
    public void traverse();
    public void insert(T data);
    public void remove(T data);
    public T getMin();
    public T getMax();
}
```

BinarySearchTree.java
```JAVA
    @Override
    public Node<T> getKSmallest(Node<T> node, int k) {
        //number of nodes in the left subtree
        //+1 because we count the root node of the subtree as well
        int n = treeSize(node.getLeftChild()) + 1;

        //this is when we find the kth smallest item
        if(n==k) {
            return node;
        }

        //if the number of nodes in the left subtree > k-th smallest item
        //it means the k-th smallest item is in the left subtree
        if(n>k) return getKSmallest(node.getLeftChild(), k);

        //if the number of nodes in the left subtree is smaller then the k-th
        //smallest item then we can discard the left subtree and consider the
        //right subtree
        //NOW WE ARE NOT LOOKING FOR THE K-TH BUT THE K-Nth SMALLEST ITEM
        if(n<k) return getKSmallest(node.getRightChild(), k-n);

        return null;
    }

    //calculate the size of a subtree with root node 'node'
    private int treeSize(Node<T> node) {
        //this is the base case
        if(node==null) return 0;

        //recursively sum up the size  of the left subtree + size of right subtree
        //size of tree = size left subtree + size of right subtree + 1 (because of the root)
        return (treeSize(node.getLeftChild()) + treeSize(node.getRightChild())+1);
    }
```    

## Family age problem overview

가계도의 나이 합계를 계산하기 위한 알고리즘 입니다. 

O(N) 선형 시간 복잡도로 트리를 순회하면서 합계를 구합니다.

Tree.java
```JAVA
package datastructure.binarysearchtree;

public interface Tree<T> {
    public Node<T> getKSmallest(Node<T> node, int k);
    public Node<T> getRoot();
    public int getAgesSum();
    public void traverse();
    public void insert(T data);
    public void remove(T data);
    public T getMin();
    public T getMax();
}

```

BinarySearchTree.java
```JAVA
public class BinarySearchTree<T extends Comparable<T>> implements Tree<T> {
..생략..
    @Override
    public int getAgesSum() {
        return getAges(this.root);
    }

    private int getAges(Node<T> node) {
        System.out.println("Considering node " + node);

        //we have to reinitialize the variables (sum is the parent's node value so the sum of the subtrees so far)
        int sum = 0;
        int leftSum = 0;
        int rightSum = 0;

        //null nodes have sum value 0
        if(node==null) { return 0;}

        //POST-ORDER TRAVERSAL
        //we do a simple post-order traversal because here we have to calculate both left and right value to
        //be able to calculate the parent's value (sum of childrens' args)
        //check the left subtree recursively
        leftSum = getAges(node.getLeftChild());
        //check the right subtree recursively
        rightSum = getAges(node.getRightChild());

        //update the sum ... given node's value is the own value = left subtree sum + right subtree sum
        System.out.println("Considering node " + node + " total ages so far is " + (((Person)node.getData()).getAge() + leftSum + rightSum));
        sum = ((Person)node.getData()).getAge() + leftSum + rightSum;

        return sum;
    }
}
```

Person.java
```JAVA
package datastructure.binarysearchtree;

public class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(int age, String name) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String data) {
        this.name = data;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public int compareTo(Person target) {
        return Integer.compare(age, target.getAge());
    }

    @Override
    public String toString() {
        return name + "(" + age + ")";
    }
}
```

App.java
```JAVA
package datastructure.binarysearchtree;

public class App {
    public static void main(String[] args) {
        BinarySearchTree<Person> bst6 = new BinarySearchTree<>();
        bst6.insert(new Person(47, "Adam"));
        bst6.insert(new Person(21, "Kevin"));
        bst6.insert(new Person(55, "Joe"));
        bst6.insert(new Person(20, "Arnold"));
        bst6.insert(new Person(34, "Noel"));
        bst6.insert(new Person(68, "Marko"));
        bst6.insert(new Person(23, "Susan"));
        bst6.insert(new Person(38, "Rose"));

        System.out.println(bst6.getAgesSum());
    }
}
```






