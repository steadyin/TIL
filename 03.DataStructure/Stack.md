## Stack

* 주요 추상 데이터 타입이며 일반적으로 배열이나 Linked List로 구현됩니다.
* LIFO(Last In First Out), 후입선출 구조 입니다.
* 기본연산은 pop(), push(), peek()가 있습니다.
* 대부분의 프로그래밍 언어는 Stack 지향입니다.
* 대부분의 기본 연산(예,두수의덧셈)은 Stack에서 인수를 가져와서 계산 후 Stack에 다시 놓는 것으로 정의한다.

## Stack 연산
* Stack은 삽입한 마지막 항목에만 액세스 할 수 있습니다.
* push()는 항목 삽입을 의미합니다.
* pop()은 마지막 항목을 반환 후 제거합니다.

## Stack in Application
* Stack 지향 프로그래밍에서 지역 변수, 참조 변수, 함수 호출 등을 저장하기 위해 사용됩니다.
* 그래프 알고리즘은 Stack에 의존합니다. 
  - Depth-First Search은 Stack으로 구현됩니다.
  - Finding Eulerian Cycles(오일러 사이클) in G(V,E) graph
  - Finding Strongly connected components in a given G(V,E) graph
* Stack은 추상 자료 구조로서 1차원 배열, Linked List로 구현 됩니다.
  - back button in web browser
  - undo operation in sofrwares
  - stack memory stores local variables and function calls

## Stack in Memory Management
* 2개의 타입의 메모리가 존재합니다. Stack, Heap 메모리
  * Stack
    * RAM(Random Access Memory)에서 Stack 메모리는 특수한 지역입니다.
    * Stack에은 호출된 함수, 지역 변수가 저장됩니다.
    * 자바가 메소드의 실행을 마친 후 반환될 위치를 아는 수단입니다.
    * Stack은 기본 데이터 타입, 자료 구조로 구현됩니다.

  * Hep
    * RAM(Random Access Memory)에서 Heap 메모리는 특수한 지역입니다.
    * Heap 메모리의 크기는 Stack 메모리보다 훨씬 큽니다. 더 많은 데이터를 저장됩니다.
    * 객체는 Heap메모리에 저장됩니다.
    * 단편화 될 수 있습니다. -> 크기가 다른 여러가지 객체를 저장할 수 있다. -> 비어있고 사용되지 않는 영역이 존재할 수 있다.
    * Heap 메모리와 Heap 데이터 구조와는 관련이 없다. 단지 메모리의 타입을 의미합니다.
    

      |Stack MEMORY|HEAP MEMORY|
      |---|---|
      |small size|larget size|
      |fast access|slow access|
      |no fragmentation(조각화)|may become fragmented|
      |store function calls|store object|
      |store local variables||


## Stack 메모리 공간 시각화

1. in the Stack a frame will be created from the main method

    <img src = "https://user-images.githubusercontent.com/79847020/135025412-80d10230-bff2-44a7-b7ed-fa8d957f616a.png" width="340" height="260"/>

    Stack 프레임 : Stack에서 하나의 작업단위를 말합니다.

    Stack 메모리는 RAM의 특정 지역 입니다. Stack 메모리는 함수 호출과 지역 변수를 저장합니다. <br>
    Heap 메모리는 RAM의 특정 지역 입니다. Heap 메모리는 용량이 더 크고 클래스의 인스턴스, 객체를 저장합니다. 

2. a new frame will be created for func2()

    <img src = "https://user-images.githubusercontent.com/79847020/135026185-1691e6da-689b-4640-90e5-d5c09d0cfbf9.png" width="340" height="260"/>

3. a new frame will be created for func3()

    <img src = "https://user-images.githubusercontent.com/79847020/135028583-51111a89-5076-4f06-8646-652b79321c71.png" width="340" height="260"/>

    클래스가 인스턴스화 된 객체, new House()는 Heap 메모리에 생성됩니다. <br>
    생성된 객체를 가리키는 참조 변수, houseRef 는 Stack 메모리에 저장됩니다.

