## Linked List ? 

추상적 자료형인 List를 구현한 동적 자료 구조로, Linked List는 데이터 덩어리(Node)를 저장할 때 그 다음 순서의 자료가 있는 위치를 데이터에 포함시키는 방식으로 자료를 저장한다. 

## Linked List의 기본 구조

Linked List의 단일 노드는 데이터 공간과 참조 공간으로 구성됩니다. 데이터 공간에는 말그대로 데이터가 저장됩니다. 정수, 부동소수, Boolean, 사용자 정의 객체 등이 될 수 있습니다. 
참조 공간에는 Linked List의 다른 항목을 참조합니다. 의미적으로는 가리킨다고 할 수 있습니다. 

이런 구조 때문에 Array 보다 더 많은 메모리를 필요로 합니다. 데이터 뿐만 아니라 다음 항목을 가리키는 포인터는 저장해야 하기 때문입니다. 
하지만 이 점 때문에 Array처럼 빈 공간이 발생하지 않는 다는 장점이 있습니다. 항목을 이동할 필요가 없다는 것입니다. 이것이 선형 시간복잡도를 상수 시간복잡도로 시간복잡도가 감소할 수 있는 이유입니다. 

## Linked List 특징

* Linked List는 이해하기 쉽고 간단하게 구현할 수 있습니다.

* Linked List로 Quque나 Stack처럼 다른 복잡한, 추상 데이터 구조를 구현할 수 있습니다.

* 연속적인 메모리 공간에 저장되지 않으며 Random Index 할 수 없습니다.

* Linked List는 데이터 구조 접근 시 첫번째 노드부터 시작해야 합니다.

* Linked List는 첫번째 노드를 조작하는데 유리합니다. (O(1) 상수 시간복잡도) 

* Linked List의 마지막 위치에 새 항목을 삽입, 삭제하는데 어려움이 있다는 것입니다. (O(N) 선형 시간복잡도)


## Linked List의 장점

* Linked List는 동적 자료 구조 입니다. 
    * Runtime에 메모리를 할당하여 새로운 노드를 추가할 수 있습니다.
    * CompileTime에 Linked List의 크기를 모른다는 것은 문제가 되지 않습니다.
    * 데이터 크기를 조정할 필요가 없습니다.

* Linked List 의 첫번째 항목을 조작하는 것은 매우 빠릅니다. (O(1) 상수 시간복잡도)

* 크기가 다른 항목들을 저장할 수 있습니다.

## Linked List의 단점

1. 데이터 영역 이외에 참조 영역 메모리 공간이 추가로 필요합니다.

2. Random Access가 불가능합니다. 
    - Linked List 첫번째 항목에만 직접 접근할 수 있습니다. 

3. Backward, 이전 노드 탐색이 불가능합니다. 
    - Double Linked List에서 개선할 수 있습니다.

4. 임의의 항목에 접근하는데 O(N) 선형 시간복잡도를 가집니다.
    - 따라서 Binaray Search가 고려됩니다.

5. 실행 시간을 예상할 수 없습니다.
    - Array도 같은 특성을 가지고 있습니다. 실행 시간은 사용자가 수행하는 작업에 의존합니다. <br> 
	첫번째 항목 조작은 빠르고, 임의의 항목 조작은 느립니다. <br>
    일정한 시간복잡도를 가져야 하는 경우, 사용자 조작에 따라 시간복잡도가 다르다면 좋은 옵션이 아닙니다.(예)페이스북 서비스)
    사용자가 수행하는 연산에 비슷한 실행 시간을 가지도록 하는 것은 매우 중요합니다. <br>
    이것이 Bianry Serarch(시간복잡도(logN))가 모든 작업에 고려되는 이유입니다. <br>
    예측성은 애플리케이션 설계에서 절대적으로 중요한 요소입니다.

## Linked List 연산

1. 첫번째 항목 조작(삽입, 삭제) 
    - O(1) 상수 시간복잡도, Linked List의 장점

2. 임의의 항목 조작 
    - O(N) 선형 시간복잡도, Linked List의 단점

## Linkst List 구현

