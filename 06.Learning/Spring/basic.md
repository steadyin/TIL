[보여지는 텍스트](# 1. 스프링 기본)

# 스프링 핵심 원리 - 기본

# 1. 스프링 기본

* EJB(Enterprise Java Beans)
  * 과거 자바 표준 기술
  * 컨테이너 기술, 트랜잭션, 분산 기술, ORM(EntityBean) 등 고급기술을 상대적으로 편하게 기능
  * 기술에 종속적인 복잡하고 느린 EJB에 의존해 개발할 수 밖에 없었다.

* J2EE Design and Development - 로드 존슨
  * EJB 문제점 지적
  * EJB 없이도 확장 가능한 애플리케이션을 개발할 수 있는 30,000라인 이상의 기반 기술을 예제 코드로 선보임
  * BeanFactory, ApplicationContext, POJO, 제어의 역전, 의존관계 주입

* 스프링 역사
  * 유겐 휠러, 얀 카로프가 로드 존슨에게 오픈소스 프로젝트 제안
  * J2EE(EJB)라는 겨울을 넘어 새로운 시작의 의미로 스프링이라고 명명

* POJO(Plain Old Java Object)
  객체지향적인 원리에 충실하면서, 환경과 기술에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 순수한 자바 오브젝트를 의미한다.

* 스프링 프레임 워크
  * 핵심 기술 : 스프링 DI 컨테이너, AOP, 이벤트
  * 웹 기술 : 스프링MVC, 스프링 WebFlux
  * 데이터 접근 기술 : 트랜잭션, JDBC, ORM지원 XML지원
  * 기술통합 : 캐시, 이메일, 원격 접근, 스케줄링
  * 테스트 : 스프링 기반 테스트 지원
  * 언어 : 코틀린, 그루비
  * 최근에는 스프링 부트를 통해서 스프링 프레임워크의 기술들을 편리하게 사용

* 스프링부트
  * 스프링을 편리하게 사용할 수 있도록 지원
  * 단독으로 실행할 수 있는 스프링 애플리케이션을 쉽게 생성
  * 톰캣 같은 웹서버를 내장해서 별도의 웹 서버를 설치하지 않아도 독립적으로 실행된다.
  * 손쉬운 빌더 구성을 위한 Starter 종속성 제공
  * 스프링과 서드파티 라이브러리 자동 구성(라이브러리별 버전 관리)
  * 메트릭, 상태, 외부구성 같은 프로덕션 준비 기능 제공

* 스프링의 의미
  * 스프링 프레임워크란 문맥에 따라 다르게 사용된다.
  * 스프링 DI 컨테이너 기술
  * 스프링 프레임워크
  * 스프링 부트, 스프링 프레임워크 등을 모두 포함한 스프링 생태계

* 스프링 핵심 컨셉
  * 스프링은 자바 언어 기반의 프레임워크
  * 자바 언어의 가장 큰 특징 - 객체 지향 언어
  * 스프링은 객체 지향 언어가 가진 강력한 특징을 살려내는 프레임워크
  * 스프링은 좋은 객체 지향 애플리케이션을 개발할 수 있게 해주는 도와주는 프레임 워크

* 객체 지향 특징 (캡 상추다)
  * 추상화
  * 캡슐화
  * 상속
  * 다형성

* 객체 지향 프로그래밍
  * 객체 지향 프로그래밍은 프로그래밍을 명령어로 보는 시각에서 벗어나 독립된 단위, 객체간의 관계로 문제를 해결하는 방식을 말한다.
  * 객체 지향 프로그래밍은 프로그램을 유연하고 변경이 용이하게 만들기 때문에 대규모 소프트웨어 개발에 많이 사용된다.

* 다형성
  * 역할과 구현으로 세상을 구분하면 세상이 단순해지고 유연해지면 변경도 편리해진다.
  * 자바언어에서는 역할은 인터페이스이고 구현은 인터페이스를 구현한 클래스, 구현 객체를 말한다.
  * 인터페이스를 구현한 객체 인스턴스를 실행 시점에 유연하게 변경할 수 있다.
  * 클라이언트를 변경하지 않고 서버의 구현 기능을 유연하게 변경할 수 있다.
  * 역할과 구현을 분리하므로서 유연하고 변경에 용이한, 확장 가능한 설계를 할 수 있다.
  * 역할 자체가 변하면 서버와 클라이언트 모두 큰 변경이 발생하므로 인터페이스를 잘 설계하는 것이 중요하다.

* 스프링과 객체지향
  * 스프링은 다형성을 극대화해서 이용할 수 있게 도와준다.
  * 제어의역전(IoC), 의존관계 주입(DI)는 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원한다.

* 좋은 객체 지향 설계의 5가지 원칙(SOLID)
  * SRP(Single Responsibility Principle) : 단일 책임 원칙
    * 한 클래스는 하나의 책임만 가져야 한다.
    * 책임이라는 것은 클수도 있고 작을 수도 있꼬 문맥과 상황에 따라 다르다. 모호하다.
    * 중요한 기준은 변경이다. 변경이 있을 때 파급효과가 적으면 단일 책임 원칙을 잘 따른 것이다.
    * 예) UI 변경, 객체의 생성과 사용을 분리
  * OCP(Open-Closed Principle) : 개방 폐쇠 원칙
    * 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
    * 역할과 구현이 분리되어 있지 않다면 OCP 위반
    * 역할과 구현이 분리되어 있더라도 클라이언트에서 완벽하게 구현을 분리하기 위해서는 별도의 연관관계 설정자가 필요하다.
  * LSP(Liskov Substitution Principle) : 리스코프 치환 원칙
    * 서브 타입은 언제나 자신의 기반타입(base type)으로 교체할 수 있어야 한다.
    * 하위 클래스 `is a kind of` 상위 클래스 - 하위 클래스는 상위 클래스의 한 종류이다.
    * 구현 클래스 `is able to` 인터페이스 - 구현 클래스는 인터페이스할 수 있어야 한다.
  * ISP(Interface Segragation Principle) : 인터페이스 분리 원칙
    * 범오적인 인터페이스보다 구체적인 인터페이스가 낫다.
  * DIP(Dependency Inversion Principle) : 의존관계 역전 원칙
    * 추상화에 의존해야지 구체화에 의존하면 안된다.
    * 구현 클래스에 의존하지 말고 인터페이스에 의존하지 말라는 뜻이다.

* 객체 지향 설계와 스프링
  * 스프링은 다음 기술로 다형성 + OCP,DIP를 가능하게 지원한다.
    * DI(Dependency Injection) : 의존관계 주입
    * DI 컨테이너 제공
  * 클라이언트 코드의 변경 없이 기능 확장
  * 좋은 객체 지향 프로그래밍을 위해 DIP, OCP 원칙을 지키면서 개발하는 것은 비용이 큰 비효율적인 일이다. DIP, OCP를 지키면서 개발을 할 수 있도록 개발하기 위해서는 결국 DI 컨테이너를 만들게
    된다.

* 정리
  * 객체 지향의 핵심은 다형성
  * 다형성 만으로는 구현 객체를 변경할 때 클라이언트 코드도 함께 변경된다.
  * 다형성 만으로는 OCP, DIP를 지킬 수 없다
  * 또 다른 개념이 필요하다.
  * 모든 설계에 역할과 구현을 분리하자.
  * 하지만 인터페이스를 도입하면 추상화 라는 비용이 발생한다.
  * 실무에서 기능을 확장할 가능성이 없다면 구현 클래스를 직접 사용하고 향후 꼭 필요할 때 리팩터링해서 인터페이스를 도입하는 것도 좋은 방법이다.

# 2. 스프링 핵심 원리 이해 1 - 예제 만들기

## 2.1 비즈니스 요구사항 설계

* 회원
  * 회원을 가입하고 조회할 수 있다.
  * 회원은 일반과 VIP 두 가지 등급이 있다.
  * 회원 데이터는 자체 DB를 구축할 수 있고 외부 시스템과 연동할 수 있다.(미확정)

* 주문과 할인 정책
  * 회원은 상품을 주문할 수 있다.
  * 회원 등급에 따라 할인 정책을 적용할 수 있다.
  * 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라 (나중에 변경 가능하다.)
  * 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고 오픈 직전까지 고민을 미루고 싶다. 회창의 경우 할인을 적용하지 않을 수도 있다. (미확정)

요구사항을 보면 회원 데이터 할인 정책 값은 부분은 지금 결정하기 어려운 부분이다. 앞에서 배운 객체 지향 설계 방법을 활용하자. 인터페이스를 만들고 구현체를 언제든지 갈아끼울 수 있도록 설계하면 된다.

## 2.2 회원 도메인 설계 및 개발

* 회원
  * 회원을 가입하고 조회할 수 있다.
  * 회원은 일반과 VIP 두 가지 등급이 있다.
  * 회원 데이터는 자체 DB를 구축할 수 있고 외부 시스템과 연동할 수 있다.(미확정)

![img_18](https://user-images.githubusercontent.com/79847020/156158131-a9d7c262-5656-4cb6-a4d8-7f7af89b1f7c.png)

![img_19](https://user-images.githubusercontent.com/79847020/156158137-dafe5799-b409-4cf4-ab6b-266a12cb145a.png)

![img_21](https://user-images.githubusercontent.com/79847020/156158156-3d95e892-d493-475a-8e2a-6be48910675e.png)

* 회원 등급 Grade

```java
public enum Grade {
    BASIC,
    VIP
}
```

* 회원 엔티티 Member

```java
public class Member {

    private Long id;
    private String name;
    private Grade grade;

    public Member(Long id, String name, Grade grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Grade getGrade() {
        return grade;
    }

    public void setGrade(Grade grade) {
        this.grade = grade;
    }
}
```

* 회원 저장소 인터페이스 MemberRepository

```java
public interface MemberRepository {

    void save(Member member);

    Member findById(Long memberId);
}
```

* 메모리 회원 저장소 구현체 MemoryMemberRepository

```java
public class MemoryMemberRepository implements MemberRepository {
    private static final Map<Long, Member> store = new HashMap<>();

    @Override
    public void save(Member member) {
        store.put(member.getId(), member);
    }

    @Override
    public Member findById(Long memberId) {
        return store.get(memberId);
    }
}
```

데이터 베이스가 아직 확정이 안되었다. 단순하게 메모리 회원 저장소를 구현하자. HashMap은 동시성 이슈가 발생할 수 있다. ConcurrentHashMap을 사용하자.

* 회원 서비스 인터페이스 MemberService

```java
public interface MemberService {

    void join(Member member);

    Member findMember(Long memberId);
}
```

* 회원 서비스 구현체 MemberServiceImpl

```java
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository;

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

## 2.3 회원 도메인 실행과 테스트

* 회원 가입 MemberApp

```java
public class MemberApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberServiceImpl();

        Member memberA = new Member(1L, "memberA", Grade.VIP);
        memberService.join(memberA);

        Member findMember = memberService.findMember(1L);
        System.out.println("new member = " + memberA.getName());
        System.out.println("find member = " + findMember.getName());
    }
}
```

애플리케이션 로직으로 테스트 하는 것은 좋은 방법이 아니다. JUnit을 활용하자.

* 회원 가입 테스트 MemberServiceTest

```java
public class MemberServiceTest {

    MemberService memberService = new MemberServiceImpl();

    @Test
    public void join() throws Exception {
        //given
        Member member = new Member(1L, "memberA", Grade.VIP);
        //when
        memberService.join(member);
        Member findMember = memberService.findMember(1L);

        //then
        Assertions.assertThat(findMember).isEqualTo(member);
    }
}
```

회원 도메인 설계의 문제점

* 구현체를 바꾸기 위해서는 클라이언트 코드를 수정해야 한다.
* 결국 구현체에 의존하므로 OCP, DIP 위반이 된다.

## 2.4 주문과 할인 도메인 설계

* 주문과 할인 정책
  * 회원은 상품을 주문할 수 있다.
  * 회원 등급에 따라 할인 정책을 적용할 수 있다.
  * 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라 (나중에 변경 가능하다.)
  * 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고 오픈 직전까지 고민을 미루고 싶다. 회창의 경우 할인을 적용하지 않을 수도 있다. (미확정)

![img_22](https://user-images.githubusercontent.com/79847020/156158168-722588f0-5871-4e3b-b84d-b9d6d3054f21.png)

![img_24](https://user-images.githubusercontent.com/79847020/156158182-a491db0f-f4a5-44a5-9509-7320a0d652ee.png)

![img_25](https://user-images.githubusercontent.com/79847020/156158192-627aa5a4-a2fa-44b5-a007-0f3249459eae.png)

![img_26](https://user-images.githubusercontent.com/79847020/156158198-e42d9054-d84a-4a4a-b52d-104866a1c31b.png)

* 할인 정책 인터페이스 DiscountPolicy

```java
public interface DiscountPolicy {

    /**
     * @return 할인 대상 금액
     */
    int discount(Member member, int price);
}
```

* 정액 할인 정책 구현체 FixDiscountPolicy

```java
public class FixDiscountPolicy implements DiscountPolicy {
    private final int discountFixAmount = 1000;

    @Override
    public int discount(Member member, int price) {
        if (member.getGrade() == Grade.VIP) return discountFixAmount;
        else return 0;
    }
}
```

* 주문 엔티티 Order

```java
public class Order {
    private Long memberId;
    private String itemName;
    private int itemPrice;
    private int discountPrice;

    public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
        this.memberId = memberId;
        this.itemName = itemName;
        this.itemPrice = itemPrice;
        this.discountPrice = discountPrice;
    }

    public int calculatePrice() {
        return itemPrice - discountPrice;
    }

    public Long getMemberId() {
        return memberId;
    }

    public void setMemberId(Long memberId) {
        this.memberId = memberId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public int getItemPrice() {
        return itemPrice;
    }

    public void setItemPrice(int itemPrice) {
        this.itemPrice = itemPrice;
    }

    public int getDiscountPrice() {
        return discountPrice;
    }

    public void setDiscountPrice(int discountPrice) {
        this.discountPrice = discountPrice;
    }

    @Override
    public String toString() {
        return "Order{" +
                "memberId=" + memberId +
                ", itemName='" + itemName + '\'' +
                ", itemPrice=" + itemPrice +
                ", discountPrice=" + discountPrice +
                '}';
    }
}
```

* 주문 서비스 인터페이스 OrderService

```java
public interface OrderService {
    Order createOrder(Long memberId, String itemName, int itemPrice);
}
```

* 주문 서비스 구현체 OrderServiceImpl

```java
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
  
    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);

        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

주문 생성 요청이 오면 회원 정보를 조회하고 할인 정책을 적용한 다음 주문 객체를 생성해서 반환한다. 메모리 회원 리포토리와 고정 금액 할인 정책을 구현체로 생성한다.

## 2.5 주문과 할인 도메인 실행과 테스트

* 주문과 할인 정책 테스트

```java
public class OrderApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberServiceImpl();
        OrderService orderService = new OrderServiceImpl();

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "ItemA", 10000);

        System.out.println("order = " + order);
    }
}
    
```

```text
order = Order{memberId=1, itemName='itemA', itemPrice=10000, discountPrice=1000}
```

JUnit 테스트를 활용하자.

```java
class OrderServiceImplTest {

    MemberService memberService = new MemberServiceImpl();
    OrderService orderService = new OrderServiceImpl();

    @Test
    public void createOrder() throws Exception {
        //given
        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);

        //when
        memberService.join(member);
        Order order = orderService.createOrder(memberId, "ItemA", 10000);

        //then
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}
```

# 3. 스프링 핵심 원리 이해 2 - 객체 지향 원리 적용

## 3.1 새로운 할인 정책 개발

새로운 할인 정책을 확장해보자.

기획자 : 주문 금액당 할인하는 정률 %할인으로 변경하고 싶어요. 10%로 지정해두면 10000원 주문시 1000원 할인해주고 20000원 주문시 2000원 할인되도록.

애자일 소프트웨어 개발 선언 "계획에 따르기 보다는 변화에 대응하기를"
애자일 소프트웨어 개발 선언 : http://agilemanifesto.org/iso/ko/manifesto.html