4. func3() execution is completed

    <img src = "https://user-images.githubusercontent.com/79847020/135030121-4678df16-0e16-43a3-abbd-f6d4afdd5fba.png" width="340" height="260"/>

    func3() 실행이 완료되면 func3()의 Stack프레임이 flushed 됩니다. 그리고 Stack 메모리에서 해당 Stack프레임이 제거됩니다.

    func3()에 존재하던 참조변수가 flushed 됬기 때문에 더 이상 Heap메모리의 House객체에 대한 유효한 참조가 존재하지 않게됩니다.

    그렇게 House객체는 가비지 콜렉션의 대상이 됩니다. 가비지 콜렉터는 Heap메모리는 모니터링 합니다. 그리고 주어진 객체에 대한 활성참조가 없으면 가비지 콜렉터는 해당 객체를 제거합니다. 

4. func2(),func1() execution is completed

    <img src = "https://user-images.githubusercontent.com/79847020/135030885-9ecbdbef-65d1-40ae-8a6a-a0285399364b.png" width="340" height="260"/>

    그 후에는 func2(), func1()이 차례로 flush되고 제거됩니다.

---

## Stack 구현

추상 데이터 유형 Stack의 이론적 배경

Linked List로 Stack 구현시 모든 연산의 시간복잡도는 O(1)를 가집니다.
헤드 노드만 조작하기 때문에 일정한 시간복잡도를 가질 수 있습니다.

Array는 정적인 데이터 구조 입니다. 따라서 배열은 선형 실행 시간 복잡도로 실행되는 크기 재조정 메소드를 구현해야합니다. <br>
이것이 배열로 구현된 Stack의 병목현상입니다. Linked List는 동적인 데이터 구조로서, 선형 시간 복잡도를 가집니다.

### Stack 구현 by Linked List 

Node.java
```JAVA
package datastructure.stack.stackbylist;

public class Node<T extends Comparable<T>> {
    private T data;
    
    //this is why this implementation has some  additional memory complexity
    private Node<T> nextNode;

    public Node(T data) {
        this.data = data;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public Node<T> getNextNode() {
        return nextNode;
    }

    public void setNextNode(Node<T> nextNode) {
        this.nextNode = nextNode;
    }

    @Override
    public String toString() {
        return data + "";
    }
}
```

Stack.java
```JAVA
package datastructure.stack.stackbylist;

public class Stack<T extends Comparable<T>> {
    // the LIFO last item we inserted is the first one we take out
    // when the pop() methos is called
    private Node<T> head;
    private int count;

    // O(1)
    public void push(T data) {
        count++;
        // this is when the linked list is empty
        if (head == null) {
            head = new Node<T>(data);
        } else {
            //the linked list already contains some items
            Node<T> oldHead = head;
            head = new Node<T>(data);
            head.setNextNode(oldHead);
        }
    }

    // removes the last item we have inserted O(1)
    public T pop() {

        if (isEmpty()) return null;

        T item = head.getData();
        head = head.getNextNode();
        count--;
        return item;
    }

    // O(1)
    public int size() {
        return count;
    }

    // O(1)
    public boolean isEmpty() {
        return count == 0;
    }
}
```

App.java
```JAVA
package datastructure.stack.stackbylist;

public class App {
    public static void main(String[] args) {
        Stack<String> names = new Stack<String>();
        names.push("Adam");
        names.push("Ana");
        names.push("Kevin");
        names.push("Michael");

        while(!names.isEmpty()) {
            System.out.print(names.pop() + " ");
        }
    }
}
```

---

### Stack 구현 by Array

Stack.java
```JAVA
package datastructure.stack.stackbyarray;

public class Stack<T> {
    private int count;
    private T[] stack;

    public Stack() {
        // Compile Error T[] a  = new T[1];
        // Java warns us that unchecked cast from object to a generic T
        // but we know for certain that it is going to work just fine.
        stack = (T[]) new Object[1];
    }

    // we just have to add the new item to the end of the array O(1)
    public void push(T newData) {
        // ARRAYS ARE NOT DYNAMIC DATA STRUCTURES !!!
        // we have to resize the underlying array if necessary
        // if there are too many items : we double the size of the array
        // if there are too few items : then we decrease (shrink) the array
        if(count == stack.length)
            resize(2*count);

        //we just have to insert into the array
        stack[count++] = newData;
    }

    //returns [removes] the last item we have inserted O(1)
    public T pop() {
        if(isEmpty()) return null;

        // arary index starts with 1
        T item = stack[--count];

        // obsolete references - avoid memory leaks
        stack[count] = null;

        // maybe we have to resize the array and decrease the size
        // the stack is 25% full
        if(count > 0 && count ==stack.length / 4)
            resize(stack.length / 2);

        return item;
    }

    // O(1)
    public boolean isEmpty() {
        return count == 0;
    }

    // size method in O(1)
    public int size() {
        return count;
    }

    //this is the bottleneck of the application O(N)
    private void resize(int capacity) {

        System.out.println("Need to resize the array");

        T[] stackCopy = (T[]) new Object[capacity];

        // copy the items one by one
        for(int i=0; i<count; i++)
            stackCopy[i] = stack[i];

        //update the reference
        stack = stackCopy;
    }
}
```

