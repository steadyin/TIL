# Iterator 

  Iterator와 Iterable은 모두 Java의 인터페이스로 매우 유사하게 보이지만 두 가지가 다릅니다. 간단히 말해서, 임의의 클래스가 Iterable 인터페이스를 구현하면 Iterator를 사용하여 해당 클래스의 객체를 반복할 수 있는 기능을 얻습니다.

  두 인터페이스의 차이점에 대해서 얘기해보자.

## 1. Iterable 인터페이스는 순회할 수 있는 Collection을 나타냅니다. 

  `Iterable` 인터페이스를 구현하면 객체가 for-each 루프를 사용할 수 있습니다. 내부적으로 객체의 iterator() 메소드를 호출하여 이를 수행합니다. 예를 들어 다음 코드는 List 인터페이스가 Collection 인터페이스를 Extend 하고 Collection 인터페이스가 Iterable 인터페이스를 Extend 하기 때문에 동작할 수 있습니다.

  ```java
  List<String> persons = new ArrayList<>(Arrays.asList("A", "B", "C"));

  for (String person: persons) {
      System.out.println(person);
  }
  ```

  또한 Iterable 인터페이스의 각 요소들에 대해 작업을 수행하는 Java8부터 forEach() 메소드를 호출할 수 있습니다. forEach()의 default 구현에서도 for-each를 내부적으로 사용합니다.

  ```java
  List<String> persons = new ArrayList<>(Arrays.asList("A", "B", "C"));

  persons.forEach(person -> System.out.println(person));
  ```
  
  반면 `Iterator`는 일종의 Collection 객체를 반복할 수 있는 인터페이스입니다. Iterator를 반복하려면 아래와 같이 while 루프에서 hasNext() + next() 메서드를 사용할 수 있습니다.
  
  ```java
  Iterator<Integer> `Iterator` = Arrays.asList(1, 2, 3, 4, 5).iterator();
  while (iterator.hasNext()) {
      System.out.println(iterator.next());
  }
  ```
  
  Java8부터 forEachRemaining() 메서드를 사용하여 Iterator를 통해 쉽게 반복할 수 있습니다.
  
  ```java
  Iterator<Integer> `Iterator` = Arrays.asList(1, 2, 3, 4, 5).iterator();
  iterator.forEachRemaining(System.out::println);
  ```
  
  람다를 사용하여 Iterator를 Iterable로 변환하는 것을 래핑하여 for-each 루프 내에서 Iterator를 사용할 수도 있습니다.
  
  ```java
  for (Integer i: (Iterable<Integer>) () -> iterator) {
    System.out.println(i);
  }
  ```
  
  
  https://www.techiedelight.com/differences-between-iterator-and-iterable-in-java/
  
  
  
  
  
  
  
  
  
  