```JAVA
package datastructure.linkedlist;

public interface List<T extends Comparable> {
    public void insert(T data);
    public void remove(T data);
    public void traverse();
    public int size();
}
```
```JAVA
package datastructure.linkedlist;

public class Node<T extends Comparable> {

    //this is the data we sotre in the data sturucture
    private T data;

    //this is why linked lists need more memory then arrays
    private Node<T> nextNode;

    public Node(T data) {this.data = data;}

    public T getData() {return data;}

    public void setData(T data) {this.data = data;}

    public Node<T> getNextNode() {return nextNode;}

    public void setNextNode(Node<T> nextNode) {this.nextNode = nextNode;}

    @Override
    public String toString() {return "" + data;}
}
```
```JAVA
package datastructure.linkedlist;

import java.util.Comparator;

public class Person implements Comparable<Person> {

    private int age;
    private String name;

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public int compareTo(Person o) {
        return Comparator.comparing(Person::getName).thenComparingInt(Person::getAge).compare(this, o);
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {return name;}
}
```
```JAVA 
    private void solution1() {
        LinkedList<String> names =new LinkedList<>();

        names.insert("Adam");
        names.insert("Daniel");
        names.insert("Ana");

        names.traverse();
        names.remove("Daniel");
        System.out.println();
        names.traverse();
    }

    private void solution2() {
        LinkedList<Person> names =new LinkedList<>();

        names.insert(new Person(23, "Adam"));
        names.insert(new Person(34, "Daniel"));
        names.insert(new Person(56, "Ana"));

        names.traverse();

        names.remove(new Person(34, "Daniel"));

        names.traverse();

    }
```
```CONSOLE
//solution1
Adam -> Daniel -> Ana

Adam -> Ana

//solution2
Adam -> Daniel -> Ana

Adam -> Ana
```    
--------------------------------------------------------------------------------

## Array와 Linked List 비교하기

1. 동적, 정적 데이터 구조
    - Array는 정적인 자료 구조 입니다. 
        - 일반적으로 데이터 구조의 크기를 미리 알아야합니다. 물론 저장해야 하는 항목의 수를 모르는 경우 지정된 기본 항목의 크기를 조정할 수 있습니다. 그래서 큰 문제는 아니지만, 일반적으로 Array은 정적 데이터 구조로 간주됩니다. 왜냐하면 우리가 저장해야 할 항목의 수가 변경된다면 데이터 구조 자체를 재설정해야 되기 때문입니다. 추후에 HashMap에 대해서 얘기할텐데 HashMap에서는 기본 1차원 Array의 근본적인 데이터 구조의 크기를 조정해야 합니다. 이것이 Array이 정적 데이터 구조로 간주되는 이유입니다.
    - Linked List는 동적이 데이터 구조 입니다.
        - 주어진 데이터 구조의 크기를 조정할 필요가 없습니다. 유동적으로 변경할 수 있습니다. 두 개의 참조를 가지고 있음으로 크기 조정 작업이 필요없습니다. 
    - 이런 점에 있어서는 Array보다 동적인 데이터 구조인 Linked List 가 장점을 가지고 있습니다. 크기 조정 작업이 필요하지 않습니다. 

2. Random Access
    - Random Access는 Array의 큰 장점입니다. 
        - 데이터는 연속적인 메모리 공간에 있습니다. O(1) 상수 시간복잡도로 데이터를 식별할 수 있는 이유입니다. 
    - Linked List는 Random Access가 불가능합니다. 
        - 연속적인 메모리 공간에 있지 않기 때문에 Random Access를 할 수 없습니다.
    - 따라서 Random Access에 대해서는 Array가 Linked List보다 유리합니다.

3. 첫번째 데이터 조작
    - Array은 최악의 시나리오에서 여러 항목을 이동해야 합니다. 이것이 Array 첫번째 데이터를 조작하면 선형 시간복잡도(O(N))을 가지는 이유입니다.
    - Linked List는 동적 데이터 구조이고 Head 노드 주변의 참조데이터만 수정하면 되므로 더 효율적으로 처리할 수 있습니다.
    - 따라서 Random Access에 대해서는 Linked List가 Array 보다 유리합니다.

