# Iterator 
Iterator와 Iterable은 모두 Java의 인터페이스로 매우 유사하게 보이지만 두 가지가 다릅니다. 간단히 말해서, 임의의 클래스가 Iterable 인터페이스를 구현하면 Iterator를 사용하여 해당 클래스의 객체를 반복할 수 있는 기능을 얻습니다.

두 인터페이스의 차이점에 대해서 얘기해보자.

1. Iterable 인터페이스는 순회할 수 있는 Collection을 나타냅니다. Iterable 인터페이스를 구현하면 객체가 for-each 뤂를 사용할 수 있습니다. 그것은 내부적으로 객체의 iterator() 메소드를 호출함으로서 동작합니다. 예를 들어 다음 코드는 List 인터페이스가 Collection 인터페이스를 Extend 하고 Collection 인터페이스가 Iterable 인터페이스를 Extend 하기 때문에 동작합니다.

```java
List<String> persons = new ArrayList<>(Arrays.asList("A", "B", "C"));
 
for (String person: persons) {
    System.out.println(person);
}
```

Java8 부터 Iterable

Iterable의 각 요소에 대해 주어진 작업을 수행하는 Java 8부터 시작하는 iterable에서 forEach() 메소드를 호출할 수도 있습니다. forEach()의 기본 구현도 내부적으로 for-each를 사용한다는 점에 유의할 필요가 있다.