![img_27](https://user-images.githubusercontent.com/79847020/156158214-a8993c1a-5a24-4236-98f9-2e4f6cf23d40.png)

* 할인 정책 추가 RateDiscountPolicy

```java
public class RateDiscountPolicy implements DiscountPolicy {

    private final int discountPercent = 10;

    @Override
    public int discount(Member member, int price) {
        if (member.getGrade() == Grade.VIP) {
            return price * discountPercent / 100;
        }
        return 0;
    }
}
```

* 테스트 작성

```java
class RateDiscountPolicyTest {

    DiscountPolicy discountPolicy = new RateDiscountPolicy();

    @Test
    @DisplayName("VIP는 10% 할인이 적용되어야 한다.")
    public void vip_o() throws Exception {
        //given
        Member member = new Member(1L, "memberVIP", Grade.VIP);
        //when
        int discount = discountPolicy.discount(member, 10000);
        //then
        Assertions.assertThat(discount).isEqualTo(1000);
    }

    @Test
    @DisplayName("VIP가 아니면 할인이 적용되지 않아야 한다.")
    public void vip_x() throws Exception {
        //given
        Member member = new Member(1L, "memberBASIC", Grade.BASIC);

        //when
        int discount = discountPolicy.discount(member, 10000);

        //then
        Assertions.assertThat(discount).isEqualTo(0);
    }
}
```

## 3.2 새로운 할인 정책 적용과 문제점

할인 정책을 변경하려면 클라이언트인 OrderServiceImple 코드를 고쳐야 한다.

OrderServiceImpl

```java
public class OrderServiceImpl implements OrderService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy(); // 수정
...
```

지금까지 나름 대로 역할과 구현을 충실하게 분리했다. 다형성을 활용하고 역할을 분리하여 인터페이스와 구현체를 구현했다. OCP와 DIP 같은 객체지향 설계 원칙을 지키려 노력했다. 하지만
OrderServiceImpl(클라이언트)에서 역할(DiscountPolicy)에 의존할 뿐만 아니라 구현(FixDiscountPolicy, RateDiscountPolicy) 에도 의존하고 있다.

결국 구체적인 것에 의존하므로 DIP를 위반이고 기능을 확장하려면 클라이언트 코드를 수정해야 하므로 OCP를 위반하고 있다.

![img_28](https://user-images.githubusercontent.com/79847020/156158229-39b8dedc-ac3e-42b2-babc-f14ad84e5020.png)

![img_29](https://user-images.githubusercontent.com/79847020/156158235-946a2338-7b7a-4534-9412-080c4bd0e854.png)

![img_30](https://user-images.githubusercontent.com/79847020/156158240-88f12d9c-aa5e-4678-94b1-58458439eee1.png)

어떻게 문제를 해결할 수 있을까? -> 인터페이스에만 의존하도록 설계를 변경하자.

![img_31](https://user-images.githubusercontent.com/79847020/156158250-bb88c10e-a315-4e67-b403-d623719024a6.png)

```java
public class OrderServiceImpl implements OrderService {
    //private final DiscountPolicy = new RateDiscountPolicy();
    private DiscountPolicy discountPolicy;
}
```

인터페이스만 의존하도록 설계와 코드를 변경했다. 하지만 구현체가 없으므로 실행할 수 없다.

결국, 문제를 해결하려면 누군가가 클라이언트인 OrderServiceImpl에 DiscountPolicy의 구현 객체를 대신 생성하고 주입해주어야 한다.

## 3.3 관심사의 분리

관심사를 분리하자. 클라이언트는 인터페이스를 사용하는 것에, 구현체는 기능을 구현하는 것에 집중하고 구현체를 생성하고 연관관계를 책임지는 관심사를 책임질 무언가를 추가하자.

애플리케이션의 전체 동작 방식을 구성하기 위해 구현 객체를 생성하고 연결하는 책임을 가지는 별도의 설정 클래스를 만들자.

* AppConfig

```java
public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService() {
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
```

AppConfig의 관심사 필요한 구현 객체를 생성한다.

MemberServiceImpl, MemoryMemberRepository, OrderServiceImpl, FixDiscountPolicy

AppConfig의 관심사 생성한 구현 객체를 생성자를 통해 주입해준다. 연관관계를 책임진다.

MemberServiceImpl -> MemoryMemberRepository OrderServiceImpl -> MemoryMemberRepository, Fix/DiscountPolicy

AppCOnfig가 잘 수행되도록 구현체들에 생성자를 추가하자.

* MemberServiceImpl.java - 생성자 주입

```java
public class MemberServiceImpl implements MemberService {

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

이제 더 이상 MemberServiceImpl은 인터페이스 MemoryMemberRepository에 의존하지 않는다.

어떤 구현 객체가 memberRepository에 주입될지는 MemberServiceImple의 관심사가 아니다.

어떤 구현 객체를 주입할지는 외부(AppConfig)의 관심사다.

![img_33](https://user-images.githubusercontent.com/79847020/156158272-0971d98d-2e0d-4ac3-93f9-9b873c67314f.png)

DIP 완성 : 추상적인 것에 의존하고 구체적인 것에 의존하지 마라.

관심사의 분리 : 객체를 생성하고 연결하는 역할과 실행하는 역할이 분리되었다.

![img_35](https://user-images.githubusercontent.com/79847020/156158283-874e6327-ef98-4e55-8a8c-50f6b5a5d42b.png)

appConfig 객체는 memoryMemberRepository 객체를 생성하고 참조값을 memberServiceImple 생성자에게 전달한다.

클라이언트 memberServiceImpl 입장에서 보면 의존관계를 마치 외부에서 주입해주는 것 같다고해서 DI(Dependency Injection) 우리말로 의존관계 주입 또는 의존성 주입이라 한다.

* OrderServiceImpl - 생성자 주입

```java
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }


    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);

        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

* 사용 클래스 - MemberApp

```java
public class MemberApp {
    public static void main(String[] args) {
        AppConfig appConfig = new AppConfig();
        MemberService memberService = appConfig.memberService();

        Member memberA = new Member(1L, "memberA", Grade.VIP);
        memberService.join(memberA);

        Member findMember = memberService.findMember(1L);
        System.out.println("new member = " + memberA.getName());
        System.out.println("find member = " + findMember.getName());
    }
}
```

* 사용 클래스 - OrderApp

```java
public class OrderApp {
    public static void main(String[] args) {
        AppConfig appConfig = new AppConfig();
        MemberService memberService = appConfig.memberService();
        OrderService orderService = appConfig.orderService();

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "ItemA", 10000);

        System.out.println("order = " + order);
    }
}
```

* 테스트 코드 오류 수정 - MemberServiceTest

```java
public class MemberServiceTest {

    MemberService memberService;

    @BeforeEach
    public void before() {
        AppConfig appConfig = new AppConfig();
        this.memberService = appConfig.memberService();
    }
    ...
}
```

OrderServiceTest

```java
class OrderServiceTest {

    MemberService memberService;
    OrderService orderService;

    @BeforeEach
    public void before() {
        AppConfig appConfg = new AppConfig();
        this.memberService = appConfg.memberService();
        this.orderService = appConfg.orderService();
    }

    @Test
    public void createOrder() throws Exception {
        //given
        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);

        //when
        memberService.join(member);
        Order order = orderService.createOrder(memberId, "ItemA", 10000);

        //then
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}
```

## 3.4. AppConfig 리팩터링

현재 AppConfig를 보면 중복이 있고, 역할에 따른 구현이 잘 안보인다.

![img_36](https://user-images.githubusercontent.com/79847020/156158293-cd46b455-c552-4c75-b3f6-278caccf0b2b.png)

* 변경 전 AppConfig

```java
public class AppConfig {
    public class AppConfig {
        public MemberService memberService() {
            return new MemberServiceImpl(new MemoryMemberRepository());
        }

        public OrderService orderService() {
            return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
        }
    }
}
```

중복을 제거하고 역할에 따른 구현이 보이도록 리팩터링 하자.

* 리팩터링 후 AppConfig

```java
public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public DiscountPolicy discountPolicy() {
        return new FixDiscountPolicy();
    }
}
```

new MemoryMemberRepository() 중복이 제거되었다. MemoryMemberRepository를 DBMemberRepository로 변경할 때 한군데서만 변경하면 된다.

AppConfig를 보면 역할과 구현 클래스가 한 눈에 들어온다.

## 3.5. 새로운 구조와 할인 정책 적용

정액 할인을 정률 할인으로 변경해보자. 어떤 부분을 변경해야할까 ?

AppConfig의 등장으로 애플리케이션이 크게 사용 영역, 생성하고 구성하는 영역으로 분리되었다.

![img_37](https://user-images.githubusercontent.com/79847020/156158311-0cdb7c48-e9ff-4261-afcc-eb116098663f.png)

![img_38](https://user-images.githubusercontent.com/79847020/156158318-bc090722-e0bb-459c-8bbf-3f8dcf66156c.png)

FixDiscountPolicy를 RateDiscountPolicy로 변경해도 AppCOnfig에서만 변경하면 된다.

* RateDiscountPolicy 변경 AppConfig

```java
public class AppConfig {
    ...생략.....
    public DiscountPolicy discountPolicy() {
        //return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

## 3.6 전체 흐름 정리

1. 새로운 할인 정책 개발
  * 다형성 덕분에 새로운 정률 할인 정책 코드를 추가 개발하는 것 자체는 아무 문제 없음.
2. 새로운 할인 정책 적용과 문제점
  * 정률 할인 정책 적용시 클라이언트 코드에 구현체를 생성하는 코드를 변경해야한다. 이 말은 클라이언트 코드가 인터페이스인 DiscountPolicy뿐만 아니라 구현 클래스인 FixDiscountPolicy도
    함께 의존한다는 말이고 이는 DIP 위반이다.
3. 관리사의 분리
  * 역할이 인터페이스이고 배우가 구현 객체면 역할에 배우를 배정하는 공연 기획자가 있어야 한다.
  * 프로그래밍적으로는 구현 객체를 생성하고 연결관계를 맺어주는 관심사를 담당할 무언가가 필요하다.
  * AppConfig 클래스가 그 관심사를 담당한다.
  * AppConfig로 인해 클라이언트 객체는 실행하는 관심사에만 집중할 수 있다.
4. AppConfig 리팩터링
  * 중복을 제거하고 역할에 따른 구현 부분을 명확하게 분리한다.
5. 새로운 구조와 할인 정책 적용
  * 클라이언트 객체를 수정하지 않고 AppConfig에서 구현 객체를 바꿈으로서 정률 할인 정책을 적용할 수 있다.

* 좋은 객체 지향 설계의 5가지 원칙의 적용
  * SRP 단일 책임 원칙
    * 기존 클라이언트 객체는 구현 객체를 생성하고 연결하고 실행하는 다양한 책임을 가지고 있었다.
    * 클라이언트 객체는 실행하는 책임만, AppConfig가 구현 객체를 생성하고 연결하는 책임을 담당
  * DIP 의존관계 역전 원칙
    * 추상화에 의존해야지 구체화에 의존하면 안된다.
    * 기존 클라이언트 객체는 인터페이스 뿐만 아니라 구현 객체에도 의존했었다.
  * OCP 개방폐쇠 원칙
    * 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
    * 소프트웨어 요소를 새롭게 확장해도 사용 영역의 변경은 닫혀 있다.

* IoC, DI, 그리고 컨테이너
  * 제어의 역전 (Inversion of Control)
    * 기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고 연결하고 실행했다.
    * AppConfig가 등장한 이후 구현 객체는 자신의 로직을 실행하는 역할만 담당한다. 생성하고 연결하는 관심사는 AppConfig에 넘겨졌다. 프로그램에 대한 제어 흐름에 대한 권한은 모두
      AppConfig가 가지고 있다. 심지어 OrderServiceImpl도 AppConfig가 생성한다. AppConfig는 OrderServiceImpl이 아닌 OrderService인터페이싀 다른 구현
      객체를 생성하고 실행할 수도 있다. 클라이언트 코드 OrderServiceImpl은 자신의 로직을 실행할 뿐이다.
    * 이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것은 제어의 역전이라 한다.

* 프레임워크 vs 라이브러리
  * 프레임워크가 내가 작성한 코드를 제어하고 대신 실행하면 그것은 프레임워크가 맞다. (Junit)
  * 반면에 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 프레임워크가 아니라 라이브러리다.

* 의존관계 주입(DI : Dependency Injection)
  * OrderServiceImpl은 DiscountPolicy 인터페이스에 의존한다. 실제 어떤 구현 객체가 사용될지는 모른다.
  * 의존관계는 정적인 클래스 의존 관계와 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계 둘을 분리해서 생각해야 한다.
  ![img_39](https://user-images.githubusercontent.com/79847020/156158341-323728cd-4c2e-41d4-9aa3-e772815dffdc.png)
  * OrderServiceImpl은 MemberRepository, DiscountPolicy에 의존한다는 것을 알 수 있다. 그러나 이런 클래스 의존관계 만으로 어떤 객체가 OrderServiceImpl에 주입
    될지 알 수 없다.
  * 애플리케이션 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 의존관계 주입이라 한다.
  * 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고 동적인 객체 인스턴스 관계를 쉽게 변경할 수 있다.


* IoC컨테이너, DI컨테이너
  * AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해주는 것을 IoC컨테이너 또는 DI컨테이너라고 한다.

# 3.7 스프링으로 전환하기

지금까지 순수한 자바 코드만으로 DI를 적용했다. 이제 스프링을 사용해보자.

* AppConfig 스프링 기반으로 변경

```java
@Configuration
public class AppConfig {
    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public DiscountPolicy discountPolicy() {
//        return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
}
```

@Configuration - 스프링 설정 파일임을 지정한다. @Bean - 스프링 빈으로 등록한다.

* MemberApp에 스프링 컨테이너 적용

```java
public class MemberApp {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);

        Member memberA = new Member(1L, "memberA", Grade.VIP);
        memberService.join(memberA);

        Member findMember = memberService.findMember(1L);
        System.out.println("new member = " + memberA.getName());
        System.out.println("find member = " + findMember.getName());
    }
}
```

* OrderApp에 스프링 컨테이너 적용

```java
public class OrderApp {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "ItemA", 20000);

        System.out.println("order = " + order);
    }
}
```

* 스프링 컨테이너
  * ApplicationContext를 스프링 컨테이너라고 한다.
  * 스프링 컨테이너는 @Configuration이 붙은 클래스를 설정(구성)정보로 사용한다. @Bean이라 적힌 메소드를 호출해서 반환한 객체를 스프링 컨테이너에 등록한다. 스프링 컨테이너에 등록된 객체를 스프링
    빈이라 한다.
  * 스프링 빈ㅇ은 @Bean이 붙은 메서드 명을 빈의 이름으로 사용한다.
  * applicationContext.getBean() 메소드를 사용해서 등록 된 빈을 얻을 수 있다.
  * AppConfig 같이 직접 자바코드로 작성했다면 이제는 스프링 컨테이너에 객체를 빈으로 등록하고 스프링 빈에서 객체를 찾아서 사용하도록 변경한다.

# 4. 스프링 컨테이너와 스프링 빈

### 스프링 컨테이너 설정

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
```

ApplicationContext를 스프링 컨테이너라고 한다. ApplicationContext는 인터페이스이다. ApplicationContext의 XML, 애노테이션 기반 Java파일을 설정(구성)정보로 사용할
수 있다.

* 스프링 컨테이너의 생성 과정
  * 스프링 컨테이너 생성
    ![img_40](https://user-images.githubusercontent.com/79847020/156158369-32a6b514-67c6-468e-b09f-267c8d6e207a.png)
  * 스프링 빈 등록
    ![img_41](https://user-images.githubusercontent.com/79847020/156158397-860bc6db-f295-431e-8682-c1bd66cd0ef5.png)

  빈 이름으로 기본적으로 메서드명이 사용되며, @Bean(name="") 다음과 같이 직접 지정할 수도 있다. 빈 이름은 고유해야 한다. 같은 이름의 빈이 있다면 덮어쓰거나 오류가 발생한다.
  * 스프링 빈 의존관계 설정 준비
    ![img_42](https://user-images.githubusercontent.com/79847020/156158410-23a3a54c-819b-40be-a921-249dad378ce4.png)
  * 스프링 빈 의존관계 설정 완료
    ![img_43](https://user-images.githubusercontent.com/79847020/156158418-87d565bc-b705-4f80-b767-809d126c7577.png)

### 컨테이너에 등록된 모든 빈 조회

```java
  class ApplicationContextInfoTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("모든 빈 출력하기")
    void findAllBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("name = " + beanDefinitionName + " obejct = " + bean);
        }
    }

    @Test
    @DisplayName("애플리케이션 빈 출력하기")
    void findApplicationBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionsName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionsName);
            //Role ROLE_APPLICATION : 직접 등록한 애플리케이션 빈
            //Role ROLE_INFRASTRUCTURE : 스프링이 내부에서 사용하는 빈
            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                Object bean = ac.getBean(beanDefinitionsName);
                System.out.println("name = " + beanDefinitionsName + " object = " + bean);
            }
        }
    }
}
```

* 모든 빈 출력하기
  * applicationContext.getBeanDefinitionNames() : ApplicationContext에 등록 된 모든 빈 이름들을 얻는다.
  * applicationContext.getBean() : 빈 이름으로 빈 객체(인스턴스)를 얻는다.
* 애플리케이션 빈 출력하기
  * ApplicationContext에 등록된 빈은 2가지 Role로 구분된다.
    * ROLE_APPLICATION : 사용자가 정의한 빈
    * ROLE_INFRASTRUCTURE : 스프링이 내부에서 사용하는 빈

### 스프링 빈 조회 - 기본

*
* 스프링 빈 조회 기본
  * 스프링 컨테이너에서 스프링 빈을 가장 기본 방법
    * applicationContext.getBean(빈이름, 타입);
    * applicationContext.getBean(타입)
    * 빈이 없으면 NoSuchBeanDefinitionException

```java
class ApplicationContextBasicFindTest {

    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("빈 이름으로 조회")
    void findBeanByName() {
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    @Test
    @DisplayName("이름 없이 타입만으로 조회")
    void findBeanByType() {
        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    @Test
    @DisplayName("구체 타입으로 조회")
    void findBeanByType2() {
        MemberService memberService = ac.getBean("memberService", MemberServiceImpl.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    @Test
    @DisplayName("빈 이름으로 조회 X")
    void findBeanByTypeX() {
        assertThrows(NoSuchBeanDefinitionException.class, () -> ac.getBean("xx", MemberService.class));
    }
}
```

### 스프링 빈 조회 - 동일한 타입이 둘 이상

타입으로 조회시 같은 타입의 스프링 빈이 둘 이상이면 오류가 발생한다. 이때는 빈 이름을 지정하자. applicationContext.getBeansOfType()을 사용하면 해당 타입의 모든 빈을 조회할 수 있다.

```java
class ApplicationContextSameBeanFindTest {

    ApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);

    static class SameBeanConfig {
        @Bean
        public MemberRepository memberRepository1() {
            return new MemoryMemberRepository();
        }

        @Bean
        public MemberRepository memberRepository2() {
            return new MemoryMemberRepository();
        }
    }

    @Test
    @DisplayName("타입으로 조회시 같은 타입이 둘 이상 있으면, 중복 오류가 발생한다.")
    void findBeanByTypeDuplicate() {
        assertThrows(NoUniqueBeanDefinitionException.class, () ->
                ac.getBean(MemberRepository.class)
        );
    }

    @Test
    @DisplayName("타입으로 조회시 같은 타입이 둘 이상 있으면, 빈 이름을 지정하면 된다.")
    void findBeanByTypeName() {
        MemberRepository memberRepository = ac.getBean("memberRepository1", MemberRepository.class);
        assertThat(memberRepository).isInstanceOf(MemberRepository.class);
    }

    @Test
    @DisplayName("특정 타입을 모두 조회하기")
    void findAllBeanByType() {
        Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value = " + beansOfType.get(key));
        }
        System.out.println("beansOfType = " + beansOfType);
        assertThat(beansOfType).hasSize(2);
    }
}
```

### 스프링 빈 조회 - 상속 관계

부모 타입으로 조회하면 자식 타입도 함께 조회한다. Object.class 타입으로 빈을 조회하면 모든 스프링 빈이 조회된다.

![img_44](https://user-images.githubusercontent.com/79847020/156158430-4c254a76-31d9-4b94-8f79-9b796b92682d.png)

```java
public class ApplicationContextExtendsFindTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);

    @Configuration
    static class TestConfig {
        @Bean
        public DiscountPolicy fixDiscountPolicy() {
            return new FixDiscountPolicy();
        }

        @Bean
        public DiscountPolicy rateDiscountPolicy() {
            return new RateDiscountPolicy();
        }
    }

    @Test
    @DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 중복 오류가 발생한다")
    void findBeanByParentTypeDuplicate() {
        assertThrows(NoUniqueBeanDefinitionException.class, () ->
                ac.getBean(DiscountPolicy.class)
        );
    }

    @Test
    @DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 빈 이름을 지정하면 된다.")
    void findBeanByParentTypeBeanName() {
        DiscountPolicy rateDiscountPolicy = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
        assertThat(rateDiscountPolicy).isInstanceOf(RateDiscountPolicy.class);
    }

    @Test
    @DisplayName("특정 하위 타입으로 조회")
    void findBeanBySubType() {
        RateDiscountPolicy bean = ac.getBean(RateDiscountPolicy.class);
        assertThat(bean).isInstanceOf(RateDiscountPolicy.class);
    }

    @Test
    @DisplayName("부모 타입으로 모두 조회하기")
    void findAllBeanByParentType() {
        Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
        assertThat(beansOfType.size()).isEqualTo(2);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value=" + beansOfType.get(key));
        }
    }

    @Test
    @DisplayName("부모 타입으로 모두 조회하기 - Object")
    void findAllBeanByObjectType() {
        Map<String, Object> beansOfType = ac.getBeansOfType(Object.class);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value=" + beansOfType.get(key));
        }
    }
}
```

### BeanFactory와 ApplicationContext

![img_45](https://user-images.githubusercontent.com/79847020/156158446-8050c9b4-7a6c-41f2-bdc9-a3c10f0a07cb.png)
* BeanFactory
  * 스프링 컨테이너의 최상위 인터페이스이다.
  * 스프링 빈을 관리하고 조회하는 역할을 담당한다.

* ApplicationContext
  * BeanFactory 기능을 모두 상속받아서 제공한다.
  * ApplicationContext을 BeanFactory 기능 이외에도 부가기능을 제공한다.
  * BeanFactory, MessageSource, EnvironmentCapable, ApplicationContextEventPublisher, ResourceLoader

![img_46](https://user-images.githubusercontent.com/79847020/156158459-9dda0127-401d-4700-ab92-87c0145c42f9.png)

ApplicationContext는 BeanFactory의 기능을 상속받는다. ApplicationContext는 BeanFactory의 기능 이외에 부가기능을 추가로 더 제공한다. BeanFactory를 직접
사용할 일은 없다. ApplicationContext를 사용한다. BeanFacotry나 ApplicationContext를 스프링 컨테이너라고한다.

### 다양한 설정 형식 지원 - 자바 코드, XML

![img_47](https://user-images.githubusercontent.com/79847020/156158471-b67302a8-a7d1-47c3-b4a2-924008b0d208.png)

AnnotationConfigApplicationContext - 애노테이션 기반 자바 설정 파일 사용 GenericXmlApplicationContext - XML 파일 설정 파일 사용

https://spring.io/projects/spring-framework

### 스프링 빈 설정 메타 정보 BeanDefinition

스프링은 어떻게 다양한 구성(설정)파일을 어떻게 지원하는 것일까?

BeanDefinition을 추상화하여 지원한다. XML 파일을 파싱해서 BeanDefinition을 생성한다. Java 설정 파일을 파싱해서 BeanDefinition을 생성한다.
ApplicationContext는 생성된 BeanDefinition만 읽으면 된다.

각 ApplicationContext별 파싱을 담당있는 BeanDefinitionReader가 존재한다.

BeanDefinition을 빈 설정 메타 정보라고 한다. @Bean, <Bean> 당 하나의 BeanDefinition이 생성된다.

BeanDefiniton을 직접 생성해서 사용할 수도 있지만 실무에서 직접 사용할 일은 없다. 스프링이 다양한 형태의 설정 정보를 BeanDefinition으로 추ㅏㅇ화해서 사용하는 것 정도만 이해하면 된다. 스프링
코드나 오픈 소스의 코드를 볼 때 BeanDefinition 을 사용하는 것을 볼 수 있다.

![img_48](https://user-images.githubusercontent.com/79847020/156158482-5289df49-5041-4496-9274-b4637d660767.png)

```java
public class BeanDefinitionTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("빈 설정 메타정보확인")
    void findApplicationBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                        " beanDefinition = " + beanDefinition);
            }
        }
    }
}
```

# 5. 싱글톤 컨테이너

### 웹 애플리케이션과 싱글톤

* 스프링은 태생이 기업용 온라인 서비스 기술을 지원하기 위해 탄생했고 주로, 웹 애플리케이션을 개발하기 위해 사용되고 있다.
* 웹 애플리케이션은 보통 여러 고객이 동시에 요청을 한다.

스프링이 없는 순수한 DI 컨테이너
![img_49](https://user-images.githubusercontent.com/79847020/156158500-6683f175-bea6-4138-816d-8e2bfb06dde4.png)

```java
public class SingletonTest {

    @Test
    @DisplayName("스프링 없는 순수한 DI 컨테이너")
    void pureContainer() {
        AppConfig appConfig = new AppConfig();
        //1. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService1 = appConfig.memberService();

        //2. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService2 = appConfig.memberService();

        //참조값이 다른 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        //memberService1 != memberService2
        assertThat(memberService1).isNotSameAs(memberService2);
    }
}
```

우리가 기존에 만들었던 AppConfig 사용할 때는 요청 할 때마다 새로 객체를 생성한다. 초당 100개의 요청이 오면 100개의 객체가 생성하고 소멸된다. 메모리 낭비가 심하고 비용이 너무크다. 이를 해결하기
위해 싱글톤 패턴으로 객체를 한개만 생성한다.

### 싱글톤 패턴

클래스의 인스턴스가 애플리케이션 내에서 1개만 생성되는 것을 보장하는 디자인 패턴이다. private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 막아야한다.

```java
public class SingletonService {
    private static final SingletonService instance = new SingletonService();

    public static SingletonService getInstance() {
        return instance;
    }

    private SingletonService() {
    }

    public void logic() {
        System.out.println("싱글톤 객체 로직 호출");
    }
}
```

1. 클래스 내부에 static 영역에 객체 instance를 미리 하나 생성해서 올려둔다.
2. 이 객체 인스턴스가 필요하면 오직 getInstance() 메서드를 통해서만 참조 할 수 있다.
3. 애플리케이션 내에서 1개의 객체 인스턴스만 존재해야 하므로, 생성자를 private으로 막아서혹시라도 외부에서 new 키워드로 객체 인스턴스가 생성되는것을 막는다.

```java
    @Test
    @DisplayName("싱글톤 패턴을 적용한 객체 사용")
    void singletonServiceTest() {
        SingletonService singletonService1 = SingletonService.getInstance();
        SingletonService singletonService2 = SingletonService.getInstance();

        System.out.println("singletonService1 = " + singletonService1);
        System.out.println("singletonService2 = " + singletonService2);

        assertThat(singletonService1).isSameAs(singletonService2);
    }
```

싱글톤 패턴을 구현하는 방법은 여러가지가 있는데 여기서는 가장 단순한 방법을 소개했다.

### 싱글톤 패턴은 안티패턴이다.

* 싱글톤 패턴 문제점
  * 싱글톤 패턴을 구현하기 위한 코드가 많이 들어간다.
  * 의존관계상 클라이언트가 구현 클래스를 의존할 수 밖에 없다. DIP 위반
  * 구현 클래스를 의존하므로 OCP 위반을 할 가능성이 높다.
  * 테스트하기 어렵다.
    * 싱글톤은 생성 방식이 제한적이기 때문에 Mock 객체로 대체하기가 어려우며, 동적으로 객체를 주입하기도 힘들다.
  * 내부 속성을 변경하거나 초기화 하기 어렵다.
  * Private 생성자로 자식 클래스를 만들기 어렵다.
  * 유연성이 떨어진다.
  * 안티패턴으로 불린다.

### 싱글톤 컨테이너로서 스프링 컨테이너

스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서 객체 인스턴스를 싱글톤으로 관리한다. 스프링 빈이 바로 싱글톤으로 관리되는 Bean이다.

스프링 컨테이너는 싱글톤 컨테이너 역할을 하며, 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라 한다.

스프링 컨테이너는 이런 기능으로 싱글톤 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지할 수 있다.

* 싱글톤 패턴을 위해 지저분한 코드가 들어가지 않아도 된다.
* DIP, OCP, 테스트, private 생성자로부터 자유롭게 싱글톤을 사용할 수 있다.

스프링 컨테이너를 사용하는 테스트 코드

```java
    @Test
    @DisplayName("스프링 컨테이너와 싱글톤")
    void springContainer() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        //1. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService1 = ac.getBean("memberService", MemberService.class);

        //2. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService2 = ac.getBean("memberService", MemberService.class);

        //참조값이 다른 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        //memberService1 != memberService2
        assertThat(memberService1).isSameAs(memberService2);
    }
```

싱글톤 컨테이너 적용 후

![img_50](https://user-images.githubusercontent.com/79847020/156158515-29e9eaa8-e632-4eb9-8223-6bb45cca36b2.png)

참고 : 스프링의 기본 빈 등록 방식은 싱글톤이지만 싱글톤 방식만 지원하는 것은 아니다. 요청할 때마다 새로운 객체를 생성해서 반환하는 기능도 제공한다.

싱글톤 방식의 주의점

싱글톤 패턴, 싱글톤 컨테이너를 사용하든 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 상태유지(StateFul)하게 설계하면 안된다. 무상태(
Stateless)로 설계해야 한다.

* 특정 클라이언트에 의존적인 필드가 있으면 안된다.
* 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
* 필드 대신 자바에서 공유하지 않는 지역 변수, 파라미터, ThreadLocal 등을 사용해야 한다.

상태를 유지하는 경우 문제점 예시

```java
public class StatefulService {

    private int price; // 상태를 유지하는 필드

    public void order(String name, int price) {
        System.out.println("name = " + name + " price = " + price);
        this.price = price; //여기가 문제!
    }

    public int order2(String name, int price) {
        System.out.println("name = " + name + " price = " + price);
        return price; //여기가 문제!
    }

    public int getPrice() {
        return price;
    }
}

public class StatefulServiceTest {

    static class TestConfig {
        @Bean
        public StatefulService statefulService() {
            return new StatefulService();
        }
    }

    @Test
    void statefulServiceSingleton() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
        StatefulService statefulService1 = ac.getBean(StatefulService.class);
        StatefulService statefulService2 = ac.getBean(StatefulService.class);

        //ThreadA: A사용자 10000원 주문
        statefulService1.order("userA", 10000);
        //ThreadB: B사용자 20000원 주문
        statefulService2.order("userA", 20000);

        //ThreadA: 사용자A 주문 금액 조회
        int price = statefulService1.getPrice();

        System.out.println("price = " + price);

        StatefulService statefulService3 = ac.getBean(StatefulService.class);
        StatefulService statefulService4 = ac.getBean(StatefulService.class);

        //Stateless 지역변수활용
        System.out.println("price = " + statefulService3.order2("userA", 10000));
        System.out.println("price = " + statefulService3.order2("userB", 20000));
    }
}
```

### @Configuration과 싱글톤

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberRepository memberRepository() {
        System.out.println("call AppConfig.memberRepository");
        return new MemoryMemberRepository();
    }

    @Bean
    public DiscountPolicy discountPolicy() {
        return new RateDiscountPolicy();
    }

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
}
```

memeberService 빈을 만드는 코드를 보면 memberRepository()를 호출한다. 그리고 new MemoryMemberRepository()를 호출한다.

orderService 빈을 만드는 코드를 보면 memberRepository()를 호출한다. 그리고 new MemoryMemberRepository()를 호출한다.

결과적으로 각각 다른 2개의 MemoryMemberRepository가 생성되면서 싱글톤이 깨지는 것처럼 보인다. 스프링 컨테이넌 이 문제를 어떻게 해결할까?

```java
@Component
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository;

    //테스트 용도
    public MemberRepository getMemberRepository() {
        return memberRepository;
    }
}

public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;

    //테스트 용도
    public MemberRepository getMemberRepository() {
        return memberRepository;
    }
}
```

실제로 MemberServiceImpl과 OrderServiceImpl에 주입되는 memberRepository를 확인해보자.

```java
public class ConfigurationSingletonTest {

    @Test
    void configurationTest() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        //
        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository = " + memberRepository1);
        System.out.println("orderService -> memberRepsoitory = " + memberRepository2);
        System.out.println("memberRepsoitory = " + memberRepository);

        assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);
        assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
    }

    @Test
    void configurationDeep() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        AppConfig bean = ac.getBean(AppConfig.class);

        System.out.println("bean = " + bean.getClass());
    }
}
```

확인해보면 memberRepository 인스턴스는 모두 같은 인스턴스가 공유되어 사용된다.

AppConfig 자바 코드에는 new MemoryMemberRepository가 분명 여러번 호출된다. AppConfig에서 객체를 생성하는 부분에서 로그를 출력해서 확인해보자.

```java
@Configuration
public class AppConfig {

//    public MemberService memberService() {
//        return new MemberServiceImpl(new MemoryMemberRepository());
//    }
//
//    public OrderService orderService() {
//        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
//    }

    @Bean
    public MemberRepository memberRepository() {
        System.out.println("call AppConfig.memberRepository");
        return new MemoryMemberRepository();
    }

    @Bean
    public DiscountPolicy discountPolicy() {
        return new RateDiscountPolicy();
    }

    @Bean
    public MemberService memberService() {
        System.out.println("call AppConfig.memberService");
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderService orderService() {
        System.out.println("call AppConfig.orderService");
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
}
```

스프링 컨테이너가 @Bean을 호출해서 스프링 빈을 생성한다. memberRepository()는 3번 호출되어야 하는 것 아닐까?

1. 스프링 컨테이너가 스프링 빈으로 등록하기 위해 memberRepository() 호출
2. memberService() 로직에서 memberRepository() 호출
3. orderService() 로직에서 memberRepository() 호출

하지만 결과는 한번만 호출된다.

```
call AppConfig.memberService
call AppConfig.memberRepository
call AppConfig.orderService
```

### @Configuration과 바이트코드 조작의 마법

스프링 컨테이너는 싱글톤 레지스트리다. 스프링 빈이 싱글톤임을 보장한다. 그런데 스프링이 자바 코드를 조작하는 것은 힘들다. 자바 코드 상으로는 3번 호출되어야 하는 것이 맞다. 그래서 스프링은 클래스의 바이트코드를
조작하는 라이브러리를 사용한다.

```java

    @Test
    void configurationDeep() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        
        //AppConfig도 스프링 빈으로 등록한다.
        AppConfig bean = ac.getBean(AppConfig.class);

        //출력 : bean = class.hello.core.AppConfig$$EnhancerBySpringCGLIB$$bd479d70
        System.out.println("bean = " + bean.getClass());
    }
```

AnnotationConfigApplicationContext에 파라미터로 넘긴 AppConfig는 스프링 빈으로 등록된다. AppConfig의 클래스 정보를 출력해보자.

```
bean = class.hello.core.AppConfig$$EnhancerBySpringCGLIB$$bd479d70
```

순수한 클래스 라면 다음과 같이 출력된다.

```
class hello.core.AppConfig
```

이것이 의미하는 것은 스프링 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고 그 다른 클래스를 스프링 빈으로 등록한 것이다.

![img_51](https://user-images.githubusercontent.com/79847020/156158527-1d85ecc7-87eb-4b35-b189-a5cf1898c5a3.png)

CGLIB 라이브러리를 사용해서 해당 클래스를 싱글톤으로 생성한다.

* AppConfig@CGLIB 예상(상상) 코드

```java
    @Bean
    public MemberRepository memberRepository() {
        if (memoryMemberRepository가 이미 스프링 컨테이너에 등록되어 있으면 ?){
            return 스프링 컨테이너에서 찾아서 반환;
        } else{ //스프링 컨테이너에 없으면
            기존 로직을 호출해서 MemoryMemberRepository를 생성하고 스프링 컨테이너에 등록 return 반환
        }
    }
```

@Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다. 덕분에 싱글톤이 보장되는 것이다.

### @Configuration을 적용하지 않고 @Bean만 적용하면 어떻게 될까?

@Configuration을 붙이면 바이트코드를 조작하는 CGLIB 기술을 사용해서 싱글톤을 보장하지만 만약 @Bean만 적용하면 어떻게 될까?

```java
//@Configuration
public class AppConfig {
    ...
}
```

```
bean = class.hello.core.AppConfig
```

AppConfig가 CGLIB 기술 없이 순수한 AppConfig로 스프링 빈에 등록된 것을 확인할 수 있다.

그리고 MemberRepository가 총 3번 호출된 것도 확인할 수 있다.

```
call AppConfig.memberService
call AppConfig.memberRepository
call AppConfig.orderService
call AppConfig.memberRepository
call AppConfig.memberRepository
```

각 인스턴스를 출력해보면

```
memberService -> memberRepository = hello.core.member.MemoryMemberRepository@6239aba6 
orderService -> memberRepository = hello.core.member.MemoryMemberRepository@3e6104fc
memberRepository = hello.core.member.MemoryMemberRepository@12359a82
```

당연히 인스턴스 또한 같지 않다.

정리하면 @Bean만 사용해도 스프링 빈으로 등록되지만 싱글톤을 보장하지 않는다. 스프링 설정정보는 항상 @Configuration을 사용하자.

# 6. 컴포넌트 스캔

### 컴포넌트 스캔과 의존관계 자동 주입 시작하기

지금까지 스프링 빈을 등록할 때는 자바코드의 @Bean이나 XML의 <bean> 등을 통해서 설정 정보에 직접 등록할 빈을 나열했다.

스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공한다.

또한 자동으로 의존관계 주입하는 @Autowired라는 기능도 제공한다.

AutoAppConfig.java

```java

@Configuration
@ComponentScan(
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
)
public class AutoAppConfig {
}
```

@ComponentScan : 컴포넌트 스캔을 사용하기 위한 애노테이션, 이름 그대로 @Component 애노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록한다.

참고 : 컴포넌트 스캔을 사용하면 @Configuration이 붙은 설정 정보도 자동으로 등록되기 때문에 AppConfig의 설정보도 등록되고 실행된다. 그래서 excludeFilters 옵션을 사용해서
@Configuration는 스캔 대상에서 제외했다. @Configuration이 컴포넌트 대상이 되는 것은 @Configuration 애노테이션 선언부에 @Component 애노테이션도 적용이 되어 있기 때문이다.

각 클래스에 @Component 애노테이션을 적용하자. 그리고 @Autowired 또한 적용하자.

```java
@Component
public class MemoryMemberRepository implements MemberRepository {}

@Component
public class RateDiscountPolicy implements DiscountPolicy {}

@Component
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository; 
    
    @Autowired
    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository; 
    }
    ...
}

@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    ...
}
```

AnnotationConfigApplicationContext로 AutoAppConifg를 설정파일로 ApplicationContext를 생성한다.

AutoAppConfigTest

```java
public class AutoAppConfigTest {
    @Test
    void basicScan() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);

        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```

AutoAppConfigTest를 하면 MemberService 빈을 잘 가져올 수 있는 것을 확인할 수 있다.

다음과 같이 컴포넌트 스캔해서 빈으로 등록되는 과정이 로그에 찍힌다.

![img_52](https://user-images.githubusercontent.com/79847020/156158532-0888518a-16dd-4fba-8f2c-b6d4aaf26464.png)

컴포넌트 스캔과 자동 의존관계 주입이 어떻게 동작하는지 살펴보자.

![img_53](https://user-images.githubusercontent.com/79847020/156158542-4ad3cd06-589f-4287-9157-170c2ab1c276.png)

@ComponentScan은 @Component가 붙은 모든 클래스를 스프링 빈으로 등록한다. 이 때 스프링 빈의 기본 이름은 클래스명을 사용하되 맨 앞글자만 소문자를 사용한다. @Compoennt("
beanName") 빈 이름을 직접 지정할수도 있다.

![img_54](https://user-images.githubusercontent.com/79847020/156158558-3af0b163-59cb-4ed7-b042-1e8995525187.png)

생성자에 @Autowired를 지정하면 스프링 컨테이너 해당 빈을 찾아서 주입한다. 기본 전략은 타입이 같은 빈을 찾아서 주입한다. getBean(MemberRepository.class)와 같다고 생각하면 된다.

### 컴포넌트 스캔 탐색 위치

탐색 위치를 지정할 수 있다.

```java
@ComponentScan(
        basePackages = "hello.core"
)
```

basePackages : 탐색할 패키지의 시작 위치를 지정한다. 이 패키지를 포함해서 하위 패키지를 모두 탐색한다. basePackages = {"hello.core", "hello.service"} 이렇게 여러
시작 위치를 지정할 수도 있다.

basePackageClasses : 지정한 클래스의 패키지를 탐색 시작 위치로 지정한다.

지정하지 않으면 @ComponentScan 애노테이션이 적용된 클래스의 패키지가 시작위치가 된다.

<u>권장하는 방법 : 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것이다.</u>

예를 들면

com.hello, com.hello.service, com.hello.repository 같은 패키지 구조가 있다면 com.hello에 AppConfig같은 설정 정보 클래스를 두고 @ComponentScan
애노테이션을 적용한다.

이렇게 하면 com.hello 패키지를 포함한 하위 패키지 모두 자동으로 컴포넌트 스캔 대상이 된다. 메인 설정 정보는 프로젝트를 전체를 총괄하기 때문에 프로젝트 시작 루트 위치에 두는 것이 자연스럽다.

참고로 스프링 부트를 사용하면 스프링 부트의 대표 시작 정보인 @SpringBootApplication를 이 프로젝트 시작 루트 위치에 두는 것이 관례이다. 그리고 @SpringBootApplication
애노테이션안에 @ComponentScan이 적용되어 있다.

### 컴포넌트 스캔 대상

* 컴포넌트 스캔의 기본 대상
  * @Component : 컴포넌트 스캔 대상 지정
  * @Controller : 스프링 MVC 컨트롤러에서 사용
  * @Service : 스프링 비즈니스 로직에서 사용
  * @Repository : 스프링 데이터 접근 계층에서 사용
  * @Configuration : 스프링 설정 정보에서 사용

각 애노테이션이 컴포넌트 스캔 대상이 될 수 있는 원리는 각 애노테이션 선언부에 @Component를 포함하고 있기 때문이다.

자바에서 애노테이션은 상손관계라는 것이 없다. 애노테이션에 특정 애노테이션이 적용되어 있다는 것을 인식할 수 있는 것은 자바가 지원하는 기능이 아니고 스프링이 지원하는 기능이다.

컴포넌트 스캔 용도 뿐만 아니라 스프링은 부가 기능을 제공한다.

* 애노테이션 부가 기능
  * @Controller : 스프링 MVC 컨트롤러로 인식
  * @Repository : 스프링 데이터 접근 계층으로 인식하고 데이터 계층의 예외를 스프링 예외로 변환해준다.
  * @Configuration : 스프링 설정 정보로 인식하고 스프링 빈이 싱글톤을 유지하도록 추가 처리한다.
  * @Service : 부가 기능은 없지만 개발자들이 핵심 비즈니스 로직이라는 것을 인식하는데 도움을 준다.

* 옵션
  * userDefaultFilters : 기본으로 켜져있는데 앞서 말한 기본 컴포넌트 스캔 대상을 스캔할 것인지 정한다.
  * includeFilters : 컴포넌트 스캔 대상을 추가로 지정한다.
  * excludeFitlers : 컴포넌트 스캔 대상을 제외시킨다.

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyIncludeComponent {
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyExcludeComponent {
}

@MyIncludeComponent
public class BeanA {
}

@MyExcludeComponent
public class BeanB {
}

public class ComponentFilterAppConfigTest {
    @Configuration
    @ComponentScan(includeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
            excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
    )
    static class ComponentFilterAppConfig {}
    
    @Test
    void filterScan() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(ComponentFilterAppConfig.class);
        BeanA beanA = ac.getBean(BeanA.class);
        assertThat(beanA).isNotNull();

        Assertions.assertThrows(NoSuchBeanDefinitionException.class,
                () -> {
                    BeanB beanb = ac.getBean(BeanB.class);
                }
        );
    }
}
```

### FilterType 옵션

* ANNOTATION : 기본값, 애노테이션을 인식해서 동작한다.
  * ex) org.exmaple.SomeAnnotation
* ASSIGNABLE_TYPE : 지정한 타입과 자식 타입을 인식해서 동작한다.
  * ex) org.example.SomeClass
* ASPECTJ : AspectJ 패턴 사용
  * ex) org.example.*Service*
* REGEX : 정규표현식
  * ex) org\.example\.Default.*
* CUSTOM : TypeFilter 이라는 인터페이스를 구현해서 처리
  * ex) org.example.MyTypeFilter

예를 들어서 BeanA도 빼고 싶으면 다음과 같이 추가하면 된다.

```java
@ComponentScan(
        includeFilters = {
            @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class)},
        excludeFilters ={
            @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class),
            @Filter(type = FilterType.ASSIGNABLE_TYPE, classes = BeanA.class)
        }
)
```

참고로 includeFiltersm excludeFilters를 사용하기 보다는 스프링 부트 컴포넌트 스캔 기본 옵션으로 사용하는 것을 권장한다.

### 중복 등록과 충돌

1. 자동 빈 등록 vs 자동 빈 등록
2. 수동 빈 등록 vs 자동 빈 등록

* 자동 빈 등록 vs 자동 빈 등록 컴포넌트 스캔에 의해 자동으로 스프링 빈이 등록될 때 이름이 중복되는 경우 ConfilictingBeanDefinitionException 예외가 발생한다.

* 수동 빈 등록 vs 수동 빈 등록

```java
@Component
public class MemoryMemberRepository implements MemberRepository {}

@Configuration
@ComponentScan(
        excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Configuration.class))
public class AutoAppConfig {
    @Bean(name - "memoryMemberRepository")
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

이 경우 수동 빈 등록이 우선권을 가진다. 오버라이딩 된다. 다음과 같은 로그가 출력된다.

```java
Overriding bean definition for bean 'memoryMemberRepository' with a different definition: replacing
```

사실 개발자가 의도적으로 이런 스프링이 오버라이딩 되는 상황을 만들이유는 없다. 따라서, 최근 스프링부트에서는 오류가 발생하도록 기본 값을 바꾸었다.

```java
Consider renaming one of the beans of enabling overriding by setting spring.main.allow-bean-definition-overriding=true
```

# 의존곤계 자동 주입

### 다양한 의존관계 주입 방법

의존관계 주입은 크게 4가지 방법이 있다.

1. 생성자 주입
2. 수정자 주입
3. 필드 주입
4. 일반 메서드 주입

* 생성자 주입

이름 그대로 생성자를 통해서 의존관계를 주입 받는 방법이다.

생성자는 호출시점에 딱 1번만 호출되는 것이 보장된다. 따라서 불변, 필수 의존관계에 사용한다.

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    ...
}
```

중요한 것은 생성자가 1개만 있으면 @Autowired를 생략해도 자동 주입된다. 물론 스프링 빈에만 해당한다.

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    ...
}
```

* 수정자 주입

setter라고 부르는 필드의 값을 변경하는 수정자 메서드를 통해서 의존관계를 주입하는 방법이다.

setter는 언제든 호출할 수 있기 때문에 선택, 변경 가능성이 있는 의존관계에서 주로 사용한다. 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법이다.

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
```