4. 마지막 데이터 조작
    - 마지막 데이터를 조작할 때 Array는 빈 공간이 생기지 않습니다. 따라서 Array는 마지막 데이터를 조작할 때 빠르고 상수 시간복잡도 O(1)를 가집니다.
    - Linked List는 오직 Head node를 통해서만 접근할 수 있습니다. 따라서 마지막 데이터를 조작하기 위해서는 Linked List 전체를 순회해야 하기 때문에 선형 시간복잡도 O(N)를 가지게 됩니다.
    - 따라서 마지막 데이터를 조작할 때는 Array이 Linked List 보다 유리합니다.

5. 메모리 관리
    - Array는 추가 메모리 공간이 필요없습니다. 
    - Linked List는 다음 노드 포인터를 저장할 추가 공간이 필요합니다.


6. 결론

|---|Linked Lists|Arrays|
|---|---|---|
|search|O(N)|O(1)|
|insert at the start|O(1)|O(N)|
|insert at the end|O(N)|O(1)|
|waste space|O(N)|0|

Array과 Linked List 모두 큰 단점을 가지고 있습니다. 임의의 항목을 검색할 때 O(N) 선형 시간복잡도가 필요합니다.       


---

## Linked List의 실제 적용

일반적으로 운영체제는 Linked List에서 HashMap까지 거의 모든 중요한 데이터 구조를 사용합니다. Linked List의 실제 적용에 대해서 이야기 하겠습니다.

1. Low Level Memory Management
    - Linked List는 로우 레벨 메모리 관리 영역에서 매우 중요합니다. 예를 들면 C프로그래밍에서 malloc(), free()을 써서 힙메모리에 할당, 해제를 할 수 있습니다. 예를 들어 힙에 30바이트의 메모리를 할당하려면 이 코드를 사용해야 합니다.<br>
    
	allocates 30 bytes of memory in the heap <br>
    ```C++ 
     char* char_ptr = (char*) malloc(30);
    ```  
    
	기본적으로 Heap 메모리에 무언가를 저장하면 데이터 블록이 생깁니다. Body에는 우리가 저장하려는 데이터가 담기게됩니다. 그리고 이 블록들은 Double Linked List 구조로 연결되어 있습니다. 따라서 모든 단일 블록 헤더는 이전 블록과 다음 블록에 대한 참조가 포함됩니다. 
    
	이것과 관련된 https://wiki.syslinux.org/wiki/index.php?title=Heap_Management 의 Memory Allocation 항목에서 좀 더 살펴볼 수 있습니다.

2. Application in Windows
	- 윈도우 작업 탭, PhotoViewer의 사진목록, 블록체인은 LinkedList 구조로 구성되어있습니다. 

---

## QUIZ

* Why to use linked lists over arrays?

    Because it is faster to insert items at the beginning of the list as we do not have to rearrange the array, just update, the references accordingly

* Which data structure to choose if our aim is to minimize the amount of memory we use?

    Arrays

* Linked lists are dynamic data structures (it can grow organically without any problems).

    True

---

## Double Linked List ?
 
* 목표는 데이터의 삽입과 제거 작업을 효율적으로 하기 위한 데이터 구조입니다. 
* Head 노드와 Tail 노드의 참조를 저장하고 있습니다.
* 모든 단일노드에 이전 노드와 다음 노드를 가리키고 있는 두 개의 참조를 포함합니다.
* 시작 데이터와 마지막 데이터를 조작할 때 O(1) 상수 시간복잡도를 가집니다.
* 양방향 순회가 가능한 것이 LInked List와의 차이며 Double Linked List의 큰 장점입니다.
* 임의의 데이터를 찾는 것은 아직 O(N) 선형 시간복잡도가 가집니다. 이를 개선하기 위해 Binary Search Tree가 고려됩니다.

## Linked List in JAVA API 

https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html

Java에서 Linked List를 인스턴스하면 Double Linke List로 구성됩니다. 
List와 Deque 인터페이스를 구현합니다. 따라서 Linked List는 모든 리스트의 연산과 모든 요소(Null포함)을 수용합니다. 

