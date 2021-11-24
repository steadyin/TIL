## Queue ?

* 주요 추상 데이터 타입이며 일반적으로 배열이나 링크드 리스트로 구현됩니다.
* FIFO(First In First Out), 선입선출 구조 입니다.
* 기본연산은 enqueue(), dequeue(), peek() 가 있습니다.
* 스레드 관리(멀티스레드)와 운영체제에서 사용됩니다.

## Queue 연산

1. Queue를 생성합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135765442-791aaae2-fbda-4b2d-becf-0d06967fb2f7.png" width="320" height="130"/>

2. enqueue() 연산

    10을 enqueue() 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135765482-00161b1f-d79b-4511-a233-62d393f0f0a0.png" width="320" height="130"/>

    6, 18, 1, 56을 enqueue() 합니다. 

    그림에서 왼쪽으로 오른쪽 순으로 삽입합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135765514-91647ab8-4d31-453c-8221-32af86a5c496.png" width="320" height="130"/>

4. dequeue() 연산

    먼저 삽입한 항목 순으로 제거, 삭제 됩니다. (반환)

    <img src = "https://user-images.githubusercontent.com/79847020/135765620-b9ce7990-c40c-46cf-bbf9-0ce9a1254022.png" width="320" height="130"/> <br>

    <img src = "https://user-images.githubusercontent.com/79847020/135765629-650fba61-f429-4e5d-a157-9e65e27e3062.png" width="320" height="130"/> <br>

    <img src = "https://user-images.githubusercontent.com/79847020/135765641-592b2a8d-36ec-4115-8803-999139964f8a.png" width="320" height="130"/> <br>

    <img src = "https://user-images.githubusercontent.com/79847020/135765651-ba2ba57a-b405-44ec-96bb-cf25906a809e.png" width="320" height="130"/> <br>

    <img src = "https://user-images.githubusercontent.com/79847020/135765442-791aaae2-fbda-4b2d-becf-0d06967fb2f7.png" width="320" height="130"/> <br>

## Queue 구현

Queue는 LinkedList 기반으로 구현합니다. 시작과 끝 포인트로 효율적으로 구현할 수 있기 때문입니다.

```JAVA
package datastructure.queue;

public class Node<T extends Comparable> {
    private T data;
    private Node<T> nextNode;

    public Node(T data) {
        this.data = data;
    }

    public Node<T> getNextNode() {
        return nextNode;
    }

    public void setNextNode(Node<T> nextNode) {
        this.nextNode = nextNode;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    @Override
    public String toString() {
        return "" + data;
    }
}
```

```JAVA
package datastructure.queue;

public class Queue<T extends Comparable> {
    private int size = 0;

    private Node<T> firstNode;
    private Node<T> lastNode;

    public boolean isEmpty() {
        return this.firstNode == null;
    }

    public int size() {
        return size;
    }

    // O(1)
    public void enqueue(T data) {
        Node<T> newNode = new Node<>(data);

        size++;

        if (isEmpty()) {
            firstNode = newNode;
            lastNode = newNode;
        } else {
            lastNode.setNextNode(newNode);
            lastNode = newNode;
        }
    }

    // O(1)
    public T dequeue() {
        size--;

        T temp = firstNode.getData();
        firstNode = firstNode.getNextNode();

        if (isEmpty()) {
            this.lastNode = null;
        }

        return temp;
    }

    public T peek() {
        return firstNode.getData();
    }
}
```

```JAVA
package datastructure.queue;

public class App {
    public static void main(String[] args) {
        Queue<Integer> queue = new Queue<>();

        queue.enqueue(1);
        queue.enqueue(2);
        queue.enqueue(3);

        System.out.println(queue.peek());

        // remove() method is a dequeue(0 method in O(1)
        while (!queue.isEmpty())
            System.out.println(queue.dequeue());
    }
}
```


## Queue in Application

1. Graph 알고리즘은 Queue를 매우 의존합니다. Breadth-First Search는 Queue로 구현됩니다.

2. Queue는 주어진 리소스가 여러 소비자에게 공유 될 때 유용합니다.

3. 스레드는 Queue에 저장됩니다. 

4. 일반적으로 Stack은 프로그래밍에서 중요합니다. Stack 메모리는 Stack 추상 데이터 구조로 구현되었기 때문이다.  <br>
   CPU 스케줄링에서 Queue는 중요합니다. Queue는 운영체제에서 중요합니다. 예를 들어 스마트폰에서 여러 이벤트(터치, 버튼)를 추적해야 한다고 하면,
   이벤트들을 Queue 대기열에 저장하고 선착순으로 처리됩니다. 이것이 CPU 스케줄링에서 Queue가 중요한 이유입니다.
   
## Queue in JAVA

Queue인터페이스는 인스턴스화 할 수는 없지만 LinkedList는 Queue 인터페이스를 구현합니다.
Qeueue의 연산은 LinkedList에서 O(1) 상수 시간 복잡도를 가지는 연산으로 구현되기 때문에 매우 효율적입니다.

### JAVA Collection Queue

```JAVA
package datastructure.queue.injava;

import java.util.LinkedList;
import java.util.Queue;

public class App {
    public static void main(String[] args) {
        //자바 Linked List는 기본적으로 Double Linked List로 구현되있습니다.
        //Queue도 시작과 끝 2개의 포인터를 가지고 있는 데이터 구조 Double Linked List로 구현이 효율적입니다.

        // we process the items in a first come first served manner
        Queue<Integer> queue = new LinkedList<>();

        //add method inserts a new item into the queue in O(1)
        queue.add(1);
        queue.add(2);
        queue.add(3);

        // element() we can return without removing the first item
        System.out.println(queue.element());
        // remove() method is a dequeue(0 method in O(1)
        while(!queue.isEmpty())
            System.out.println(queue.remove());      
    }
}
```


### Queue 작업 대기열 
```JAVA
package datastructure.queue.injava;

public class Task {

    private int id;

    public Task(int id) {
        this.id = id;
    }

    public void execute() {
        System.out.println("Executing task with id " + id);
    }
}
```

```JAVA
public class App {
    public static void main(String[] args) {
        Queue<Task> work = new LinkedList<>();

        work.add(new Task(1));
        work.add(new Task(2));
        work.add(new Task(3));

        // remove() method is a dequeue(0 method in O(1)
        while(!work.isEmpty())  {
            Task task = work.remove();
            task.execute();
        }
    }
}
```


---

## Quiz

### Queues have a FIFO structure

    True
    
### Queues are data structures
    
    False, Queue are abstract data types without the implementation