@Autowired의 기본 동작은 주입할 대상이 없으면 오류가 발생한다. 주입할 대상이 없어도 동작하게 하려면 @Autowired(required = false)로 지정하면 된다.

자바빈 프로퍼티, 자바에서는 과거부터 필드의 값을 직접 변경하지 않고 setXX, getXXX 라는 메서드를 통해서 값을 읽거나 수정하는 규칙을 만들었는데 그것이 자바빈 프로퍼티 규약이다.

```java
class Data {
    private int age;
    public void setAge(int age) {
        this.age = age;
    }
    
    public int getAge() {
        return age;
    }
}
```

* 필드 주입 - 사용하지 말자!!!!

이름 그대로 필드에 바로 주입하는 방법이다. 코드가 간결해서 개발자들이 사용하기 좋지만, 외부에서 변경이 불가능해서 테스트 하기 힘들다는 치명적인 단점이다. DI 프레임워크가 없으면 아무것도 할 수 없다.

사용하지 말자 !!

1. 애플리케이션의 실제 코드와 관계 없는 테스트 코드
2. 스프링 설정 목적으로 하는 @Configuration 같은 곳에서만 특별한 용도로 사용

```java
@Component
public class OrderServiceImpl implements OrderService {
    @Autowired
    private MemberRepository memberRepository;

    @Autowired
    private DiscountPolicy discountPolicy;
```