Double Linked List의 메소드에는 addFirst와 addLast가 있습니다. 이 작업은 Head노드와 Tail노드의 정보를 알고 있기 때문에 상수복잡도를 가집니다.
Head 데이터와 Tail 데이터를 매우 빠르게 삽입하고, 제거할 수 있다는 점이 Double Linked List의 장점입니다. 
그래도 여전히 임의의 항목을 검색할 때는 선형 시간복잡도를 가집니다. 이를 개선하기 위해 이진 검색 트리, 해시 테이블이 고려됩니다.

```JAVA
List<STring> dll = new LinkedList<>();
```
       
---

    
## Quiz

* What is the difference between linked lists and doubly linked lists?    
    
    Doubly Linked Lists store an extra reference to the previous node in teh list as well
    
* We can Manipulate the first as well as the last item in O(1) with doubly lijnked lists
    
    true
    
---

## Double Linked List 구현 
    
```JAVA
package datastructure.doublelinkedlist;

public class Node<T extends Comparable> {

    private T data;

    //this is even more memory heavy than single linked lists
    private Node<T> previousNode;
    private Node<T> nextNode;

    public Node(T data) {this.data = data;}

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public Node<T> getPreviousNode() {
        return previousNode;
    }

    public void setPreviousNode(Node<T> previousNode) {
        this.previousNode = previousNode;
    }

    public Node<T> getNextNode() {
        return nextNode;
    }

    public void setNextNode(Node<T> nextNode) {
        this.nextNode = nextNode;
    }

    @Override
    public String toString() {return "" + data ;}
}
```
```JAVA
package datastructure.doublelinkedlist;

import java.util.Comparator;

public class Person implements Comparable<Person> {

    private int age;
    private String name;

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public int compareTo(Person o) {
        return Comparator.comparing(Person::getName).thenComparingInt(Person::getAge).compare(this, o);
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {return name;}
}
```
```JAVA
package datastructure.doublelinkedlist;

public class DoubleLinkedList<T extends Comparable<T>> {
    //this is the head node or root node
    private Node<T> head;
    private Node<T> tail;

    private int numberOfItems;

    public void insert(T data) {

        Node<T> newNode = new Node<>(data);
        
        // this is the first item in linked list
        if(tail==null) {
            //both of the pointers are pointing to the new node
            tail = newNode;
            head = newNode;
        } else {
            //we have to insert the new item to usually the end of the list
            //we just have to manipulate the tail node and its references in O(1)
            newNode.setPreviousNode(tail);
            tail.setNextNode(newNode);
            tail = newNode;
        }
        numberOfItems++;
    }

    // let's traverse the list forward
    public void traverseForward() {
        Node<T> actualNode = head;

        while(actualNode != null) {
            System.out.println(actualNode);
            actualNode = actualNode.getNextNode();
        }
    }

    // let's traverse the list backward
    public void traverseBackward() {
        Node<T> actualNode = tail;

        while(actualNode != null) {
            System.out.println(actualNode);
            actualNode = actualNode.getPreviousNode();
        }
    }

    public int size() {
        return numberOfItems;
    }
}
```
```JAVA
    private static void solution1() {
        LinkedList<String> dll1 = new LinkedList<>();
        dll1.addFirst("Kevin");
        dll1.addFirst("Daniels");
        dll1.addFirst("Ana");

        for (String name : dll1)
            System.out.print(name + " ");

        LinkedList<String> dll2 = new LinkedList<>();

        dll2.addLast("Kevin");
        dll2.addLast("Daniels");
        dll2.addLast("Ana");

        System.out.println();

        for (String name : dll2)
            System.out.print(name + " ");

        DoubleLinkedList<String> names = new DoubleLinkedList<>();

        names.insert("Adam");
        names.insert("Kevin");
        names.insert("Ana");
        names.insert("Daniel");

        System.out.println();
        names.traverseForward();
        System.out.println();
        names.traverseBackward();
    }
```
```CONSOLE
Ana Daniels Kevin 
Kevin Daniels Ana 
Adam Kevin Ana Daniel 
Daniel Ana Kevin Adam
```
```JAVA
    private static void solution2() {
        ArrayList<Integer> array = new ArrayList<>();

        long now = System.currentTimeMillis();

        for (int i = 0; i < 500000; ++i)
            array.add(0, i);

        System.out.println("Time taken for Insert First Data ArrayList : " + (System.currentTimeMillis() - now));

        LinkedList<Integer> list = new LinkedList<>();

        now = System.currentTimeMillis();

        for (int i = 0; i < 500000; ++i)
            list.addFirst(i);

        System.out.println("Time taken for Insert First Data to LinkedList: " + (System.currentTimeMillis() - now));

        array = new ArrayList<>();

        now = System.currentTimeMillis();

        for (int i = 0; i < 500000; ++i)
            array.add(i);

        System.out.println("Time taken for Insert Last Data ArrayList : " + (System.currentTimeMillis() - now));

        list = new LinkedList<>();

        now = System.currentTimeMillis();

        for (int i = 0; i < 500000; ++i)
            list.addLast(i);

        System.out.println("Time taken for Insert Last Data to LinkedList: " + (System.currentTimeMillis() - now));
    }
```
```CONSOLE
Time taken for Insert First Data ArrayList : 8998
Time taken for Insert First Data to LinkedList: 77
Time taken for Insert Last Data ArrayList : 11
Time taken for Insert Last Data to LinkedList: 31
```
---
    