App.java
```JAVA
package datastructure.stack.stackbyarray;

public class App {
    public static void main(String[] args) {
        Stack<Integer> nums = new Stack<Integer>();
        nums.push(1);
        nums.push(2);
        nums.push(3);
        nums.push(4);
        nums.push(5);
        nums.push(6);

        while(!nums.isEmpty()) {
            System.out.println(nums.pop());
        }
    }
}
```

---

## Dijkstra's interpreter

인터프리터는 주어진 코드를 해석할 수 있는 능력이 있습니다. 예를 들어 Stack은 SAX or Stax 같은 용량이 큰 XML파일을 Parsing할 때 매우 유용합니다. <br>
the shunting-yard algorithm은 수학적 표현을 Stack을 써서 Parsing 하는 알고리즘 입니다. Dijkstra에 의해서 개발된 알고리즘 입니다. <br>
다중 Stack을 써서 인터프리터를 구성할 수 있다는 것이 중요합니다. 

예를 들어 Operation Stack이 있고 Values Stack이 있습니다. 괄호가 있는 수학적 표현이 있습니다. 

1. 알고리즘은 여는 괄호일 때 아무작업도 수행하지 않습니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135763431-dc2fba1b-e0dd-4d8e-a3b9-982d86846fd1.png" width="320" height="270"/>

2. 정수 값을 발견하면 Values Stack에 Push 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135763420-43e0293c-8cda-40b9-a4a7-fbb330e4e601.png" width="320" height="270"/>

3. 연산기호를 발견하면 Operation Stack에 Push 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135763468-32dfad83-7b3b-40e1-85f0-38651fa88809.png" width="320" height="270"/>

4. 닫는 괄호인 경우 Values Stack에서 두개의 값을 Pop하고 Opertaion Stack에서 1개의 연산자를 Pop 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135763489-f911f444-59ca-48d0-84f5-5b6df7bd2006.png" width="320" height="270"/>

5. 두 개의 Value와 한 개의 연산자를 계산 후 Values Stsack에 Push 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135763555-03e8d19c-6ef5-4e9e-b100-6ee24a55d5c7.png" width="320" height="270"/>

6. 이 과정을 반복하면 수학표현식을 계산할 수 있습니다. 

    Stack 추상 데이터 구조를 써서 수학적 표현식을 Parsing 하는 방법입니다. 이것인 인터프리터의 기본 구조 입니다. 
    Stack은 여러 응용프로그램에서 사용되고 운영체제에서도 중요한 역할을 담당합니다. 

### 소스코드

Algorithm.JAVA
```JAVA
package datastructure.stack.dijkstra;

import java.util.Stack;

public class Algorithm {

    private Stack<String> operationStack;

    private Stack<Double> valueStack;

    public Algorithm() {
        this.operationStack = new Stack<>();
        this.valueStack = new Stack<>();
    }

    public void interpretExpression(String expression) {
        String[] expressionArray = expression.split(" ");

        for (String s : expressionArray) {
            if (s.equals("(")) {
                // do nothing
            } else if (s.equals("+")) {
                this.operationStack.push(s);
            } else if (s.equals("*")) {
                this.operationStack.push(s);
            } else if (s.equals("-")) {
                this.operationStack.push(s);
            } else if (s.equals("/")) {
                this.operationStack.push(s);
            } else if (s.equals(")")) {
                String operation = operationStack.pop();

                if (operation.equals("+")) {
                    valueStack.push(valueStack.pop() + valueStack.pop());
                } else if (operation.equals("*")) {
                    valueStack.push(valueStack.pop() * valueStack.pop());
                } else if (operation.equals("-")) {
                    valueStack.push(-1 * valueStack.pop() + valueStack.pop());
                } else if (operation.equals("/")) {
                    valueStack.push((1 / valueStack.pop()) * valueStack.pop());
                }
            } else {
                //정수
                valueStack.push(Double.valueOf(s));
            }
        }
    }

    public void result() {
        System.out.println(this.valueStack.pop());
    }
}
```