순수한 자바 테스트 코드에는 당연히 @Autowired가 동작하지 않는다. @SpringBootTest 처럼 스프링 컨테이너를 테스트에 통합한 경우에만 가능하다.

```java
@Bean
OrderService orderService(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    return new OrderServiceImpl(memberRepository, discountPolicy);
        }
```

@Bean에서 파라미터에 에 의존관계는 자동 주입된다. 수동 등록시 자동 등록된 빈의 의존관계가 필요할 때 문제를 해결할 수 있다.

* 일반 메서드 주입

일반 메서드를 통해 주입한다. 일반적으로 잘 사용하지 않는다.

```java
@Component
//@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작한다. 일반 클래스에서 @Autowired 코드를 적용해도 아무 기능도 동작하지 않는다.

### 자동 주입 옵션 처리

주입할 스프링 빈이 없어도 동작해야 할 때가 있다. 그런데 @Autowired는 required 옵션이 true로 되어 있기 때문에 자동 주입 대상이 없으면 오류가 발생한다.

자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.

1. Autowired(required=false)
2. org.springframework.lang.@Nullable 자동 주입할 대상이 없으면 null이 주입된다.
3. Optional<> 자동 주입할 대상이 없으면 Optional.empty가 입력된다.

```java
    
    //호출 안됨
    @Autowired(required = false)
    public void setNoBean1(Member member) {
        System.out.println("setNoBean1 = " + member);
    }

    //null 호출
    @Autowired
    public void setNoBean2(@Nullalbe Member member) {
        System.out.println("setNoBean2 = " + member);
    }

    //Optional.empty 호출
    @Autowired(required = false)
    public void setNoBean3(Optional<Member> member) {
        System.out.println("setNoBean3 = " + member);
    }
