# AVL Tree

Adelson-Velsky와 Landis(AVL)가 1962년에 발명한 균형 잡힌 데이터 구조입니다. 

이진 탐색 트리를 구성하면 O(logN) 로그 시간 복잡도로 임의의 항목을 검색할 수 있습니다. 하지만 최악의 경우 Array와 LinkedList와 마찬가지로 O(N) 선형 실행 시간 복잡도를 가지게 됩니다.
AVL Tree는 이런 이진 탐색 트리의 문제점을 개선하기 위해 균형을 유지하는 이진 탐색 트리 입니다. 

## 1. AVL Tree 특징

* AVL Tree는 균형 이진 탐색 트리라고 말할 수 있습니다. 

* 따라서 삽입과 기본 연산(삽입과 삭제)은 이진 탐색 트리와 동일합니다. 차이점은 노드의 높이 추적하고 필요시 Rotation 연산을 수행한다는 점입니다. 

* 균형을 유지하므로 Worst-case에서도 O(logN) 시간 복잡도를 가집니다.

    |---|Average-case|Worst-case|
    |---|---|---|
    |space complexity|O(N)|O(N)|
    |insertion|O(logN)|O(logN)|
    |deletion(removal)|O(logN)|O(logN)|
    |search|O(logN)|O(logN)|

* AVL 트리에서 노드의 두 하위 서브 트리의 높이 차이는 최대 1을 유지합니다.
    
* AVL 트리의 높이는 h(높이) = log N(노드수)로 제한합니다.

* 왼쪽 서브트리와 오른쪽 서브트리의 높이 매개변수 차이가 1보다 크면 불균형한 것으로 판단하여 데이터 구조를 재구성합니다. 

* 불균형 트리를 같은 균형 잡힌 트리로 재구성하려면 Rotation 연산을 해야 합니다.

* 삽입과 제거 연산이 많이 발생하는 상황에서 AVL Tree는 최선의 선택이 아닙니다. 많은 Rotation이 발생할 수 있으므로. 

* AVL Tree vs Red-Black Tree
 
    두 데이터 구조 모두 동일한 문제를 해결하기 위한 데이터 구조 입니다. 일반 연산 수행속도는 AVL Tree가 더 우수합니다. AVL Tree는 좀 더 엄격한 균형 유지 작업을 수행하기 때문입니다. 때문에 Red-Black가 Tree를 구성하는 것은 더 빠릅니다. 두 데이터 구조 모두 좋은 데이터 구조이기 때문에 어떤 것이 나은 데이터 구조라고 말하기는 어렵습니다. 상황에 따로 때로는 AVL Tree가 더 낫고 때로는 Red-Black Tree가 낫기도 합니다. 

## 2. AVL Tree 높이