## Finding the middle node in a linked list overview

Suppose we have a standard linked list. Construct an in-place (without extra memory) algorithm thats able to find the middle node !

추가 메모리 없이 중간 노드 찾기

1) naive solution
    - Linked List를 탐색하면서 노드의 수를 알아냅니다.
    - 노드의수/2 까지 탐색하면서 중간 노드 값을 알아냅니다.
    
2) using tow pointers
    - First Pointer : 한번에 한개의 노드를 탐색합니다.
    - Second Pointer : 한번에 두개의 노드를 탐색합니다.
    - Second Pointer가 끝에 도달하면 First Pointer는 Middle Node를 가리키고 있습니다.
    
추가 작업 없이 목록의 중간 노드를 선형 실행 시간으로 O(N)으로 찾을 수 있습니다. 
두 개의 Pointer를 사용하지만 in-memory 구현입니다.
    
```JAVA
    @Override
    public Node<T> getMiddleNode() {
        Node<T> fastPointer = this.root;
        Node<T> slowPointer = this.root;

        while(fastPointer.getNextNode()!=null && fastPointer.getNextNode().getNextNode()!=null) {
            fastPointer = fastPointer.getNextNode().getNextNode();
            slowPointer = slowPointer.getNextNode();
        }
        return slowPointer;
    }
```
```JAVA
    private static void solution3() {
        List<Integer> myLinkedList = new LinkedList<>();

        myLinkedList.insert(10);
        myLinkedList.insert(2);
        myLinkedList.insert(13);
        myLinkedList.insert(5);
        myLinkedList.insert(6);
        myLinkedList.insert(7);
        myLinkedList.insert(8);
        myLinkedList.insert(9);
        myLinkedList.insert(1);

        System.out.println(myLinkedList.getMiddleNode());
    }
```	
```CONSOLE
6
```
---
    
## Reverse a linked list in-place overview    

Construct an in-place algorithm (without the need for extra memory) to reverse a linked list!

For example: 1 -> 2 -> 3 -> 4 should be transformed into 4 -> 3 -> 2 -> 1

1) naive solution : 새로운 Linked List를 생성하여 하나씩 옮긴다. O(N) 공간 복잡도, in-memory 알고리즘이 아닙니다.
    
2) using pointers : in-memory 알고리즘, O(N) 시간복잡도
    
5 -> 13 -> 2 -> 10 -> NULL
    
Null <- 5 <- 13 <- 2 <- 10
    
로 Linked List를 뒤집습니다. 
	
```JAVA
    @Override
    public void reverse() {
        Node<T> currentNode = this.root;
        Node<T> previousNode = null;
        Node<T> nextNode = null;

        while(currentNode!=null) {
            nextNode = currentNode.getNextNode();
            currentNode.setNextNode(previousNode);

            previousNode = currentNode;
            currentNode = nextNode;
        }

        this.root = previousNode;
    }
```
```CONSOLE
10 2 13 5 
5 13 2 10
```

	