```

Member타입의 등록된 빈이 없다면 setNoBean1은 호출되지 않는다.

출력결과

```java
setNoBean2 = null;
setNoBean3 = Optional.empty
```

### 생성자 주입을 선택해라

과거에는 수정자 주입과 필드 주입을 많이 사용했지만, 최근에는 스프링을 포함한 DI 프레임워크 대부분이 생성자 주입을 권장한다. 그 이유는 다음과 같다.

불변

* 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다. 오히려 대부분 의존관계는 애플리케이션 종료 전까지 변하면 안된다.(불변해야 한다.)
* 수정자 주입을 사용하면 setXXX메서드를 public으로 열어두어야 한다.
* 누군가 실수로 변경할 수도 있고 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방법이 아니다.
* 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없다. 따라서 불변하게 설계할 수 있다.

누락

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
    ...
```

프레임워크 없이 순수한 자바 코드를 단위 테스트에 하는 경우 다음과 같이 수정자 의존관계인 경우 @Autowired가 프레임워크 안에서 동작할 때는 의존관계가 없으면 오류가 발생하지만 지금은 프레임워크 없이 순수한
자바 코드로만 단위 테스트를 수행하고 있다.

이렇게 테스트를 수행하면 실행은 된다.

```java
  @Test
  void createOrder() {
      OrderServiceImpl orderService = new OrderServiceImpl();
      orderService.createOrder(1L, "itemA", 10000);
  }
```

막상 실행하면 결과는 NPE(Null Pointer Exception)이 발생하는데 memberRepository, discountPolicy 모두 의존관계 주입이 누락되었기 때문이다.

생성자 주입을 사용하면 다음처럼 주입 데이터를 누락 했을 때 컴파일 오류가 발생한다. 그리고 IDE에서 바로 어떤 값을 필수로 주입해야 하는지 알 수 있다.

생성자 주입을 사용하면 필드에 final 키워드를 사용할 수 있다. 그래서 생성자에서 혹시라도 같이 설정되지 않는 오류를 컴파일 시점에 막아준다.

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        //this.discountPolicy = discountPolicy;
    }
    ...
```

잘 보면 필수 필드인 discountPolicy에 값을 설정해야 하는데 이 부분이 누락되었다. 자바는 컴파일 시점에 다음 오류를 발생시킨다.

```
java : variable discountPolicy might not have been initializaed
```

<u>컴파일 오류는 세상에서 가장 빠르고 좋은 오류다.</u>

정리

생성자 주입 방식을 선택하는 이유는 여러가지가 있지만 프레임워크에 의존하지 않고 순수한 자바 언어의 특성을 잘 살리는 방법이기도 하다. 기본으로 생성자 주입을 사용하고 필수 값이 아닌 경우에는 수정자 주입 방식을
옵션으로 부여하면 된다. 생성자 주입과 수정자 주입을 동시에 사용할 수 있다. 항상 생성자 주입을 선택해라. 그리고 옵션이 필요하면 수정자 주입을 선택해라. 필드 주입은 사용하지마라.

### 롬복과 최신 트랜드

막상 개발을 해보면 대부분이 다 불변이고 생성자에 final 키워드를 사용하게 된다. 그런데 생성자도 만들어야 하고, 주입 받은 값을 대입하는 코드도 만들어야 하고 필드 주입처럼 좀 편리하게 사용하는 방법은 없을까
역시나 개발자는 편리한 것을 찾는다.

생성자가 딱 한개만 있으면 @Autowired를 생략할 수 있다.

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    ...
```

롬복 라이브러리가 제공하는 @RequiredArgsConstructor 기능을 사용하면 final이 붙은 필드를 모아서 생성자를 자동으로 만들어준다. 코드에서 생성자가 보이지 않지만 실제 호출이 가능하다.

다음과 같이 코드를 작성할 수 있다.

```java
@Component
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

```

롬복이 자바의 애노테이션 프로세서라는 기능을 이용해서 컴파일 시점에 생성자 코드를 자동으로 생성해준다. 실제 class를 열어보면 다음 생성자가 추가되어 있는 것을 확인할 수 있다.

정리

최근에는 생성자를 1개만 두고 @Autowired를 생략하는 방법을 주로 사용한다. 여기에 Lombok라이브러리의 @RequiredArgsConstructor 함께 사용하면 기능을 다 제공하면서 코드는 깔끔하게
사용할 수 있다.

### 롬복 라이브러리 적용 방법

build.grald 설정 추가

```
//lombok 설정 추가 시작
configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}
//lombok 설정 추가 끝

    //lombok 라이브러리 추가 시작
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'

    testCompileOnly 'org.projectlombok:lombok'
    testAnnotationProcessor 'org.projectlombok:lombok'
    //lombok 라이브러리 추가 끝
```

Preference(윈도우 File > Setting) -> plugin -> lombok 검색 설치 Preference(윈도우 File > Setting) -> Annotation Processors 검색 ->
Enable Annotation Processing 체크 임의의 테스트 클래스 만들어 @Getter, @Setter 테스트

### 조회 빈이 2개 이상 - 문제

```java
@Autowired
private Discount discountPolicy;
```

@Autowired는 타입으로 빈을 조회한다. 타입으로 조회하기 때문에 다음 코드와 유사하게 동작한다.

```java
applicationContext.getBean(DiscountPolicy.class)
```

타입으로 조회하면 선택된 빈이 2개 이상일 때 문제가 발생한다. DiscountPolicy 타입 빈을 다음과 같이 등록하자.

```java
@Component
public class FixDiscountPolicy implements DiscountPolicy {}

@Component
public class RateDiscountPolicy implements DiscountPolicy {}
```

그리고 다음과 같이 의존관계 자동 주입을 실행하면

```java
@Autowired    
private final DiscountPolicy discountPolicy;
```

NoUniqueBeanDefinitionException 오류를 발생한다.

```java
NoUniqueBeanDefinitionException : No qualifying bean of type 'hello.core'discount.DiscountPolicy' available : expected single matching bean but found 2 : fixDiscountPolicy, rateDiscoutPoliy
```

오류메시지가 친절하게도 하나의 빈을 기대했는데 'fixDiscountPolicy', 'rateDiscoutPolicy' 2개가 발견되었다고 알려준다.

### @Autowired 필드명, @Qualifier, @Primary

해결 방법을 하나씩 알아보자.

조회 대상 빈이 2개 이상일 때 해결 방법

1. @Autowired 필드 명 매칭
2. @Qualifier -> @Qualifier 끼리 매칭 -> 빈 이름 매칭
3. @Primary 사용

* @Autowired 필드 명 매칭

@Autowired는 타입 매칭을 시도하고 이때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭한다.

기존 코드

```java
@Autowired    
private final DiscountPolicy discountPolicy;
```

필드 명을 빈 이름으로 변경

```java
@Autowired    
private final DiscountPolicy rateDiscountPolicy;
```

필드 명이 rateDiscountPolicy이므로 정상 주입된다. 필드 명 매칭은 먼저 타입 매칭을 시도 하고 그 결과에 여러 빈이 있을 때 추가로 동작하는 기능이다.

@Autowired 정리

1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드 명, 파라미터 명으로 빈 이름 매칭

* @Qualifier 사용 @Qualifier는 추가 구분자를 붙여주는 방법이다. 주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.

```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {}

@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {}
```

주입시에 @Qualifier를 붙여주고 등록한 이름을 적어준다.

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```

수정자 자동 주입 예시

```java
//    @Autowired
//    public void setDiscountPolicy(@Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
//        this.discountPolicy = discountPolicy;
//    }
```

@Qualifier로 주입할 때 @Qualifier("mainDiscountPolicy")를 못찾으면 어떻게 될까? 그러면 mainDiscountPolicy라는 이름의 스프링 빈을 추가로 찾는다. 하지만 경험상
@Qualifier는 @Qualifier를 찾는 용도로만 사용하는게 명확하고 좋다.

@Qualifier 정리

1. @Qualifier끼리 매칭
2. 빈 이름 매칭
3. NoSuchBeanDefinitionException 예외 발생

@Primary 사용

@Primary는 우선순위를 정하는 방법이다. @Autowired 시에 여러 빈이 매칭되면 @Primary가 우선권을 가진다.

rateDiscountPolicy가 우선권을 가지도록 하자

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {}

@Component
public class FixDiscountPolicy implements DiscountPolicy {}
```

사용코드

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}

@Autowired
public DiscountPolicy setDiscountPolicy(DiscountPolicy discountPolicy) {
    return discountPolicy;
}
```

@Primary이 적용된 RateDiscountPolicy이 DiscountPolicy 타입에 의존성 주입되는 것을 알 수 있다.

그럼 @Primary와 @Qualifier 중에 어떤 것을 사용하면 좋을까 ? @Qualifer의 단점은 주입 받을 때 다음과 같이 모든 코드에 @Qualifer를 붙여주어야 한다느 점이다.

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
        }        
```

반면에 @Primary를 사용하면 이렇게 주입 대상에 애노테이션을 적용할 필요가 없다.

* @Primary, @Qualifier 활용

코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 획득하는 스프링 빈이 있고 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스의 커넥션을 획득하는 스프링 빈이 있따고 생각해보자. 메인 데이터베이스의 커넥션을
획득하는 스프링 빈은 @Primary를 적용해서 조회하는 곳에서 @Qualifier를 지정없이 편리하게 조회하고 서브 데이터베이스 커넥션 빈을 획득할 때는 @Qualifier를 지정해서 명시적으로 획득 하는 방식으로
사용하면 코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈을 등록할 때 @Qualifier를 지정해주는 것은 상관없다.

* 우선순위

@Primary는 기본값 처럼 동작하는 것이고 @Qualifier는 매우 상세하게 동작한다. 이런 경우 어떤 것이 우선권을 가져갈까? 스프링은 자동보다는 수동이 넓은 범위의 선택권 보다는 좁은 범위의 선택권이
우선순위가 높다 여기서도 @Qualifier가 우선권이 높다.

### 애노테이션 직접 만들기

@Qualifier("mainDiscountPolicy") 이렇게 문자를 적으면 컴파일시 타입 체크가 안된다. 다음과 같은 애노테이션을 만들어서 문제를 해결할 수 있다.

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
@Qualifier("mainDiscountPolicy")
public @interface MainDiscountPolicy {
}
```

```java
@Component
@MainDiscountPolicy
public class RateDiscountPolicy implements DiscountPolicy {}
```

```java
    //생성자
    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, @MainDiscountPolicy DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

```java
    //setter
    @Autowired
    public void setDiscountPolicy(@MainDiscountPolicy DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
```

애노테이션에는 상속이라는 개념이 없다. 여러 애노테이션을 모아서 사용하는 기능은 스프링이 지원해주는 기능이다. @Qualifier 뿐만 아니라 다른 애노테이션들도 함께 조합해서 사용할 수 있다. 단적으로
@Autowired도 재정의 할 수 있다. 물론 스프링이 제공하는 기능을 뚜렷한 목적없이 무분별하게 재정의 하는 것은 유지보수에 더 혼란만 가눙할 수 있다.

### 조회한 빈이 모두 필요할 때, List, Map

의도적으로 해당 타입의 스프링 빈이 다 필요한 경우도 있다. 예를 들어서 할인 서비스를 제공하는데 클라이언트가 할인의 종류(rate, fix)를 선택할 수 있다고 가정해보자. 스프링을 사용하면 소위 말하는 전략
패턴을 매우 간단하게 구현할 수 있다.

```java
public class AllBeanTest {

    @Test
    void finalAllBean() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(
                AutoAppConfig.class, DiscountService.class
        );
        DiscountService discountService = ac.getBean(DiscountService.class);
        Member member = new Member(1L, "userA", Grade.VIP);

        int discountPrice = discountService.discount(member, 10000, "fixDiscountPolicy");

        assertThat(discountService).isInstanceOf(DiscountService.class);
        assertThat(discountPrice).isEqualTo(1000);

        int rateDiscountPrice = discountService.discount(member, 20000, "rateDiscountPolicy");
        assertThat(rateDiscountPrice).isEqualTo(2000);
    }

    static class DiscountService {
        private final Map<String, DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policies;

        public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
            this.policyMap = policyMap;
            this.policies = policies;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policies = " + policies);
        }

        public int discount(Member member, int price, String discountCode) {
            DiscountPolicy discountPolicy = policyMap.get(discountCode);

            System.out.println("discountCode = " + discountCode);
            System.out.println("discountPolicy = " + discountPolicy);

            return discountPolicy.discount(member, price);
        }
    }
}
```

로직 분석

* DiscountService는 Map으로 만든 DiscountPolicy를 주입받는다. 이 때 fixDiscountPolicy, rateDiscountPolicy가 주입된다.
* discount() 메서드는 discountCode로 fixDiscountPolicy가 넘어오면 map에서 fixDiscountPolicy 스프링 빈을 찾아서 실행한다. 물론 "rateDiscountPolicy"
  가 넘어오면 rateDiscountPolicy 스프링 빈을 찾아서 실행한다.

주입 분석

* Map<String, DiscoutPolicy> : map의 키에 스프링 빈의 이름을 넣어주고 그 값으로 DiscountPolicy 타입으로 조회한 모든 스프링 빈을 담아준다.
* List<DiscountPolicy> : DiscountPolicy 타입으로 조회한 모든 스프링 빈을 담아준다. 만약 해당하는 타입의 스프링 빈이 없으면 빈 컬렉션이나 Map을 주입한다.

* 스프링 컨테이너를 생성하면서 스프링 빈 등록하기

스프링 컨테이너는 생성자에 클래스 정보를 받는다. 여기에 클래스 정보를 넘기면 해당 클래스가 스프링 빈으로 자동 등록된다.

```java
new AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class)
```

이 코드는 2가지로 나누어 이해할 수 있다.

* new AnnotationConfigApplicationContext()를 통해 스프링 컨테이너를 생성한다.
* AutoAppConfig.class, DiscountService.class를 파라미터로 넘기면서 해당 클래스를 자동으로 스프링 빈으로 등록한다.
* 정리하면 스프링 컨테이너를 생성하면서, 해당 컨테이너에 동시에 AutoAppConfig, DiscountService를 스프링 빈으로 등록한다.

### 자동, 수동의 올바른 실무 운영 기준

그러면 어떤 경우 컴포넌트 스캔과 자동 주입을 사용하고 어떤 경우에 설정 정보를 통해서 수동으로 빈을 등록하고 의존관계도 수동으로 주입해야 할까?

* 편리한 자동 기능을 기본으로 사용하자.