App.java
```JAVA
package datastructure.stack.dijkstra;

public class App {
    public static void main(String[] args) {
        Algorithm algorithm = new Algorithm();

        algorithm.interpretExpression("( ( 1 + 2 ) * ( 2 + 1 ) )");

        algorithm.result();
    }
}
```

## Java in Stack

App.java
```JAVA
package datastructure.stack;

import java.util.Stack;

public class App {
    public static void main(String[] args) {
        Stack<String> stack = new Stack<>();

        // push() - O(1)
        stack.push("Adam");
        stack.push("Bill");
        stack.push("Jeff");
        stack.push("Elon");

        // peek() - O(1) and returns the last item we have inserted
        // WITHOUT REMOVING IT !!!
        //System.out.println(stack.peek());
        //System.out.println(stack.size());

        // pop() - O(1) that is going to remove the last item ( + returns the value)
        //System.out.println(stack.pop());
        //System.out.println(stack.size());

        while (!stack.isEmpty()) {
            System.out.println(stack.pop());
        }
        System.out.println(stack.size());
    }
}
```

## Quiz

### Stacks are data structures

    No : Stacks are abstract data types without the implementation
    
### Stacks have a FIFO structure
    
    False, LIFO Structure

### Method calls are stored on the heap memory

    False, on the Stack


## Max in a stack problem overview

Interview Question #1

The aim is to design an algorithm that can return the maximum item of a stack in O(1) running time complexity. We can use O(N) extra memory!

Hint: we can use another stack to track the max item, Running Time : O(1) Memory Needed : O(N)

문제는 Stack이 있을 때 가장 큰 항목을 삽입시 추적하는 것이다. (O(N) 시간복잡도, O(N) 공간 복잡도 내에서)

<img src = "https://user-images.githubusercontent.com/79847020/135778501-4855abb6-fc0f-44bf-ab8a-16e86189df20.png" width="320" height="270"/>

1. mainStack에 '10' 항목을 삽입합니다. 항목이 한 개 밖에 없으므로 maxStack에도 '10' 항목을 그대로 삽입합니다.

2. mainStack에 '8' 항목을 삽입합니다. maxStack의 마지막 값과 새로 입력된 '8' 항목을 비교 후 '10'을 maxStack에 그대로 삽입합니다.

3. mainStack에 '23' 항목을 삽입합니다. maxStack의 마지막 값과 새로 입력된 '23' 항목을 비교 후 '23'을 maxStack에 그대로 삽입합니다.

4. 값이 큰 최대항목을 얻기 위해서 maxStack에서 pop()을 합니다.

MaxItemStack.java
```JAVA
package datastructure.stack.maxItemStack;

import java.util.Stack;

public class MaxItemStack {
    //this is the original stack
    private Stack<Integer> mainStack;

    //this stack is just for tracking the maximum item
    private Stack<Integer> maxStack;

    public MaxItemStack() {
        this.mainStack = new Stack<>();
        this.maxStack = new Stack<>();
    }

    public void push(int item) {

        //push the new item onto the stack
        mainStack.push(item);

        //first item is the same in both stacks
        if (mainStack.size() == 1) {
            maxStack.push(item);
            return;
        }

        //if the item is the largest one so far: we insert it onto the stack
        if (item > maxStack.peek()) {
            maxStack.push(item);
        } else {
            //if note hte largest one then we duplicate the largest one on the maxStack
            maxStack.push(maxStack.peek());
        }
    }
}
```


## The aim is to design a queue abstract data type with the help of stacks

The problem is that we want to implement queue abstact data type with the enqueue() and dequeue() operations with stacks

### Stack With Queue Solution

we can use 2 stacks to solve this problem

one stacks : for enqueue() operation

one stacks : for dequeue() operation

1. '10', '18', '30' 을 EnqueueStack에 Push 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135779190-8dede9a8-118c-43b8-9e73-56f147fcc8f8.png" width="320" height="270"/>

2. dequeue()을 수행합니다. (dequeueStack 항목이 비어 있는 경우)
 
    2.1 첫번째 항목을 get 하기 위해 EnqueueStack에서 pop 한 후 DequeueStack에 Push 합니다.

    <img src = "https://user-images.githubusercontent.com/79847020/135779353-4287f6b5-2e0e-41a6-a013-e643d6f065cb.png" width="320" height="270"/>

    2.2 DequeueStack에서 Pop을 합니다. '10'이 제거됩니다.
    
    <img src = "https://user-images.githubusercontent.com/79847020/135779496-3b79258c-f7bf-4988-9d8d-2303fc20841c.png" width="320" height="270"/>

