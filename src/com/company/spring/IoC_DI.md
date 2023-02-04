### 제어의 역전IoC(Inversion of Control)

---
#### OrderServiceImpl
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

기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성,연결,실행 했다.
한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다.



#### OrderServiceImpl
```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
    
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    
    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
       Member member = memberRepository.findById(memberId);
       int discountPrice = discountPolicy.discount(member,itemPrice);
       return new Order(memberId,itemName, itemPrice, discountPrice);
    }
}
```

반면에 AppConfig가 등장한 이후에 구현 객체는 자신의 로직을 실행하는 역할만 담당한다.
프로그램의 제어 흐름은 이제 AppConfig가 가져간다. 예를 들어서 OrderServiceImpl은 필요한
인터페이스들을 호출하지만 어떤 구현 객체들이 실행될지 모른다.

#### AppConfig

```java
public class AppConfig {

    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }

    public OrderService orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }

}
```

반면에 AppConfig가 등장한 이후에 구현 객체는 자신의 로직을 실행하는 역할만 담당한다.
프로그램의 제어 흐름은 이제 AppConfig가 가져간다.

이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을
제어의 역전(IoC)라고 한다.

### 프레임워크 vs 라이브러리

---
- 프레임워크가 내가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크(JUnit)
- 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 라이브러리

### 의존관계 주입DI(Dependency Injection)

---
- 의존관계는 **정적인 클래스 의존 관계**와, 실행 시점에 결정되는 **동적인 객체 의존관계**둘을 분리해서 생각해야 한다.
>
#### 정적인 클래스 의존관계
클래스가 사용하는 import 코드만 보고 의존관계를 쉽게  판단할 수 있다.
정적인 의존관계는 애플리케이션을 실행하지 않아도 분석할 수 있다.

>
#### 동적인 객체 인스턴스 의존 관계
애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계다.

- 애플리케이션 **실행 시점**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서
  클라이언트와 서버의 실제 의존관계가 연결 되는 것을 **의존관계 주입** 이라 한다.
- 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.
- 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
- 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.

### IoC컨테이너, DI컨테이너

---
- **AppConcig**처럼 객체를 생성하고 관리하면서 의존 관계를 연결해 주는 것을
- **IoC 컨테이너** 또는 **DI 컨테이너**라 한다

```java
public class AppConfig {

    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }

    public OrderService orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }

}
```
AppConfig를 보면 orderService를 만들 때 OrderServiceImpl객체를 생성할 때
생성자에 memberRepository(),discountPolicy()를 AppConfig가 집어넣어줘서
**DI컨테이너** 라고도 불린다