결론부터 이야기하면 스프링이 나오고 시간이 갈 수록 점점 자동을 선호하는 추세다. 스프링은 @Component 뿐만 아니라 @Controller, @Service, @Repository 처럼 계층에 맞추어 일반적인
애플리케이션 로직을 자동으로 스캔할 수 있도록 지원한다. 거기에 더해서 최근 스프링 부트는 컴포넌트 스캔을 기본으로 사용하고 스프링 부트의 다양한 스프링 빈들도 조건이 맞으면 자동으로 동작하도록 설계했다.

설정 정보를 기반으로 애플리케이션을 구성하는 부분과 실제 동작하는 부분을 명확하게 나누느 것이 이상적이지만 개발자 입장에서 스프링 빈을 하나 등록할 때 @Component만 넣어주면 끝나는 일을
@Configuration 설정 정보에 가서 @Bean을 적고 객체를 생성하고 주입할 대상을 일일이 넣어주는 과정은 상당이 번거롭다.

또 관리할 빈이 많아서 설정 정보가 커지면 설정 정보를 관리하는 것 자체가 큰 부담이 된다. 그리고 결정적으로 자동 빈 등록을 사용해도 OCP, DIP를 지킬 수 있다.

* 그러면 수동 빈 등록은 언제 사용하면 좋을까?

애플리케이션은 크게 업무 로직과 기술 지원 로직으로 나눌 수 있다.

업무 로직 빈 : 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리 등이 모두 업무 로직이다. 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.

기술 지원 빈 : 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. 데이터 베이스 연결이나 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술들이다.

업무 로직을 숫자도 매우 많고 한번 개발해야 하면 컨트롤러, 서비스, 리포지토리 처럼 어느정도 유사한 패턴이 있다. 이런 경우 자동 기능을 적극 사용하는 것이 좋다. 보통 문제가 발생해도 어떤 곳에서 문제가
발생했는지 명확하게 파악하기 쉽다.

기술 지원 로직은 업무 로직과 비교해서 그 수가 매우 작고 보통 애플리케이션 전반에 걸쳐서 광범위하게 영향을 미친다. 그리고 업무 로직은 문제가 발생했을 때 어디가 문제인지 명확하게 잘 들어나지만 기술 지원 로직은
적용이 잘 되고 있는지 아닌지 조차 파악하기 어려운 경우가 많다. 그래서 이런 기술 지원 로직들은 가급적 수동 빈 등록을 사용해서 명확하게 들어내는 것이 좋다.

* 애플리케이션에 고아범위하게 영향을 미치는 기술 지원 객체는 수동 빈으로 등록해서 딱 설정 정보에 바로 나타나게 하는 것이 유지 보수하기 좋다.

* 비즈니스 로직 중에 다형성을 적극 활용할 때

조회한 빈이 모두 필요할 때 List, Map을 사용하는 경우를 살펴보자. DiscountService 의존관계 자동 주입으로 Map<Stringm DiscountPolicy>에 주입을 받는 상황을 생각해보자.
여기에 어떤 빈들이 주입될지, 각 빈들의 이름은 무엇일지 ㅗ드만 보고 한번에 쉽게 파악할 수 있을까? 다른 개발자가 개발해서 나에게 준 것이라면 자동 등록을 사용하고 있기 때문에 파악하려면 여러 코드를 찾아봐야
한다.

이런 경우 수동 빈으로 등록하거나 또는 자동으로하면 특정 패키지에 같이 묶어두는게 좋다. 핵심은 딱 보고 이해가 되어야 한다.

이 부분을 별도의 설정 정보로 만들고 수동으로 등록하면 다음과 같다.

```java
@Configuration
public class DiscountPolicyConfig {
    @Bean
    public DiscountPolicy rateDiscountPolicy() {
        return new RateDiscountPolicy();
    }
    
    @Bean
    public DiscountPolicy fixDiscountPolicy() {
        return new FixDiscountPolicy();
    }
}
```

이 설정 정보만 봐도 한눈에 빈의 이름은 물론이고 어떤 빈들이 주입될지 파악할 수 있다. 그래도 빈 자동 등록을 사용하고 싶으면 파악하기 좋게 DiscountPolicy의 구현 빈들만 따로 모아서 특정 패키지에
모아두자.

참고로 스프링과 스프링 부트가 자동으로 등록하는 수 많은 빈들은 예외다. 이런 부분들은 스프링 자체를 잘 이해하고 스프링의 의도대로 잘 사용하는게 중요하다. 스프링 부트의 경우 DataSource 같은 데이터 베이스
연결에 사용하는 기술 지원 로직까지 내부에서 자동으로 등록하는데 이런 부분은 메뉴얼을 잘 참고해서 스프링 부트가 의도한 대로 편리하게 사용하면 된다. 반면에 스프링 부트가 아니라 내가 직접 기술 지원 객체를 스프링
빈으로 등록한다면 수동으로 등록해서 명화하게 하는 것이 좋다.

정리 편리한 자동 기능을 기본으로 사용하자. 직접 등록하는 기술 지원 객체는 수동 등록하자. 다형성을 적극 활용하는 비즈니스 로직은 수동 등록을 고민해보자.

# 8. 빈 생명주기 콜백

### 빈 생명주기 콜백 시작

데이터베이스 커넥션 풀이나 네트워크 소켓처럼 애플리케이션 시작 시점에 필요한 연결을 미리 해두고 애플리케이션 종료 시점에 연결을 모두 종료하는 작업을 진행하려면 객체의 초기화와 종료 작업이 필요하다.

스프링을 통해 이런 초기화 작업과 종료 작업을 어떻게 진행하는지 예쩨로 알아보자.

간단하게 외부 네트워크에 미리 연결하는 객체를 하나 생성한다고 가정해보자. 실제로 네트워크에 연결하는 것은 아니고 단순히 문자만 출력하도록 했다. 이 NetworkClient는 애플리케이션 시작 시점에
connection()를 호출해서 연결을 맺어두어야 하고 애플리케이션 종료되면 disConnection()를 호출해서 연결을 끊어야 한다.

```java
public class NetworkClient {

    private String url;

    public NetworkClient() {
        System.out.println("생성자 호출, url = " + url);
        connect();
        call("초기화 연결 메시지");
    }

    public void setUrl(String url) {
        this.url = url;
    }

    //서비스 시작시 호출
    public void connect() {
        System.out.println("connect: " + url);
    }

    public void call(String message) {
        System.out.println("call: " + url + " message = " + message);
    }

    //서비스 종료시 호출
    public void disconnect() {
        System.out.println("close " + url);
    }
}
```

```java
public class BeaeLifeCycleTest {
    @Test
    public void lifeCycleTest() {
        ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        NetworkClient client = ac.getBean(NetworkClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig {
        @Bean
        public NetworkClient networkClient() {
            NetworkClient networkClient = new NetworkClient();
            networkClient.setUrl("http://hello.spring.dev");
            return networkClient;
        }
    }
}
```

실행해보면 다음과 같음 메시지가 나온다.

```java
생성자 호출, url = null
connect: null
call: null message = 초기화 연결 메시지
```

생성자 부분을 보면 url 정보 없이 connect가 호출되는 것은 확인할 수 있다. 너무 당연한 이야기이지만 객체를 새엇ㅇ하는 단계에서 url이 없고 객체를 생성한 다음에 외부에서 수정자 주입을 통해서
setUrl()이 호출되어야 url이 존재하게 된다.

스프링 빈은 간단하게 다음과 같은 라이프 사이클을 가진다.

객체생성 -> 의존관계 주입

스프링 빈은 객체를 생성하고 의존관계를 주입이 다 끝난 다음에야 필요한 데이터를 사용할 수 있는 준비가 완료된다. 따라서 초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출해야 한다. 그런데 의존관계 주입이
모두 완료된 시점을 어떻게 알 수 있을까 ?

스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해서 초기화 시점을 알려주는 다양한 기능을 제공한다. 또한 스프링은 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 준다. 따라서 안전하게 종료 작업을
진행할 수 있따.

* 스프링 빈의 이벤트 라이프 사이클

스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸전 콜백 -> 스프링 종료

초기화 콜백 : 빈이 생성되고 빈의 의존관계 주입이 완료된 후 호출 소멸전 콜백 : 빈이 소멸되기 직전에 호출

스프링은 다양한 방식으로 생명주기 콜백을 지원한다.

참고 : 객체의 생성과 초기화를 분리하자. 생성자는 필수 정보(파라미터)를 받고 메모리를 할당해서 객체를 생성하는 책임을 가진다. 반면에 초기화는 이렇게 생성된 값들을 활용해서 외부 커넥션을 연결하는등 무거운 동작을
수행한다. 따라서 생성자 안에서 무거운 초기화 작업을 함께 하는 것보다는 객체를 생성하는 부분과 초기화 하는 부분을 명확하게 나누는 것이 유지보수 관점에서 좋다. 물론 초기화 작업이 내부 값들만 약간 변경하는 정도로
단순한 경우에는 생성자에서 한번에 다 처리하는게 더 나을 수 있다.

참고 : 싱글톤 빈들은 스프링 컨테이너가 종료될 때 싱글톤 빈들도 함께 종료되기 때문에 스프링 컨테이너가 종료되기 직전에 소멸전 콜백이 일어난다. 뒤에서 설명하겠지만 싱글톤처럼 컨테이너의 시작과 종료까지 생존하는
빈도 있지만 생명주기가 짧은 빈들도 있는데 이 빈들은 컨테이너와 무관하게 해당 빈이 종료되기 직전에 소멸전 콜백이 일어난다.

스프링은 크게 3가지 방법으로 빈 생명주기 콜백을 지원한다.

1. 인터페이스(InitializingBean, DisposableBean)
2. 설정 정보에 초기화 메서드, 종료 메서드 지정
3. @PostConstruct, @PreDestory 애노테이션 지원

### 인터페이스 InitializingBean, DisposableBean

```java
public class NetworkClient implements InitializingBean, DisposableBean {

    private String url;

    public NetworkClient() {
        System.out.println("생성자 호출, url = " + url);
    }

    public void setUrl(String url) {
        this.url = url;
    }

    //서비스 시작시 호출
    public void connect() {
        System.out.println("connect: " + url);
    }

    public void call(String message) {
        System.out.println("call: " + url + " message = " + message);
    }

    //서비스 종료시 호출
    public void disconnect() {
        System.out.println("close " + url);
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        connect();
        call("초기화 연결 메시지");
    }

    @Override
    public void destroy() throws Exception {
        disconnect();
    }
}
```

* InitializingBean은 afterPropertiesSet() 메서드로 초기화를 지원한다.
* DisposableBean은 destroy() 메서드로 소멸을 지원한다.

```java
생성자 호출, url = null
connect: http://hello.spring.dev
call: http://hello.spring.dev message = 초기화 연결 메시지
19:04:17.952 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@47e2e487, started on Mon Feb 28 19:04:17 KST 2022
close http://hello.spring.dev
```

출력 결과를 보면 초기화 메서드가 주입 완료 후에 적절하게 호출 된 것을 확인할 수 있다. 그리고 스프링 컨테이너의 종료가 호출되자 소멸 메서드가 호출 된 것도 확인할 수 있다.

초기화 소멸 인터페이스 단점

1. 이 인터페이스는 스프링 전용 인터페이스다. 해당 코드가 스프링 전용 인터페이스에 의존한다.
2. 초기화, 소멸 메서드의 이름을 변경할 수 없다.
3. 내가 코드를 고칠 수 없는 외부 라이브러리에 적용할 수 없다.

4. 참고 : 인터페이스를 사용하는 초기화, 종료 방법은 스프링 초창기에 나온 방법들이고 지금은 다음의 더 나은 방법들이 있어서 거의 사용하지 않는다.

### 빈 등록 초기화, 소멸 메서드 지정

설정 정보에 @Bean(initMethod = "init", destroymethod = "close") 처럼 옵션을 주어 초기화, 소멸 메서드를 지정할 수 있다.

```java
public class BeaeLifeCycleTest {
    @Test
    public void lifeCycleTest() {
        ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        NetworkClient client = ac.getBean(NetworkClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig {
        @Bean(initMethod = "init", destroyMethod = "close")
        public NetworkClient networkClient() {
            NetworkClient networkClient = new NetworkClient();
            networkClient.setUrl("http://hello.spring.dev");
            return networkClient;
        }
    }
}
```

```java
public class NetworkClient {

    private String url;

    public NetworkClient() {
        System.out.println("생성자 호출, url = " + url);
    }

    public void setUrl(String url) {
        this.url = url;
    }

    //서비스 시작시 호출
    public void connect() {
        System.out.println("connect: " + url);
    }

    public void call(String message) {
        System.out.println("call: " + url + " message = " + message);
    }

    //서비스 종료시 호출
    public void disconnect() {
        System.out.println("close " + url);
    }

    public void init() throws Exception {
        System.out.println("NetworkClient.init");
        connect();
        call("초기화 연결 메시지");
    }
    
    public void close() throws Exception {
        System.out.println("NetworkClient.close");
        disconnect();
    }

}
```

설정 정보 사용 특징

1. 메서드 이름을 자유롭게 줄 수 있다.
2. 스프링 빈이 스프링 코드에 의존하지 않는다.
3. 코드가 아니라 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있다.

종료 메서드 추론

1. @Bean의 destroyMethod 속성에는 아주 특별한 기능이 있다.
2. 라이브러리는 대부분 close, shutdown 이라는 이름의 종료 메소드를 사용한다.
3. @Bean의 destoryMethod는 기본값이 (inferred)(추론)으로 등록되어 있다.
4. 이 추론 기능은 close, shutdown 라는 이름의 메서드를 자동으로 호출해준다. 이름 그대로 종료 메서드를 추론해서 호출해준다.
5. 따라서 직접 스프링 빈으로 등록하면 종료 메서드는 따로 적어주지 않아도 잘 동작한다.
6. 추론 기능을 사용하기 싫으면 destoryMethod="" 처럼 빈 공백을 지정하면 된다.

### 애노테이션 @PostConstruct @PreDstory

```java
public class NetworkClient /* implements InitializingBean, DisposableBean */ {

    private String url;

    public NetworkClient() {
        System.out.println("생성자 호출, url = " + url);
    }

    public void setUrl(String url) {
        this.url = url;
    }

    //서비스 시작시 호출
    public void connect() {
        System.out.println("connect: " + url);
    }

    public void call(String message) {
        System.out.println("call: " + url + " message = " + message);
    }

    //서비스 종료시 호출
    public void disconnect() {
        System.out.println("close " + url);
    }

    @PostConstruct
    public void init() throws Exception {
        System.out.println("NetworkClient.init");
        connect();
        call("초기화 연결 메시지");
    }

    @PreDestroy
    public void close() throws Exception {
        System.out.println("NetworkClient.close");
        disconnect();
    }
}
```

```java
public class BeaeLifeCycleTest {
    @Test
    public void lifeCycleTest() {
        ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        NetworkClient client = ac.getBean(NetworkClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig {
        @Bean
        public NetworkClient networkClient() {
            NetworkClient networkClient = new NetworkClient();
            networkClient.setUrl("http://hello.spring.dev");
            return networkClient;
        }
    }
}
```

```
생성자 호출, url = null
NetworkClient.init
connect: http://hello.spring.dev
call: http://hello.spring.dev message = 초기화 연결 메시지
20:34:34.113 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@1b11171f, started on Mon Feb 28 20:34:33 KST 2022
NetworkClient.close
close http://hello.spring.dev
```

@PostConstruct, @PreDestroy 이 두 애노테이션을 사용하면 가장 편리하게 초기화와 종료를 실행할 수 있다.

@PostConstruct, @PreDestroy 애노테이션 특징

* 최신 스프링에서 가장 권장하는 방법이다.
* 애노테이션 하나만 붙이면 되므로 매우 편리하다.
* 패키지를 잘 보면 javax.annotation.PostConstruct 이다. 스프링에 종속적인 기술이 아니라 JSR-250라는 자바 표준이다. 따라서 스프링이 아닌 다른 컨테이너에서도 동작한다.
* 컴포넌트 스캔과 잘 어울린다.
* 유일한 단점은 외부 라이브러리에는 적응하지 못한다는 것이다. 외부 라이브러리를 초기화, 종료 해야 하면 @Bean의 기능을 사용하자.

정리

* @PostConstruct, @PreDestroy 애노테이션을 사용하자.
* 코드로 고칠 수 없는 외부 라이브러리를 초기화, 종료해야 하면 @Bean의 initMethod, destroyMethod를 사용하자.

