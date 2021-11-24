## Array ? 

Array는 같은 타입의 데이터들이 0부터 시작하는 정수 값으로 식별되는 데이터 구조

## Array 종류

Static Array : 정적 배열, 일반적인 배열, 크기가 변경되지 않습니다. 

Dynamic Array : 동적 배열, 크기가 변경 되는 배열입니다. 정적 배열로 구현됩니다.

## Array 특징 

1. Random Access : 데이터에 접근할 때 인덱스 기반으로 직접 접근할 수 있는 특성입니다. (시간복잡도 O(1))

2. 연속적인 메모리 공간이 할당됩니다.

3. 연속적인 메모리 공간에 있기 때문에 메모리 주소를 계산할 수 있습니다. <br>
(memory address = arry's address + index * data size(int의 경우 32bit))

4. Array의 참조 주소는 기본적으로 첫번째 데이터의 메모리 위치입니다.

5. 다차원 배열로 다양한 형태를 표현할 수 있습니다.

## Array 장점

1. 사용과 구현이 간단해서 다른 자료구조(스택, 큐, 해시 테이블)에서 응용됩니다. 

2. Random Access, 인덱스 기반으로 직접 접근할 수 있습니다. ( 시간복잡도 O(1) )

3. 빠른 데이터 구조 입니다. 순차 접근시에도 연결 리스트보다 우수한 성능을 가집니다.

4. Array는 마지막 위치에 항목을 추가하거나 삭제할 때 유리합니다. 


## Array 단점

1. 컴파일 시점에 배열의 크기를 알아야 합니다. 동적 자료 구조가 아닙니다. 

2. 동적 자료 구조가 아니기 때문에 Array가 가득 찰 때마다 배열을 다시 생성해 옮겨야 합니다. (선형 시간 복잡도 O(N))

3. 다른 타입의 데이터를 저장할 수 없습니다. 

4. Array는 특정 위치 삽입, 삭제에 유리한 자료구조가 아닙니다. 


## Arrays 연산 

#### 삽입 연산 

Array 마지막 인덱스에 새 데이터를 삽입할 수 있습니다. 

Array에 빈 공간이 있다면 O(1)의 시작복잡도를 가집니다.

```
Array 자료구조가 가득차면 어떤 일이 발생합니까?

1. 일반적으로 메모리 크기가 두배인 더 큰 메모리 공간을 할당합니다.
2. 기존의 모든 항목을 하나씩 새 배열에 복사합니다.
3. 이런 연산 때문에 O(N)의 시간복잡도 가집니다.
4. 이는 Array의 병목(Bottleneck) 현상입니다.
```

---

#### 특정 인덱스 삽입 연산

Array 특정 인덱스에 새 데이터를 삽입하기 위해서 다음과 과정을 거칩니다.

1. 특정 인덱스부터 존재하는 모든 데이터들을 다음 인덱스 위치로 이동시켜야 합니다.
2. 삽입할 데이터를 특정 인덱스에 삽입합니다.

이 과정은 특정 인덱스 뒤에 있는 데이터 전체가 이동하므로 O(N) 선형 시간복잡도를 가집니다.

---

#### 삭제 연산

Array 마지막 인덱스에 있는 데이터를 삭제할 수 있습니다. 

O(1) 상수 시간복잡도를 가집니다.

---

#### 특정 인덱스 삭제 연산

Array 특정 인덱스에 데이터를 삽입하기 위해서 다음과 과정을 거칩니다.

1. 특정 인덱스에 접근하여 데이터를 제거합니다. 
2. 특정 인덱스 뒤에 위치하는 데이터를 한단계씩 앞으로 이동하여 빈공간을 채웁니다.

이 과정은 특정 인덱스 뒤에 있는 데이터 전체가 이동하므로 O(N) 선형 시간복잡도를 가집니다.

```
데이터의 인덱스를 알지 못할 때 삭제하려면 어떻게 해야합니까 ?

1. 순차 검색(Sequence Search)을 통해서 항목을 찾습니다. 시작복잡도 O(N)
2. 해당 항목을 삭제합니다. 시간복잡도 O(1)
3. 빈 공간을 채웁니다. 시간복잡도 O(N)

총 시간복잡도 O(N)
```

## Array 사용

#### 컴파일 에러

```JAVA
int[] nums = new int[];
```

다음과 같이 배열을 선언하면 컴파일 에러가 발생합니다. 

배열은 동적인 데이터 구조가 아닙니다. 

컴파일 타임에 배열의 크기를 지정해주야 합니다.

#### 데이터 접근

```JAVA
public class App {

    public static void main(String[] args) {

        //Array는 Dynamic Data Structure이 아닙니다.
        //컴파일 타임에 우리는 Array의 크기를 정의해야합니다.
        int[] nums = new int[10];

        //랜덤 인덱싱(Random indexing)
        //배열의 공간에 인덱스를 사용해서 접근할 수 있습니다.
        //O(1)
        for(int i=0; i<10; i++) nums[i] = i;

        //인덱스를 안다면, 항목에 직접 접근하여 수정 할 수 있습니다.
        nums[0] = 100;

        //인덱스를 모른다면, 선형 검색을 수행합니다.
        //LINEAR SEARCH O(N)
        //항목 6을 검색합니다.
        for(int i=0; i<10; i++) {
            if(nums[i]==6)
                System.out.println("We have found the item at index : " + i);
        }
    }
}
```

랜덤 인덱싱(Random Access): 인덱스를 아는 경우 Array 데이터에 바로 접근하고 시간복잡도 O(1)을 가집니다.

선형 탐색(Linear Search) : 인덱스를 모르는 경우 선형 탐색을 통해서 접근하고 시간복잡도 O(N)을 가집니다.

선형 탐색보다 더 나은 시간 복잡도을 가진 이진 검색 트리, 해시 테이블을 고려해봐야 합니다.

## Quiz

#### Arry 데이터 구조에 관한 한 효율적인 운영은 무엇입니까?

데이터 구조의 끝에서 항목 삽입 또는 제거(예: 마지막 인덱스 포함)

#### 랜덤 액세스란?

랜덤 액세스란 항목이 메모리에서 서로 바로 옆에 있기 때문에 O(1) 상수 실행 시간에 인덱스를 기반으로 항목에 액세스할 수 있음을 의미합니다.

#### 어레이의 중요한 단점은 무엇입니까?

데이터 구조의 "빈 공간"을 처리해야 합니다. 즉, 작업을 지연시킬 수 있는 많은 항목 이동을 필요로 합니다. 

## Reversing an array in-place overview

The problem is that we want to reverse a T[] array in O(N) linear time complexity and we want the algorithm to be in-place as well!
For example: input is [1,2,3,4,5] then the output is [5,4,3,2,1]

O(N) 시간복잡도로 추가 메모리 없이 배열을 반전시키자.

```JAVA
// 시작복잡도 O(N), 추가 메모리 없는 조건
public class ReverseArray {
    public int[] reversArray(int[] nums) {

        int startIndex = 0;

        int endIndex = nums.length - 1;

        while(endIndex > startIndex) {

            swap(nums, startIndex, endIndex);

            startIndex++;
            endIndex--;
        }

        return nums;

    }

    private void swap(int[] nums, int index1, int index2) {

        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;

    }
}
```

## Anagram Problem

Construct an algorithm to check whether two words (or phrases) are anagrams or not!
"An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once"
example : restful --> fluster

Anagrams : 원 문자(Subject)를 재배열해서 만들 수 있는 단어

두 단어가 Anagrams인지 확인하는 알고리즘

```JAVA

public class AnagramProblem {
    public boolean solve(char[] s1, char[] s2) {

        if (s1.length != s2.length) return false;

        // bottleneck because it has O(NlogN) running time
        Arrays.sort(s1);
        Arrays.sort(s2);

        // running time is O(N)
        for (int i = 0; i < s1.length; i++) 
            if (s1[i] != s2[i]) 
                return false;

        //O(NlogN)+O(N) = O(NlogN)
        return true;
    }

}
```

## Duplicates in an array problem solution

The problem is that we want to find duplicates in a T[] one-dimensional array of integers in O(N) running time where the integer values are smaller than the length of the array!

정수값으로 구성 된 1차원 배열에서 중복을 찾아내는 문제. <br>
음수는 사용할 수 없다. <br>
(O(N) 시간복잡도)

1) Brute-force Approach : O(N<sup>2</sup>)
2) HashMap : O(N), not an in-place algorithm
3) using abolute values : O(N), in-place algorithm


```JAVA
public class RepeatedIntegersProblem {
    public void solve(int[] array) {
        //O(N) running time
        for (int i = 0; i < array.length; i++) {
            // if value is postive, flip 
            if (array[Math.abs(array[i])] > 0) {
                array[Math.abs(array[i])] = -array[Math.abs(array[i])];
            // if negative, it mean repetition
            } else {
                System.out.println(Math.abs(array[i]) + " is a repetition !");
            }
        }
    }
}
```