![image](https://user-images.githubusercontent.com/79847020/137331487-4bf8a016-0aaa-44e1-9efe-7dbc41e507b8.png)

* 노드의 높이는 노드에서 가장 긴 리프 노드까지의 간선 수(경로) 입니다. 

* 트리의 높이는 루트 노드로 부터 리프 노드까지의 간선 수(경로) 라고 할 수 있습니다. 

* 노드의 높이를 잘 생각해보면 바닥(리프노드)부터 주어진 노드까지의 계층 높이를 생각하면 됩니다.

* 노드 수와 높이의 관계를 고려하면 h = logN 으로 높이를 제한하므로서 트리의 균형을 유지할 수 있습니다. 

<img src="https://user-images.githubusercontent.com/79847020/137388842-e27b5879-88f7-4487-a5f2-9577cce41cc8.png" width="500px"  height="250px">

* 다음 식이 성립합니다.
    
    `height = max(left child's height, right child's height) + 1`

* Null 노드의 높이는 -1로 정의합니다. 

* 왼쪽 자식 노드의 높이와 오른쪽 자신 노드의 높이가 같다는 것은 균형을 이루고 있음을 의미합니다. 

* 왼쪽 자식 노드의 높이가 오른쪽 자신 노드의 높이보다 클수록 왼쪽으로 치우처져 있음을 의미합니다. 

* 왼쪽 자식 노드의 높이가 오른쪽 자신 노드의 높이보다 작을수록 오른쪽으로 치우처져 있음을 의미합니다. 

## 3. AVL Tree 균형 인수

* h<sub>left</sub>-h<sub>right</sub>  왼쪽 서브 트리의 높이와 오른쪽 서브 트리의 높이 차를 통해서 트리의 균형을 확인할 수 있으며 이를 균형 인수라고 합니다. 

* 균형 인수는 0에 가까울 수록 주어진 서브 트리가 균형을 이루고 있음을 의미합니다.

<img src ="https://user-images.githubusercontent.com/79847020/136891571-f2b4fc22-c740-45b5-9e1b-1fcbb97024c0.png" width="150px" height="200px">

![image](https://user-images.githubusercontent.com/79847020/137389306-2526ace9-7679-4ec6-9eb1-b12cb83313f8.png)

* 그림 처럼 왼쪽 서브 트리와 오른쪽 서브 트리의 높이 변수 차이가 1 보다 크면 불균형하다고 판단할 수 있습니다.

* 왼쪽으로 치우쳐진 트리의 루트 균형 인수는 2이상의 높이 차를 가집니다. 

* 오른쪽으로 치우쳐진 트리의 루트 균형 인수는 -2이하의 높이 차를 가집니다. 

## 4. AVL Tree Rotation

![image](https://user-images.githubusercontent.com/79847020/137038014-8c4f0fff-2388-4e75-bb3d-cc7babdff999.png)

* AVL Tree에서 균형 인수를 계산하는 이유 AVL Tree의 회전 여부를 결정하기 위해서입니다.

* 회전 후에도 자식은 항상 부모 노드보다 작고 오른쪽 자식은 항상 부모 노드보다 큽니다.

* 순차 순회도 변하지 않습니다.

* Rotation 후에도 이진 탐색 트리의 속성은 유지됩니다.

* Rotation은 몇 개의 참조를 수정하기만 되기 때문에 O(1) 시간 복잡도를 가집니다. 

* Rotation 수행 후에 불균형 문제가 생길 수 있기 때문에 추가 회전 여부를 루트 노드까지 확인해야 합니다. O(logN) 실행 시간이 걸립니다.

<img src ="https://user-images.githubusercontent.com/79847020/137393871-a7c861ca-e9e3-4b51-b136-bc15b444337b.gif" height=200px width=220px>

* Right Rotation은 Root Nood를 오른쪽으로 이동시키는 것입니다.

* 균형 인수가 양수이면 왼쪽 서브트리가 무거운 상황임을 의미합니다. 이진 탐색 트리의 균형을 재조정하려면 오른쪽 회전을 해야 합니다.

* O(1) 시간 복잡도로 몇 개의 참조를 수정함으로서 Right Rotation을 수행할 수 있습니다.

<img src ="https://user-images.githubusercontent.com/79847020/137393884-46d30ef7-4e74-445c-9e80-be30b7bfcefb.gif" height=200px width=220px>

* Left Rotation은 Root Nood를 왼쪽으로 이동시키는 것입니다.

* 균형 인수가 음수이면 오른쪽 서브트리가 무거운 상황임을 의미합니다. 이진 탐색 트리의 균형을 재구성하기 위해 왼쪽 회전을 해야 합니다.

* O(1) 시간 복잡도로 몇 개의 참조를 수정함으로서 Left Rotation을 수행할 수 있습니다.

## 4.1 Rotation Case 

Rotation은 4가지 경우로 분류 할 수 있습니다.

* 왼쪽으로 트리가 치우친 경우 - LL

    ![image](https://user-images.githubusercontent.com/79847020/137435040-fd91d3b0-f33f-42f6-8a70-465ed71f1ac7.png)
    
    트리가 왼쪽으로 치우처져 있는 경우 오른쪽 Rotation을 수행합니다.

* 오른쪽으로 트리가 치우친 경우 - RR

    ![image](https://user-images.githubusercontent.com/79847020/137434837-73a5cb14-e094-4797-aab9-962f6c0e4e55.png)
    
    트리가 오른쪽으로 치우처져 있는 경우 왼쪽 Rotation을 수행합니다.

* Rotation Case 3 - 왼쪽 자식노드에 오른쪽 자식 노드만 있을 경우 - LR

    ![image](https://user-images.githubusercontent.com/79847020/137435582-9dad4578-db07-4054-9a8b-60735d157b05.png)
    
    루트 노드에서 균형 인수가 -2 이므로 왼쪽 서브트리에 치우처져 있음을 알 수 있다.
    
    왼쪽 서브트리에 오른쪽 자식 노드만 있다면 왼쪽 서브트리에서 Left Rotation을 한다.
    
    Case 1 왼쪽으로 트리가 치우진 경우와 같은 구조가 발생한다. 따라서 오른쪽 Rotation을 한다.

* Rotation Case 4 -오른쪽 자식 노드에 왼쪽 자식 노드만 있을 경우- RL

    ![image](https://user-images.githubusercontent.com/79847020/137438012-cc0f62ca-51bc-407c-9dd9-a21f3c2e1faf.png)
    
    루트 노드에서 균형 인수가 2 이므로 오른쪽 서브트리에 치우처져 있음을 알 수 있다.
    
    오른쪽 서브트리에 왼쪽 자식 노드만 있다면 오른쪽 서브트리에서 Left Rotation을 한다.
    
    Case 2 오른쪽으로 트리가 치우진 경우와 같은 구조가 발생한다. 따라서 왼쪽 Rotation을 한다.
   
## 4.2 Rotation 

* Insert

실제 주어진 트리에서 연산을 수행해봅시다.

![image](https://user-images.githubusercontent.com/79847020/137446765-d42d016b-e0a0-4edb-9fb7-2acf24a7bf68.png)

주어진 트리에서 12를 삽입하는 경우는 생각해봅시다. AVL Tree의 삽입은 이진 탐색 트리와 동일한 작업입니다.

루트 32에만 액세스 할 수 있습니다. 32보다 작고 10보다 크고 19보다 작고 16보다 작으므로 12 노드의 위치는 16 왼쪽 자식 노드일 것입니다.

![image](https://user-images.githubusercontent.com/79847020/137447086-718c67b9-9e5a-4c82-b283-98433ea750f9.png)

AVL Tree에서 작업을 수행 할 때마다 무엇을 해야 할까요 ? 

높이 변수와 균형 인수를 다시 계산하여 회전 여부를 판단합니다.

![image](https://user-images.githubusercontent.com/79847020/137450510-945bc2a3-2890-4da4-b47b-723e9de4f658.png)

리프 노드부터 높이 계산을 하고 균형 인수는 왼쪽 자식 노드의 높이와 오른쪽 자식 노드의 노드의 차 입니다.

노드 19는 균형 인수가 2 이상이 때문에 왼쪽으로 치우쳐져 있다고 판단할 수 있으며, 오른쪽 Rotation 연산이 필요합니다. 

![image](https://user-images.githubusercontent.com/79847020/137450137-c3da153d-6d14-4dc3-a5b1-34302fe1e058.png)

오른쪽 Rotation 후 트리는 이전 이진 탐색 트리의 성질을 그대로 유지합니다.

![image](https://user-images.githubusercontent.com/79847020/137451185-af38e047-d8f8-46cb-8352-9149162dc032.png)

연산을 수행했으므로 높이 변수와 균형 인수를 다시 계산하여 회전 여부를 판단합니다.

균형 인수를 통해 우리는 이진 탐색 트리가 균형상태란 것을 알 수 있습니다.

* 12, 19, 35, ,56, 78를 순서대로 입력하는 상황입니다.

![Right-Insert](https://user-images.githubusercontent.com/79847020/137468046-813ebdd9-5ec0-43c9-80b2-298cd6c69b63.gif)

* 49, 24, 61, 72, 58, 52을 순서대로 입력한느 상황입니다.

![Right-Left-Insert](https://user-images.githubusercontent.com/79847020/137468646-f8e32dc4-7829-4bf0-8061-f95dffab5425.gif)

## 5. 구현

```JAVA
package datastructure.avltree;

public interface Tree<T> {
    public Node<T> getRoot();
    // in-order traversal yields tha natural ordering
    public void traverse();
    public void insert(T data);
    public void remove(T data);
}
```

```JAVA
package datastructure.avltree;

public class AVLTree<T extends Comparable<T>> implements Tree<T> {

    private Node<T> root;

    @Override
    public Node<T> getRoot() {
        return root;
    }

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

        // 제할 노드가 싱글 Left Child 노드인 경우 삭제 후 부모노드 Height 업데이트
        updateHeight(node);

        // settle the violation
        settleViolations(node);

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

                //삭제할 노드가 리프노드인 경우 삭제 후 부모노드 Height 업데이트
                updateHeight(parent);
                settleViolations(parent);
            }

            // case 2) when we remove items with a single child
            // a single right child
            else if (node.getLeftChild() == null && node.getRightChild() != null) {
                System.out.println("Removing a node with a single right child...");
                Node<T> parent = node.getParentNode();
                if (parent != null) {

                    //the noede is a left child
                    if (parent != null && parent.getLeftChild() == node) {
                        parent.setLeftChild(node.getRightChild());
                        // the node is a right child
                    } else if (parent != null && parent.getRightChild() == node) {
                        parent.setRightChild(node.getRightChild());
                    }
                    // when we deal with the root node
                    if (parent == null) {
                        root = node.getRightChild();
                    }

                    // have to update the right child's parent
                    node.getRightChild().setParentNode(parent);
                    node = null;

                    //삭제할 노드가 싱글 Right Child 노드인 경우 삭제 후 부모노드 Height 업데이트
                    updateHeight(parent);
                    settleViolations(node);
                }
            }
            // is is approximatly the same CASE 2) BUT we have to deal with left child
            // a single left child
            else if (node.getLeftChild() != null && node.getRightChild() == null) {
                System.out.println("Removing a node with a single left child...");
                Node<T> parent = node.getParentNode();
                if (parent != null) {

                    //the noede is a left child
                    if (parent != null && parent.getLeftChild() == node) {
                        parent.setLeftChild(node.getLeftChild());
                        // the node is a right child
                    } else if (parent != null && parent.getRightChild() == node) {
                        parent.setRightChild(node.getLeftChild());
                    }
                    // when we deal with the root node
                    if (parent == null) {
                        root = node.getLeftChild();
                    }

                    // have to update the right child's parent
                    node.getLeftChild().setParentNode(parent);
                    node = null;

                    //삭제할 노드가 싱글 Left Child 노드인 경우 삭제 후 부모노드 Height 업데이트
                    updateHeight(parent);
                    settleViolations(node);
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

            //!!!!!!!!!!!!!!!!!!!!!!!!added By AVL Tree
            // settle the violation
        }
    }

    private Node<T> getPredecessor(Node<T> node) {
        if (node.getRightChild() != null) return getPredecessor(node.getRightChild());
        return node;
    }

    @Override
    public void traverse() {
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

    private void settleViolations(Node<T> node) {
        // we have to check up to the root node
        while(node != root) {
            updateHeight(node);
            settleViolationsHelper(node);
            node = node.getParentNode();
        }
    }

    private void settleViolationsHelper(Node<T> node) {
        int balance = getBalance(node);

        // OK, we know the tree is LEFT HEAVY BUT it can be left-right heavy or left-left heavy
        if(balance > 1) {
            // left right heavy situation : left rotation
            if(getBalance(node.getLeftChild()) < 0) {
                leftRotation(node);
            }
            // doubly left heavy situation then just a single right rotation is needed
            // this is the right rotation
            rightRotation(node);
        }

        // OK, we know the tree is RIGHT HEAVY BUT it can be right-left heavy or right-right heavy
        if(balance < -1) {
            // right left heavy situation : right rotation
            if(getBalance(node.getRightChild()) > 0) {
                rightRotation(node);
            }
            // doubly right heavy situation then just a single left rotation is needed
            // this is the left rotation
            leftRotation(node);
        }
    }

    // it can be done in O(1)
    private void rightRotation(Node<T> node) {
        //       D                                 B
        //   B       E     right rotation->    A       D
        // A   C           <- left rotation          C   E
        //
        System.out.println("Rotating right on Node");
        // this is the new Root node after rotation (node B)
        Node<T> tempLeftChild = node.getLeftChild();
        // Node C
        Node<T> grandChild = tempLeftChild.getRightChild();

        // make the rotation - the new root node will be the tempLeftChild;
        tempLeftChild.setRightChild(node);
        node.setLeftChild(grandChild);

        // we have to handle the parents
        if(grandChild != null)  grandChild.setParentNode(node);

        // we have to handle the parents for the node
        Node<T> tempParent = node.getParentNode();
        node.setParentNode(tempLeftChild);
        tempLeftChild.setParentNode(tempParent);

        // we have to handle the parent
        if(tempLeftChild.getParentNode()!=null && tempLeftChild.getParentNode().getLeftChild()==node) {
            tempLeftChild.getParentNode().setLeftChild(tempLeftChild);
        }

        if(tempLeftChild.getParentNode()!=null && tempLeftChild.getParentNode().getRightChild()==node) {
            tempLeftChild.getParentNode().setRightChild(tempLeftChild);
        }

        // no parent after rotation because it has become the root node
        if(node == root) root = tempLeftChild;

        // after rotation the height parameters can change
        updateHeight(node);
        updateHeight(tempLeftChild);
    }

    // it can be done in O(1)
    private void leftRotation(Node<T> node) {
        //       D                                 B
        //   B       E     right rotation->    A       D
        // A   C           <- left rotation          C   E
        //
        System.out.println("Rotating left on Node");
        // this is the new Root node after rotation (node B)
        Node<T> tempRightChild = node.getRightChild();
        // Node C
        Node<T> grandChild = tempRightChild.getLeftChild();

        // make the rotation - the new root node will be the tempRightChild;
        tempRightChild.setLeftChild(node);
        node.setRightChild(grandChild);

        // we have to handle the parents
        if(grandChild != null)  grandChild.setParentNode(node);

        // we have to handle the parents for the node
        Node<T> tempParent = node.getParentNode();
        node.setParentNode(tempRightChild);
        tempRightChild.setParentNode(tempParent);

        // we have to handle the parent
        if(tempRightChild.getParentNode()!=null && tempRightChild.getParentNode().getRightChild()==node) {
            tempRightChild.getParentNode().setRightChild(tempRightChild);
        }

        if(tempRightChild.getParentNode()!=null && tempRightChild.getParentNode().getLeftChild()==node) {
            tempRightChild.getParentNode().setLeftChild(tempRightChild);
        }

        // no parent after rotation because it has become the root node
        if(node == root) root = tempRightChild;

        // after rotation the height parameters can change
        updateHeight(node);
        updateHeight(tempRightChild);
    }

    // update the height of a given node
    private void updateHeight(Node<T> node) {
        node.setHeight(Math.max(height(node.getLeftChild()), height(node.getLeftChild())) + 1);
    }

    // it return the height parameter for a given node
    private int height(Node<T> node) {
        if (node == null) return -1;

        return node.getHeight();
    }

    // balance factor to decide the left heavy or right heavy cases
    private int getBalance(Node<T> node) {
        if (node == null) return 0;

        return height(node.getLeftChild()) - height(node.getRightChild());
    }
}
```

## Balanced Binary Search Tree Application

* Game Engine

    거의 모든 게임 엔진은 이진 탐색 트리를 사용하여 렌더링하는 개체를 결정합니다. 
    
* Compiler

    컴파일러가 프로그램 언어의 소스코드의 구문 분석을 할 때 이진 탐색 트리가 사용됩니다. 컴파일러에서 사용되는 트리를 Synctax Tree라고도 합니다.
    
* AVL Sort

    균형 이진 탐색를 사용하여 정렬을 하기도 합니다. 
    
    N개의 데이터를 AVL Tree의 삽입 후 in-order Traversal합니다. 
    
    시간복잡도는 O(NlogN) + O(N) = O(NlogN) 입니다.
    
* Database

    AVL Tree는 데이터베이스를 구성하는데도 사용됩니다. 
    
    하지만 삽입과 제거 연산이 많이 발생하는 상황에서 AVL Tree는 많은 Rotation으로 발생으로 썩 좋은 데이터 구조는 아닙니다.(AVL Tree단점)
    
    이 경우 Red-Black 트리가 AVL Tree보다 더 좋은 자료 구조로 간주됩니다.
    
    하지만 최선의 선택은 B-Tree로 알려져 있습니다. 
    
## Quiz

* What is the height parameter of a given node in an AVL tree?

      The height of a node is the longest path from the actual node to a leaf node
      
* When to use AVL Trees ?

      When there are a lot of search operations
      
      Because AVL trees are more rigidly balanced, thats why O(logN) time complexity is always true. So search will guaranteed to be very fast.
      
* Why to bother with rotations and why to make sure the binary tree is balanced?

      This is how O(logN) logarithmic running time is guaranteed
      
* What is the balance factor of a given node?

      height(node.getLeftChild() - height(node.getRightChild())     