# 9. 빈 스코프

### 빈 스코프란 ?

지그까지 우리는 스프링 빈이 스프링 컨테이너의 시작과 함께 생성되어서 스프링 컨테이너가 종료될 때까지 유지된다고 학습했다. 이것은 스프링 빈이 기본적으로 싱글톤 스코프로 생성되기 때문이다. 스코프는 번역 그대로 빈이
존재할 수 있는 범위를 뜻한다.

스프링은 다양한 스코프를 지원한다.

1. 싱글톤 : 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프
2. 프로토타입 : 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프이다.
3. 웹 관련 스코프
  1. request : 웹 요청이 들어오고 나갈때 까지 유지되는 스코프이다.
  2. session : 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프이다.
  3. application : 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프이다.

빈의 스코프는 다음과 같이 지정할 수 있다.

컴포넌트 스캔 자동 등록

```java
@Scope("prototype")
@Component
public class HelloBean {}
```

수동 등록

```java
@Scope("prototype")
@Bean
PrototypeBean HelloBean() {
    return new HelloBean();    
}
```

### 프로토타입 스코프

싱글톤 스코프의 빈을 조회하면 스프링 컨테이너는 항상 같은 인스턴스의 스프링 빈을 반환한다. 반면에 프로토타입 스코프를 스프링 컨테이너에 조회하면 스프링 컨테이너는 항상 새로운 인스턴스를 생성해서 반환한다.

싱글톤 빈 요청

