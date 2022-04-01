# 추상 클래스(Abstract Class) vs 인터페이스(Interface)

실무에서 Class를 디자인 할 때 상위 Class를 토대로 하위 Class를 디자인 하는 경우 Bottom-up으로 디자인 하는 것을 Generalization이라 하며 Top-Down으로 디자인 하는 것을 Specialization이라고 합니다. 그리고 Super Class와 Sub Class의 종속 관계를 만들 수 있습니다. 이런 관계를 객체 지향에서는 상속(Inheritance)라고 합니다.

Super Class를 어느 정도 구현한 상태에서 Sub Class를 디자인 하는 경우에 Sub Class는 Super Class에는 구현이 안되있는 필요한 부분을 더 구현할 수 있습니다. 이렇게 Super Class가 어느 정도 구현이 되있지만 완성된 형태를 Sub Class에 요구하는 경우 Super Class를 Java에서는 Abstrat Class로 디자인 합니다.

Abstrat Class에는 Sub Class에 상속될 수 있는 멤버 변수나 메소드들이 있을 수도 있고 없을 수도 있습니다. 또한 구현 부분이 비어있는 Abstract Method를 선언할 수도 있습니다. Abstrat Class 역시 Java Class이기 때문에 다중 상속이 불가능합니다.

참고로 Abstract Class를 상속받은 Sub Class는 Abstract Method가 있으면 Abstract Method의 Body를 구현해주거나 구현하지 않으면 Sub Class 역시 Abstract Class로 선언되어야 합니다.

Interface는 Abstract와 비슷하지만 실질적인 구현이 전혀 없는 Abstract로 구성된 빈 Class입니다. 즉 구현은 Sub Class에게 맡기고 단지 Sub Class들이 구현해야 되는 메소드들을 정의해주는 일종의 규약 같은 것입니다.

이 개념은 Class 디자인에서 상당히 중요하게 생각되는데 그 이유는 Interface를 보면 Sub Class들이 어떤 기능을 가지고 있을지 파악 되기 때문입니다. 물론 실질적인 기능은 Sub Class 마다 다르게 구현될 수 있습니다.

그래서 Interface는 일반적으로 내부가 비어진 빈 껍데기와 같은 Empty Shell로 구성되어있습니다. 물론 Interface 내에 메소드 뿐만 아니라 변수를 선언할 수도 있지만 Interface내 변수는 자동적으로 public static final이 붙게되서 변수가 아닌 상수가 됩니다.

인터페이스는 구현의 개념이기 때문에 다중 구현 또한 가능합니다.

정리하면

* Abstract Class A extends B -> A가 B를 확장한 클래스 관계를 디자인할 때, Interface는 A has B -> A는 B의 기능을 포함한다를 디자인 할 때 사용됩니다.
* Abstract Class는 단일 상속(Sigle Inheritance)만 가능하며, Interface는 다중 상속(Multiple Inheritance) 다중 구현(Multiple Implementations)이 가능합니다. 
* Abstract Class는 모든 접근 제어자(private, public, protected, default)가 선언 가능하며, Interface는 public만 가능합니다.
* Abstract Class는 일반적인 변수와, 상수가 사용 가능하며, Interface는 상수만 사용이 가능합니다.
* Abstract Class는 추상 메소드, 일반 메소드 둘다 사용이 가능한 반면에 Interface는 오직 추상 메소드만 사용이 가능합니다.
* Interface에 Java8에서 Default, Static Java9에서 Private 메소드가 추가되면서 기능이 보완되었습니다.