3. '35'을 enqueue() 합니다.
    
    <img src = "https://user-images.githubusercontent.com/79847020/135779605-7e1199cc-eedf-4df1-a3ec-d0c8fc0abe80.png" width="320" height="270"/>
    
4. dequeue()을 수행합니다. (dequeueStack 항목이 비어 있지 않은 경우)

    4.1 dequeueStack이 비어 있지 않으므로 dequeueStack에서 바로 pop 합니다. 
    
    <img src = "https://user-images.githubusercontent.com/79847020/135779715-8473dd1d-5a94-473a-afff-67a31648ed4f.png" width="300" height="270"/>
    
    <img src = "https://user-images.githubusercontent.com/79847020/135779751-846761a5-10f7-427e-813e-2a2a33589e88.png" width="300" height="270"/>
    
    <img src = "https://user-images.githubusercontent.com/79847020/135779850-6925be0b-c26b-4479-b353-3c79acee4ade.png" width="300" height="270"/>
    
    이 상태에서 dequeue()을 수행하면 2번 과정이 수행됩니다.
    
Queue.java
```JAVA
package datastructure.stack.queueWithStack;

import java.util.Stack;

public class Queue {

    //use one stack for enqueue operation
    private Stack<Integer> enqueueStack;

    //use another stack for the dequeue operation
    private Stack<Integer> dequeueStack;

    public Queue() {
        this.enqueueStack = new Stack<>();
        this.dequeueStack = new Stack<>();
    }

    //adding an item to the queue is O(1) operation
    public void enqueue(int item) {
        this.enqueueStack.push(item);
    }

    public int dequeue() {

        //no items in the stacks
        if (enqueueStack.isEmpty() && dequeueStack.isEmpty())
            throw new RuntimeException("Stacks are empty...");

        //if the dequeueStack is empty we have to add items O(N) time
        if (dequeueStack.isEmpty()) {

            while (!enqueueStack.isEmpty()) {
                dequeueStack.push(enqueueStack.pop());
            }
        }

        //otherwise we just have to pop off an item in O(1)
        return dequeueStack.pop();
    }
}
```

App.java
```JAVA
package datastructure.stack.queueWithStack;

public class App {
    public static void main(String[] args) {

        Queue queue = new Queue();

        queue.enqueue(10);
        queue.enqueue(5);
        queue.enqueue(20);

        System.out.println(queue.dequeue());

        queue.enqueue(100);

        System.out.println(queue.dequeue());
        System.out.println(queue.dequeue());
        System.out.println(queue.dequeue());
    }
}
```

### Stack With Queue Solution - recursion

Queue의 enqueue() 연산을 구현하기 위해 Stack의 마지막 포인터에서 첫번째 항목까지 pop() 연산을 계속 수행해서 첫번째 항목을 반환하고 나머지 값들을 다시 push()해주는 방법으로 구현할 것입니다.

1개의 스택 공간이 사용되고 재귀호출 알고리즘이 사용됩니다.

OneStackQueue.java
```JAVA
package datastructure.stack.queueWithStack2;

import java.util.Stack;

public class OneStackQueue {

    // user one stack for enqueue operation
    private Stack<Integer> stack;

    public OneStackQueue() {
        this.stack = new Stack<>();
    }

    // adding an item to the queue is O(1) operation
    public void enqueue(int item) {
        this.stack.push(item);
    }

    // NOTE: we use 2 stack again but instead of explicitiy define the second stack we
    // use the call-stack of program (stack memory of execution stack)
    // 두번째 스택을 명시적으로 정의하는 대신 애플리케이션에 대한 메소드 호출 스택을 사용합니다.
    public int dequeue() {
        // base case for the recursive method calls the first item
        // is what we are after (this is what we need for queue's dequeue operation)
        if (stack.size() == 1) {
            return stack.pop();
        }

        // we keep poping the items until we find the last one
        int item = stack.pop();

        // we call the method recursively
        int lastDequeueItem = dequeue();

        // after we find the item we dequeue we have to reinsert the items one by one
        enqueue(item);

        // this is the item we are looking for (this is what have been popped off in the
        // stack.size()==1 section
        return lastDequeueItem;
    }
}
```

    