![img_55](https://user-images.githubusercontent.com/79847020/156158578-8b253360-86a9-4526-a4a3-b080d256c7f2.png)

프로토 타입 빈 요청

![img_58](https://user-images.githubusercontent.com/79847020/156158611-7f502710-ba30-4e58-9263-c48fd988eb4e.png)

![img_57](https://user-images.githubusercontent.com/79847020/156158620-ef9b3e70-d22e-4532-b36b-a69ae421a330.png)

핵심은 스프링 컨테이너는 프로토타입 빈을 생성하고 의존관계 주입, 초기화까지만 처리한다는 것이다. 클라이언트에 빈을 반환하고 이후 스프링 컨테이너는 생성된 프로토타입 빈을 관리하지 않는다. 프로토타입 빈을 관리할
책임을 프로토타입 빈을 받은 클라이언트에 있다. 그래서 @PreDestory 같은 종료 메서드가 호출되지 않는다.

싱글톤 스코프 빈 테스트

```java
public class SingletonTest {
    @Test
    void singletonBeanFind() {
        ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);
        SingletonBean singletonBean1 = ac.getBean(SingletonBean.class);
        SingletonBean singletonBean2 = ac.getBean(SingletonBean.class);
        System.out.println("singletonBean1 = " + singletonBean1);
        System.out.println("singletonBean2 = " + singletonBean2);

        Assertions.assertThat(singletonBean1).isSameAs(singletonBean2);
        ac.close();
    }

    @Scope("singleton") //default
    static class SingletonBean {
        @PostConstruct
        public void init() {
            System.out.println("SingletonBean.init");
        }

        @PreDestroy
        public void destroy() {
            System.out.println("SingletonBean.destroy");
        }
    }
}
```

출력결과

```text
SingletonBean.init
singletonBean1 = hello.core.scope.SingletonTest$SingletonBean@4f0100a7
singletonBean2 = hello.core.scope.SingletonTest$SingletonBean@4f0100a7
12:35:39.356 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@51e69659, started on Tue Mar 01 12:35:38 KST 2022
SingletonBean.destroy
```

1. 빈 초기화 메서드를 실행하고
2. 같은 인스턴스의 빈을 조회하고
3. 종료 메서드까지 정상 호출 된 것을 확인할 수 있다.

프로토타입 스코프 빈 테스트

```java
public class PrototypeTest {
    @Test
    void prototypeBeanFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
        System.out.println("find prototypeBean1");
        PrototypeBean prototypeBean1 = ac.getBean(PrototypeBean.class);
        System.out.println("find prototypeBean2");
        PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class);

        System.out.println("prototypeBean1 = " + prototypeBean1);
        System.out.println("prototypeBean2 = " + prototypeBean2);
        assertThat(prototypeBean1).isNotSameAs(prototypeBean2);

        ac.close();
    }

    @Scope("prototype")
    static class PrototypeBean {
        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init");
        }

        @PreDestroy
        public void destroy() {
            System.out.println("PrototypeBean.destroy");
        }
    }
}
```

프로토타입 스코프의 빈을 조회하는 prototypeBeanFind() 테스트를 실행해보자.

실행결과

```java
find prototypeBean1
PrototypeBean.init
find prototypeBean2
PrototypeBean.init
prototypeBean1 = hello.core.scope.PrototypeTest$PrototypeBean@4f0100a7
prototypeBean2 = hello.core.scope.PrototypeTest$PrototypeBean@3cdf2c61
12:39:47.578 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@51e69659, started on Tue Mar 01 12:39:47 KST 2022
```

싱글톤 빈은 스프링 컨테이너 생성 시점에 메서드가 실행되지만 프로토타입 스코프의 빈은 스프링 컨테이너에서 빈을 조회할 때 생성되고 초기화 메서드도 실행된다.

프로토타입 빈을 2번 조회했으므로 완전히 다른 스프링 빈이 생성되고 초기화도 2번 실행된 것을 확인할 수 있다.

싱글톤 빈은 스프링 컨테이너가 관리하기 때문에 스프링 컨테이너가 종료될 때 빈의 종료 메서드가 실행되지만 프로토타입 빈은 스프링 컨테이너가 생성과 의존관계 주입 그리고 초기화까지만 관여하고 더는 관리하지 않는다.
따라서 프로토타입 빈은 스프링 컨테이너가 종료될 때 @PreDestory 같은 종료 메서드는 실행되지 않는다.

프로토타입 빈의 특징 정리

1. 스프링 컨테이너에 요청할 때마다 새로 생성된다.
2. 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입 그리고 초기화까지만 관여한다.
3. 종료 메서드가 호출되지 않는다.
4. 그래서 프로토타입 빈은 프로토타입 빈을 조회한 클라이언트가 관리해야 한다. 종료 메서드에 대한 호출도 클라이언트가 직접해야 한다.

### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 문제점

스프링 컨테이너에 프로토타입 스코프 빈의 요청하면 항상 새로운 객체 인스턴스를 생성해서 반환한다. 하지만 싱글톤 빈과 함께 사용할 때는 의도한 대로 잘 동작하지 않으므로 주의해야 한다. 그림과 코드로 설명하겠다.

먼저 컨테이너에 프로토타입 빈을 직접 요청하는 예제를 보자.

* 프로토타입 빈 직접 요청

![img_59](https://user-images.githubusercontent.com/79847020/156158628-2c18c4e2-1511-4c4c-95ff-a46450bbd4b8.png)

1. 클라이언트 A는 스프링 컨테이너에 프로토타입 빈을 요청한다.
2. 스프링 컨테이너는 프로토타입 빈을 새로 생성해서 반환(x01)한다. 해당 빈의 count 필드 값은 0이다.
3. 클라이언트는 조회한 프로토타입 빈에 addCount()를 호출하면서 count필드를 +1한다.
4. 결과적으로 프로토타입 빈(x01)의 count는 1이 된다.

![img_60](https://user-images.githubusercontent.com/79847020/156158637-eb64df74-bc7c-40f6-8914-16325b230b64.png)

1. 클라이언트B는 스프링 컨테이너에 프로토타입 빈을 요청한다.
2. 스프링 컨테이너는 프로토타입 빈을 새로 생성해서 반환(x02)한다. 해당 빈의 count필드는 0이다.
3. 클라이언트는 조회한 프로토타입 빈에 addCount()를 호출하면서 count필드를 +1한다.
4. 결과적으로 프로토타입 빈(x02)의 count는 1이된다.

코드

```java
public class SingletonWithPrototypeTest1 {

    @Test
    void prototypeFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
        PrototypeBean prototypeBean1 = ac.getBean(PrototypeBean.class);
        prototypeBean1.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);

        PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class);
        prototypeBean2.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);
    }

    @Scope("prototype")
    static class PrototypeBean {
        private int count = 0;

        public void addCount() {
            count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init " + this);
        }

        @PreDestroy
        public void destory() {

        }
    }
}
```

싱글톤 빈에서 프로토타입 빈 사용

이번에는 clientBean이라는 싱글톤 빈이 의존관계 주입을 통해서 프로토타입 빈을 주입받아서 사용하는 예를 보자.

싱글톤과 프로토타입 빈 사용1

![img_61](https://user-images.githubusercontent.com/79847020/156158655-7718f32a-c0b2-41ea-8373-19c1196ba8e6.png)

1. clientBean은 싱글톤이므로 보통 스프링 컨테이너 생성 시점에 함께 생성되고 의존관계 주입도 발생한다.
2. clientBean은 의존관계 자동 주입을 사용한다. 주입 시점에 스프링 컨테이너에 프로토타입 빈을 요청한다.
3. 스프링 컨테이너는 프로토타입 빈을 생성해서 clientBean에 반환한다. 프로토타입 빈의 count필드 값은 0이다.
4. 이제 clientBean은 프로토타입 빈을 내부 필드에 보관한다. (정확히는 참조값을 보관한다)

싱글톤과 프로토타입 빈 사용 2

![img_62](https://user-images.githubusercontent.com/79847020/156158662-2255918e-1179-45d3-9989-be7c44ace06f.png)

1. 클라이언트A는 clientBean을 스프링 컨테이너에 요청해서 받는다. 싱글톤이므로 항상 같은 clientBean이 반환된다.
2. 클라이언트A는 clientBean.logic()을 호출한다.
3. clientBean은 prototypeBean의 addCount()을 호출해서 프로토타입 빈의 count를 증가한다. count값이 1이 된다.

싱글톤에서 프로토타입 빈 사용 3

1. 클라이언트B는 clientBean을 스프링 컨테이너에 요청해서 받는다. 싱글톤이므로 항상 같은 clientBean이 반환된다.
2. 여기서 중요한 점이 있는데 clientBean이 내부에 가지고 있는 프로토타입 빈은 이미 과거에 주입이 끝난 빈이다. 주입 시점에 스프링 컨테이너에 요청해서 프로토타입 빈이 새로 생성이 된 것이지, 사용할
   때마다 새로 생성되는 것이 아니다.
3. 클라이언트B는 clientBean.logic()을 호출한다.
4. clientBean은 prototypeBean의 addCount()를 호출해서 프로토타입 빈의 count를 증가한다. 원래 count값이 1이었으므로 2가 된다.

테스트 코드

```java
public class SingletonWithPrototypeTest1 {

    @Test
    void prototypeFine() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
        PrototypeBean prototypeBean1 = ac.getBean(PrototypeBean.class);
        prototypeBean1.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);

        PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class);
        prototypeBean2.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);
    }

    @Test
    void singletonClientUserPrototype() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class, ClientBean.class);

        ClientBean clientBean1 = ac.getBean(ClientBean.class);
        int count1 = clientBean1.logic();
        assertThat(count1).isSameAs(1);

        ClientBean clientBean2 = ac.getBean(ClientBean.class);
        int count2 = clientBean2.logic();
        assertThat(count2).isSameAs(1);

        System.out.println(count1);
        System.out.println(count2);
    }

    @Scope("singleton")
    static class ClientBean {
        @Autowired
        private Provider<PrototypeBean> prototypeBeanProvider;

        public int logic() {
            PrototypeBean prototypeBean = prototypeBeanProvider.get();
            prototypeBean.addCount();
            int cnt = prototypeBean.getCount();
            return cnt;
        }
    }

    @Scope("prototype")
    static class PrototypeBean {
        private int count = 0;

        public void addCount() {
            count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init " + this);
        }

        @PreDestroy
        public void destory() {

        }
    }
}
```

스프링은 일반적으로 싱글톤 빈을 사용하므로 싱글톤 빈이 프로토타입 빈을 사용하게 된다. 그런데 싱글톤 빈은 생성 시점에만 의존관계 주입을 받기 때문에 프로토타입 빈이 새로 생성되기는 하지만 싱글톤 빈과 함께 계속
유지되는 것이 문제다.

아마 원하는 것이 이런 것은 아닐 것이다. 프로토타입 빈을 주입 시점에만 새로 생성하는게 아니라 사용할 때마다 새로 생성해서 사용하는 것이 개발자가 의도한 것일 것이다.

참고 : 여러 빈에서 같은 프로토 타입 빈을 주입 받으면 주입 받는 시점에 각각 새로운 프로토타입 빈이 생성된다. 예를 들어서 clientA, clientB가 각각 의존관계 주입을 받으면 각각 다른 인스턴스의
프로토타입 빈을 주입 받는다. clientA -> prototypeBean@x01 clientB -> prototypeBean@x02 물론 사용할 때마다 새로 생성되는 것은 아니다.

### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 Provider로 문제해결(ObjectFactory, ObjectProvider)

싱글톤 빈과 프로토타입 빈을 함께 사용할 때 어떻게 하면 사용할 때 마다 항상 새로운 프로토타입 빈을 생성할 수 있을까?

스프링 컨테이너에 요청

가장 간단한 방법은 싱글톤 빈이 프로토타입을 사용할 때마다 스프링 컨테이너에 새로 요청하는 것이다.

```java
public class PrototypeProviderTest {
    @Test
    void providerTest() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(ClientBean.class, PrototypeBean.class);

        ClientBean clientBean1 = ac.getBean(ClientBean.class);
        int count1 = clientBean1.logic();
        assertThat(count1).isEqualTo(1);

        ClientBean clientBean2 = ac.getBean(ClientBean.class);
        int count2 = clientBean2.logic();
        assertThat(count2).isEqualTo(1);
    }

    static class ClientBean {
        @Autowired
        private ApplicationContext ac;

        public int logic() {
            PrototypeBean prototypeBean = ac.getBean(PrototypeBean.class);
            prototypeBean.addCount();
            int count = prototypeBean.getCount();
            return count;
        }

    }

    @Scope("prototype")
    static class PrototypeBean {
        private int count = 0;

        public void addCount() {
            count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init " + this);
        }

        @PreDestroy
        public void destroy() {
            System.out.println("PrototypeBean.destroy");
        }
    }
}
```

1. 실행해보면 ac.getBean()을 통해서 항상 새로운 프로토타입 빈이 생성되는 것을 확인할 수 있다.
2. 의존관계를 외부에서 주입(DI) 받는게 아니라 직접 필요한 의존관계를 찾는 것을 Dependency Lookup(DL) 의존관계 탐색이라 한다.
3. 이렇게 스프링 애플리케이션 컨텍스트 전체를 주입받게 되면 스프링 컨테이너에 종속적인 코드가 되고 단위 테스트도 어려워진다.
4. 지금 필요한 기능은 지정한 프로토타입 빈을 컨테이너에서 대신 찾아주는 딱 DL 정도의 기능만 제공하는 무언가가 있으면 된다.

스프링에는 이미 모든게 준비되어 있다.

* ObjectFactory, ObjectProvider

지정한 빈을 컨테이너에서 대신 찾아주는 DL 서비스를 제공하는 것이 바로 ObjectProvider이다. 참고로 과거에는 ObjectFactory가 있었는데 여기에 편의 기능을 추가해서 ObjectProvider가
만들어졌다.

```java
    static class ClientBean {
    @Autowired
    private ObjectProvider<PrototypeBean> prototypeBeanProvider;

    public int logic() {
        PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
        prototypeBean.addCount();
        int count = prototypeBean.getCount();
        return count;
    }
}

```

1. 실행해보면 prototypeBeanProvider.getObejct()를 통해서 항상 새로운 프로토타입 빈이 생성되는 것을 확인할 수 있다.
2. ObjectProvider의 getObject()를 호출하면 내부에서는 스프링 컨테이너를 통해 해당 빈을 찾아서 반환한다.(DL)
3. 스프링이 제공하는 기능을 사용하지만, 기능이 단순하므로 단위테스트를 만들거냐 mock코드를 만들기는 훨씬 쉬워진다.
4. ObjectProvider는 지금 딱 필요한 DL정도의 기능만 제공한다.

특징

1. ObjectFactory : 기능이 단순, 별도의 라이브러리 필요 없음. 스프링에 의존
2. ObjectProvider : ObjectFactory 상속, 옵션, 스트림 처리등 편의 기능이 많고 별도의 라이브러리 필요 없음. 스프링에 의존

* JSR-330 Provider

마지막 방법은 javax.inject.Provider라는 JSR 330 자바 표준을 사용하는 방법이다. 이 방법을 사용하려면 javax.inject:javax.inject:1 라이브러리를 gradle에 추가해야
한다.

javax.inject.Provider 참고용 코드

```java
package javax.inject;
public interface Provider<T> {
    T get();
}
```

```java
//implementation 'javax,inject:javax.inject:1' gradle 추가 필요
static class ClientBean {
    @Autowired
    private Provider<PrototypeBean> provider;

    public int logic() {
        PrototypeBean prototypeBean = provider.get();
        prototypeBean.addCount();
        int count = prototypeBean.getCount();
        return count;
    }
}
```

1. get() 메소드 하나로 기능이 매우 단순한다.
2. 별도의 라이브러리가 필요하다.
3. 자바 표준이므로 스프링이 아닌 다른 컨테이너에서도 사용할 수 있다.

정리

1. 프로타입 빈을 언제 사용할까 ? 매번 사용할 때마다 의존관계 주입이 완료된 새로운 객체가 필요하면 사용하면 된다. 그런데 실무에서 웹 애플리케이션을 개발해보면 싱글톤 빈으로 대부분 문제를 해결할 수 있기 때문에
   프로토타입 빈을 직접적으로 사용하는 일은 매우 드물다.
2. ObjectProvider, JSR330 Provider 등은 프로토타입 뿐만 아니라 DL이 필요한 경우는 언제든지 사용할 수 있다.

참고 : 스프링이 제공하는 메서드에 @Lookup 애노테이션을 사용하는 방법도 있지만 이전 방법들로 충분하고 고려해야할 내용도 많아서 생략하겠다. 참고 : 실무에서 자바 표준인 JSR-330 Provider를 사용할
것인지 아니면 스프링이 제공하는 ObjectProvider를 사용할 것인지 고민이 될 것이다. ObjectPRovider는 DL를 위한 편의 기능을 많이 제공해주고 스프링 외에 별도의 의존관계 추가가 필요 없기
때문에 편리하다. 만약(그럴일은 거의 없겠지만) 코드를 스프링이 아닌 다른 컨테이너에서도 사용할 수 있어야 한다면 JSR 330 Provider를 사용해야 한다. 스프링을 사용하다보면 이 기능 뿐만 아니라 다른
기능들도 자바 표준과 스프링이 제공하는 기능이 겹칠 때가 많이 있다. 대부분 스프링이 더 다양하고 편리한 기능을 제공해주기 때문에 특별히 다른 컨테이너를 사용할일이 없다면 스프링이 제공하는 기능을 사용하면 된다.

### 웹 스코프

지금까지 싱글톤과 프로토타입 스코프를 학습했따. 싱글톤은 스프링 컨테이너의 시작과 끝까지 함께하는 매우 긴 스코프이고, 프로토타입은 생성과 의존관계 주입, 그리고 초기화까지만 진행하는 특별한 스코프이다.

이번에는 웹 스코프에 대해서 알아보자.

웹 스코프 특징

1. 웹 스코프는 웹 환경에서만 동작한다.
2. 웹 스코프는 프로토타입과 다르게 스프링이 해당 스코프의 종료시점까지 관리한다. 따라서 종료 메서드가 호출된다.

웹 스코프 종류

* request : HTTP 요청 하나가 들어오고 나갈 때 까지 유지되는 스코프. 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고 관리된다.
* session : HTTP Session과 동일한 생명주기를 가지는 스코프
* application : 서블릿 컨텍스트(ServletContext)와 동일한 생명주기를 가지는 스코프
* websocket : 웹 소켓과 동일한 생명주기를 가지는 스코프

사실 세션이나, 서블릿 컨텍스트, 웹 소켓 같은 용어를 잘 모르는 분들도 있을 것이다. 여기서는 request 스코프를 예제로 설명하겠다.

![img_63](https://user-images.githubusercontent.com/79847020/156158674-0e753189-8550-4f19-aceb-dd3614b972e6.png)

### request 스코프 예제 만들기

웹 환경 추가

웹 스코프는 웹 환경에서만 동작하므로 web 환경이 동작하도록 라이브러리를 추가하자.

build.gradle에 추가

```text
//web 라이브러리 추가
implementation 'org.springframework.boot:spring-boot-starter-web'
```

이제 hello.core.CoreApplication의 main 메서드를 실행하면 웹 애플리케이션이 실행되는 것을 확인할 수 있다.

```text
Tomcat started on port(s) : 8080 (http) with context path ''
Started CoreApplication in 0.914 seconds (JVM running for 1.528)
```

참고 : spring-boot-starter-web 라이브러리를 추가하면 스프링 부트는 내장 톰캣 서버를 활용해서 웹 서버와 스프링을 함께 실행시킨다.

참고 : 스프링 부트는 웹 라이브러리가 없으면 우리가 지금까지 학습한 AnnotationConfigApplicationContext 을 기반으로 애플리케이션을 구동한다. 웹 라이브러리가 추가되면 웹과 관련된 추가
설정과 환경들이 필요하므로 AnnotationConfigServletApplicationCOntext를 기반으로 애플리케이션을 구동한다.

만약 기본 포트인 8080포트를 다른 곳에서 사용중이어서 오류가 발생하면 포트를 변경해야 한다. 9090포트로 변경하려면 다음 설정을 추가하자.

main/resources/application.properties

```text
server.port=9090
```

request 스코프 예쩨 개발

동시에 여러 HTTP 요청이 오면 정확히 어떤 요청이 남긴 로그인지 구분하기 어렵다. 이럴때 사용하기 딱 좋은 것이 바로 request 스코프이다.

다음과 같이 로그가 남도록 request 스코프를 활용해서 추가 기능을 개발해보자.

```text
[d06b992f...] request scope bean create
[d06b992f...][http://localhost:8080/log-demo] controller test
[d06b992f...][http://localhost:8080/log-demo] service id = testId
[d06b992f...] request scope bean close
```

기대하는 공통 포맷 : [UUID][requestURL][message]
UUID를 사용해서 HTTP 요청을 구분하자. requestURL 정보도 추가로 넣어서 어떤 URL을 요청해서 남은 로그인지 확인하자.

먼저 코드로 확인해보자. MyLogger

```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyLogger {

    private String uuid;
    private String requestURL;

    public void setRequestURL(String requestURL) {
        this.requestURL = requestURL;
    }

    public void log(String message) {
        System.out.println("[" + uuid + "]" + "[" + requestURL + "]" + message);
    }

    @PostConstruct
    public void init() {
        uuid = UUID.randomUUID().toString();
        System.out.println("[" + uuid + "] request scope bean create:" + this);
    }

    @PreDestroy
    public void close() {
        System.out.println("[" + uuid + "] request scope bean close:" + this);
    }
}
```

1. 로그를 출력하기 위한 MyLogger 클래스이다.
2. @Scope(value = "request")를 사용해서 request 스코프로 지정했다. 이제 이 빈은 HTTP 요청 당 하나씩 생성되고 HTTP 요청이 끝나는 시점에 소멸된다.
3. 이 빈은 생성되는 시점에 자동으로 @PostConstruct 초기화 메서드를 사용해서 uuid를 생성해서 저장해둔다. 이 빈은 HTTP요청 당 하나씩 생성되므로 uuid를 저장해두면 다른 HTTP 요청과 구분할
   수 있다.
4. 이 빈이 소멸되는 시점에 @PreDestroy를 사용해서 종료 메서드를 남긴다.
5. requestURL은 이 빈이 더 생성되는 시점에는 알 수 없으므로 외부에서 setter로 입력받는다.

LogDemoController

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final MyLogger myLogger;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.login("testId");
        return "OK";
    }
}
```

1. 로거가 잘 작동하는지 확인하는 테스트 컨트롤러이다.
2. 여기서 HttpServletRequest를 통해서 요청 URL을 받았다.
  1. requestURL 값 http://localhost:8080/log-demo
3. 이렇게 받은 requestURL 값을 myLogger에 저장해둔다. myLogger는 HTTP요청 당 각각 구분되므로 다른 HTTP 요청 때문에 값이 섞이는 걱정은 하지 않아도 된다.
4. 컨트롤러에서 controller test라는 로그를 남긴다.

참고 : requestURL을 MyLogger에 저장하는 부분은 컨트롤러 보다는 공통 처리가 가능한 스프링 인터셉터나 서블릿 필터 같은 곳을 활용하는 것이 좋다. 여기서는 예제를 단순화하고 아직 스프링 인터셉터를
학습하지 않은 분들을 위해서 컨트롤러를 사용했따. 스프링 웹에 익숙하다면 인터셉터를 사용해서 구현해보자.

LogDemoService 추가

```java
@Service
@RequiredArgsConstructor
public class LogDemoService {

    private final MyLogger myLogger;

    public void login(String id) {
        myLogger.log("service id = " + id);
    }
}
```

1. 비즈니스 로직이 있는 서비스 계층에서도 로그를 출력해보자.
2. 여기서 중요한점이 있다. request scope를 사용하지 않고 파라미터로 이 모든 정보를 서비스 계층에 넘긴다면 파라미터가 많아서 지저분해진다. 더 문제는 requestURL 같은 웹과 관련된 정보가 웹과
   관련없는 서비스 계층까지 넘어가게 된다. 웹과 관련된 부분은 컨트롤러까지만 사용해야 한다. 서비스 계층은 웹 기술에 종속되지 않고 가급적 순수하게 유지하는 것이 유지보수 관점에서 좋다.
3. request scope의 MyLogger 덕분에 이런 부분을 파라미터로 넘기지 않고 MyLogger의 멤버변수에 저장해서 코드와 계층을 깔끔하게 유지할 수 있다.

기대하는 출력

```
[d06b992f...] request scope bean create
[d06b992f...][http://localhost:8080/log-demo] controller test
[d06b992f...][http://localhost:8080/log-demo] service id = testId
[d06b992f...] request scope bean close
```

실제는 기대와 다르게 애플리케이션 실행 시점에 오류 발생

```text
Error creating bean with name 'myLogger': Scope 'request' is not active for the current thread; consider defining a scoped proxy for this bean if you intend to refer to it from a singleton;
```

스프링 애플리케이션을 실행 시키면 오류가 발생한다. 스프링 애플리케이션을 실행하는 시점에 싱글톤 빈은 생성해서 주입이 가능하지만 request 스코프 빈은 아직 생성되지 않는다. 이 빈은 요청이 와야 생성할 수
있다.

### 스코프와 Provider

첫번째 해결방법은 Provider를 사용하는 것이다. ObjectProvider를 사용해보자.

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final ObjectProvider<MyLogger> myLoggerProvider;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        MyLogger myLogger = myLoggerProvider.getObject();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.login("testId");
        return "OK";
    }
}
```

```java
@Service
@RequiredArgsConstructor
public class LogDemoService {

    private final ObjectProvider<MyLogger> myLoggerProvider;

    public void login(String id) {
        MyLogger myLogger = myLoggerProvider.getObject();
        myLogger.log("service id = " + id);
    }
}
```

스프링 애플리케이션을 시작한 후 웹 브라우저에서 http://localhost:8080/log-demo에 접속하자.

```
[d06b992f...] request scope bean create
[d06b992f...][http://localhost:8080/log-demo] controller test
[d06b992f...][http://localhost:8080/log-demo] service id = testId
[d06b992f...] request scope bean close
```

1. ObjectProvider 덕분에 ObjectProvider.getObject()를 호출하는 시점까지 request scope 빈의 생성을 지연할 수 있다.
2. ObjectProvider.getObject()를 호출하시는 시점에는 HTTP요청이 진행중이므로 request scope 빈의 생성이 정상 처리된다.
3. ObjectProvider.getObject()를 LogDemonController, LogDemoService에서 각각 한번씩 따로 호출해도 같은 HTTP 요청이면 같은 스프링 빈이 반환된다.

### 스코프와 프록시

이번에는 프록시 방법을 사용해보자.

```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyLogger {
}
```

1. 여기가 핵심이다. proxyMode = ScopedProxyMode.TARGET_CLASS 를 추가해주자.
  1. 적용 대상이 인터페이스가 아닌 클래스면 TARGET_CLASS를 선택
  2. 적용 대상이 인터페이스면 INTERFACES를 선택
2. 이렇게 하면 MyLogger의 가짜 프록시 클래스를 만들어두고 HTTP request와 상관 없이 가짜 프록시 클래스를 다른 빈에 미리 주입해둘 수 있다.

코드를 Provider 사용 이전으로 돌려두자.

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final MyLogger myLogger;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.login("testId");
        return "OK";
    }
}

@Service
@RequiredArgsConstructor
public class LogDemoService {

    private final MyLogger myLogger;

    public void login(String id) {
        myLogger.log("service id = " + id);
    }
}
```

실행해보면 잘 동작하는 것을 확인할 수 있다.

코드를 잘 보면 LogDemoController, LogDemoService는 Provider 사용 전과 완전히 동일하다. 어떻게 된 것일까?

myLogger 클래스를 출력해보자.

```text
myLogger = class.hello.core.common.MyLogger$$EnhanceBySpringCGLIB$$b68b726d
```

1. CGLIB라는 라이브러리로 내 클래스를 상속 받은 가짜 프록시 객체를 만들어서 주입한다.
2. @Scope의 proxyMode = ScopedProxyMode.TARGET_CLASS 를 설정하면 스프링 컨테이너는 CGLIB 라는 바이트코드를 조작하는 라이브러리를 사용해서 MyLogger를 상속받은 가짜
   프록시 객체를 생성한다.
3. 결과를 확인해보면 우리가 등록한 순수한 MyLogger 클래스가 아니라 MyLogger$$EnhanceBySpringCGLIB라는 클래스로 만들어진 객체가 대신에 등록된 것을 확인할 수 있다.
4. 그리고 스프링 컨테이너에 myLogger라는 이름으로 진짜 대신에 이 가짜 프록시 객체를 등록한다.
5. ac.getBean("myLogger", MyLogger.class)로 조회해도 프록시 객체가 조회되는 것을 확인할 수 있다.
6. 의존관계 주입도 역시 프록시 객체가 주입된다.

![img_64](https://user-images.githubusercontent.com/79847020/156158684-ee825cf3-34c9-4a7f-92f1-fbaace646ee7.png)

1. 가짜 프록시 객체를 요청이 오면 그때 내부에서 진짜 빈을 요청하는 위임 로직이 들어있다.
2. 가짜 프록시 객체는 내부에 진짜 myLogger를 찾는 방법을 알고 있다.
3. 클라이언트가 myLogger.logic()을 호출하면 사실은 가짜 프록시 객체의 메서드를 호출한 것이다.
4. 가짜 프록시 객체는 request 스코프의 진짜 MyLogger.logic()을 호출한다.
5. 가짜 프록시 객체는 원본 클래스를 상속 받아서 만들어졌기 때문에 이 객체를 사용하는 클라이언트 입장에서는 사실 원본인지 아닌지도 모르게 동일하게 사용할 수 있다.(다형성)

동작 정리

1. CGLIB라는 라이브러리로 내 클래스를 상속 받은 가짜 프록시 객체를 만들어 주입한다.
2. 이 가짜 프록시 객체는 실제 요청이 오면 그때 내부에서 실제 빈을 요청하는 위임 로직이 들어있다.
3. 가짜 프록시 객체는 실제 request scope와는 관계가 없다. 그냥 가짜이고 내부에 단순한 위임 로직만 있고 싱글톤 처럼 동작한다.

특징 정리

1. 프록시 객체 덕분에 클라이언트는 마치 싱글톤 빈을 사용하듯이 편리하게 request scope를 사용할 수 있다.
2. 사실 Provider를 사용하든, 프록시를 사용하든 핵심 아이디어는 진짜 객체 조회를 필요한 시점까지 지연처리 한다는 점이다.
3. 단지 애노테이션 설정 변경만으로 원본 객체를 프록시 객체로 대체할 수 있다. 이것에 바로 다형성과 DI컨테이너가 가진 큰 강점이다.
4. 꼭 웹 스코프가 아니어도 프록시는 사용할 수 있다.

주의점

1. 마치 싱글톤을 사용하는 것 같지만 다르게 동작하기 때문에 주의해야한다.
2. 특별한 scope는 꼭 필요한 곳에서만 최소화해서 사용하자. 무분별하게 사용하면 유지보수 하기 어려워진다.

# 10. 다음으로







