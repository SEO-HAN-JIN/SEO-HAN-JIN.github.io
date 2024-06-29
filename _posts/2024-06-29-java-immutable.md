---
title : "[Java] 불변객체"
categories: Java
tag: [java]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
# search: false # 검색을 원하지 않을 때
---
&nbsp;

# 데이터 타입의 종류

- **기본형** : 하나의 값을 여러 변수에서 절대로 공유하지 않는다.
- **참조형** : 하나의 객체를 참조값을 통해 여러 변수에서 공유할 수 있다.

&nbsp;

# 기본형은 값을 절대로 공유하지 않는다.

```java
public class PrimitiveMain {
    public static void main(String[] args) {
        // 기본형은 절대로 같은 값을 공유하지 않는다.
        int a = 10;
        int b = a; // a -> b, 값 복사 후 대입
        System.out.println("a = " + a);
        System.out.println("b = " + b);

        b = 20;
        System.out.println("20 - > b");
        System.out.println("a = " + a);
        System.out.println("b = " + b);

    }
}
```

**결과**

```java
a = 10
b = 10
20 - > b
a = 10
b = 20

Process finished with exit code 0

```

- 기본형 변수 `a`와 `b`는 절대로 하나의 값을 공유하지 않는다.
- `b = a` 라고 하면 자바는 항상 값을 복사해서 대입한다.  이 경우 `a`에 있는 값 `10`을 복사해서 `b`에 전달한다.
- 결과적으로 `a`와 `b`는 둘다 `10`이라는 똑같은 숫자의 값을 가진다. 하지만 `a`가 가지는 `10`과 `b`가 가지는 `10`은 복사된 완전히 다른 `10`이다. 메모리 상에서도 `a`에 속하는 `10`과 `b`에 속하는 `10`이 각각 별도로 존재한다.
- `b=20`이라고 하면 `b`의 값만 `20`으로 변경된다.
- `a`의 값은 `10`으로 그대로 유지된다.
- 기본형 변수는 하나의 값을 절대로 공유하지 않는다. 따라서 값을 변경해도 변수 하나의 값만 변경된다. 여기서는 변수 `b`의 값만 `20`으로 변경되었다.

&nbsp;

# 참조형은 공유한다.

```java
public class Address {
    private String value;

    public Address(String value) {
        this.value = value;
    }

    public String getValue() {
        return value;
    }

    public void setValue(String value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Address{" +
                "value='" + value + '\'' +
                '}';
    }
}
```

```java
public class RefMain1_1 {

    public static void main(String[] args) {
        // 참조형 변수는 하나의 인스턴스를 공유할 수 있다.
        Address a = new Address("서울");
        Address b = a;

        System.out.println("a = " + a);
        System.out.println("b = " + b);

        b.setValue("부산"); // b의 값을 부산으로 변경해야함
        System.out.println("부산 -> b");
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
}
```

**결과**

```java
a = Address{value='서울'}
b = Address{value='서울'}
부산 -> b
a = Address{value='부산'}
b = Address{value='부산'}

Process finished with exit code 0
```

- `b`뿐만 아니라 `a`의 주소도 함께 `부산`으로 변경되었다.

![1719662310062]({{site.url}}/images/2024-06-29-java-immutable/1719662310062.png)

- 참조형 변수들은 같은 참조값을 통해 같은 인스턴스를 참조할 수 있다.
- `b=a`라고 하면 `a`에 있는 참조값 `x001`을 복사해서 `b`에 전달한다.
  - 자바에서 모든 값 대입은 변수가 가지고 있는 값을 복사해서 전달한다. 변수가 `int`같은 숫자값을 가지고 있으면 숫자값을 복사해서 전달하고, 참조값을 가지고 있으면 참조값을 복사해서 전달한다.
- 결과적으로 `a`, `b`는 같은 `x001`인스턴스를 참조한다.
- 기본형 변수는 절대로 같은 값을 공유하지 않는다.
  - `a=10`, `b=10`과 같이 같은 모양의숫자 `10`이라는 값을 가질 수는 있지만 같은 값을 공유하는 것은 아니다. 서로 다른 숫자 `10`이 두 개 있는 것이다.
- 참조형 변수는 참조값을 통해 같은 객체(인스턴스)를 공유할 수 있다.

&nbsp;

# 공유 참조와 사이드 이펙트

사이드 이펙트(`Side Effect`)는 프로그래밍에서 어떤 계산이 주된 작업 외에 추가적인 부수 효과를 일으키는 것을 말한다.

## `b.setValue("부산")`

위의 코드에서 `b`의 주소값을 서울에서 부산으로 변경할 의도로 값 변경을 시도했다. 하지만 `a`, `b`는 같은 인스턴스를 참조한다. 따라서 `a`의 값도 함께 부산으로 변경되어 버린다.

이렇게 주된 작업 외에 추가적인 부수 효과를 일으키는 것을 사이드 이펙트라 한다.

### 해결방안

아래 코드와 같이 `a`와 `b`가 처음부터 서로 다른 인스턴스를 참조하면 된다.

```java
public class RefMain1_2 {

    public static void main(String[] args) {
        // 참조형 변수는 하나의 인스턴스를 공유할 수 있다.
        Address a = new Address("서울"); // x001
        Address b = new Address("서울"); // x002

        System.out.println("a = " + a);
        System.out.println("b = " + b);

        b.setValue("부산"); // b의 값을 부산으로 변경해야함
        System.out.println("부산 -> b");
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
}
```

![1719663322867]({{site.url}}/images/2024-06-29-java-immutable/1719663322867.png)

`a`와 `b`는 서로 다른 인스턴스를 참조한다. 따라서 `b`가 참조하는 인스턴스의 값을 변경해도 `a`에는 영향을 주지 않는다.

## 하나의 객체를 공유하는 것을 막을 방법은 없다

위의 해결 방법과 같이  `b = a`와 같은 코드를 작성하지 않도록 해서, 여러 변수가 하나의 참조값을 공유하지 않으면 문제가 해결될 것 같지만 `Address`를 사용하는 개발자 입장에서 실수로 `b = a`라고 해도 아무런 오류가 발생하지 않는다. 왜냐하면 문법상 참조형 변수의 대입은 아무런 문제가 없기 때문이다.

그러면 공유 참조로 인해 발생하는 문제를 어떻게 해결할 수 있을까?

# 불변 객체

위에서 보았던 문제는 공유하면 안되는 객체를 변수에서 공유했기 때문에 발생한 문제이다. 객체를 공유하는 것 자체는 문제가 아니다. 객체를 공유한다고 바로 사이드 이펙트가 발생하지는 않는다. **문제의 직접적인 원인은 객체의 값을 변경한 것에 있다.**

## 도입

객체의 상태(객체 내부의 값, 필드, 멤버 변수)가 변하지 않는 객체를 불변 객체(`Immutable Object`)라 한다. 위에서 만들었던 `Address` 클래스를 상태가 변하지 않는 불변 클래스로 다시 만들어보자.

```java
public class ImmutableAddress {
    private final String value;

    public ImmutableAddress(String value) {
        this.value = value;
    }

    public String getValue() {
        return value;
    }
  

    @Override
    public String toString() {
        return "Address{" +
                "value='" + value + '\'' +
                '}';
    }
}
```

- 내부 값이 변경되면 안되기 때문에 `value`의 필드를 `final`로 선언했다.
- 값을 변경할 수 있는 `setValue()`를 제거했다.
- 이 클래스는 생성자를 통해서만 값을 설정할 수 있고, 이후에는 값을 변경하는 것이 불가능하다.

```java
public class RefMain2 {
    public static void main(String[] args) {
        // 참조형 변수는 하나의 인스턴스를 공유할 수 있다.
        ImmutableAddress a = new ImmutableAddress("서울");
        ImmutableAddress b = a; // 참조값 대입을 막을 수 있는 방법이 없다.

        System.out.println("a = " + a);
        System.out.println("b = " + b);

//        b.setValue("부산"); // b의 값을 부산으로 변경해야함
        b = new ImmutableAddress("부산");
        System.out.println("부산 -> b");
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
}
```

**결과**

```java
a = Address{value='서울'}
b = Address{value='서울'}
부산 -> b
a = Address{value='서울'}
b = Address{value='부산'}

```

- `ImmutableAdress`의 경우 값을 변경할 수 있는 `b.setValue()` 메서드 자체가 제거되었다.
- 이제 `ImmutableAddress` 인스턴스의 값을 변경할 수 있는 방법은 없다.
- `ImmutableAddress`를 사용하는 개발자는 값을 변경하려고 시도하다가, 값을 변경하는 것이 불가능하다는 것을 알고, 이 객체가 불변 객체인 사실을 깨닫게 된다.

## 정리

**불변이라는 단순한 제약을 사용해서 사이드 이펙트라는 큰 문제를 막을 수 있다.**

- 객체의 공유 참조는 막을 수 없다. 그래서 객체의 값을 변경하면 다른 곳에서 참조하는 변수의 값도 함께 변경되는 사이드 이펙트가 발생한다. 사이드 이펙트가 발생하면 안되는 상황이라면 불변 객체를 만들어서 사용하면 된다. 불변 객체는 값을 변경할 수 없기 때문에 사이드 이펙트가 차단된다.
- 불변 객체는 값을 변경할 수 없다. 따라서 불변 객체의 값을 변경하고 싶다면 변경하고 싶은 값으로 새로운 불변 객체를 생성해야 한다. 이렇게 하면 기존 변수들이 참조하는 값에는 영향을 주지 않는다.

# 불변 객체-예제

`Address`, `ImmutableAddress`를 그대로 활용하여 불변 객체의 사용 예를 확인해보자.

## 변경 클래스 사용

```java
public class MemberV1 {
    private String name;
    private Address address;

    public MemberV1(String name, Address address) {
        this.name = name;
        this.address = address;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "MemberV1{" +
                "name='" + name + '\'' +
                ", address=" + address +
                '}';
    }
}
```

`MemberV1`은 변경 가능한 `Address` 클래스를 사용한다.

```java
public class MemberMainV1 {
    public static void main(String[] args) {
        Address address = new Address("서울");

        MemberV1 memberA = new MemberV1("회원A", address);
        MemberV1 memberB = new MemberV1("회원B", address);

        // 회원A, 회원B의 처음 주소는 모두 서울
        System.out.println("memberA = " + memberA);
        System.out.println("memberB = " + memberB);

        // 회원B의 주소를 부산으로 변경해야함
        memberB.getAddress().setValue("부산");
        System.out.println("부산 -> memberB.address");
        System.out.println("memberA = " + memberA);
        System.out.println("memberB = " + memberB);
    }
}
```

**결과**

```java
memberA = MemberV1{name='회원A', address=Address{value='서울'}}
memberB = MemberV1{name='회원B', address=Address{value='서울'}}
부산 -> memberB.address
memberA = MemberV1{name='회원A', address=Address{value='부산'}}
memberB = MemberV1{name='회원B', address=Address{value='부산'}}
```

- `회원A`와 `회원B`는 둘다 서울에 살고 있다.
- 중간에 `회원B`의 주소를 부산으로 변경해야 한다.
- 그런데 `회원A`와 `회원B`는 같은 `Address`인스턴스를 참조하고 있다.
- `회원B`의 주소를 부산으로 변경하는 순간 `회원A`의 주소도 부산으로 변경된다.

## 불변 클래스 사용

```java
public class MemberV2 {
    private String name;
    private ImmutableAddress address;

    public MemberV2(String name, ImmutableAddress address) {
        this.name = name;
        this.address = address;
    }

    public ImmutableAddress getAddress() {
        return address;
    }

    public void setAddress(ImmutableAddress address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "MemberV1{" +
                "name='" + name + '\'' +
                ", address=" + address +
                '}';
    }
}
```

`MemberV2`는 주소를 변경할 수 없는, 불변인 `ImmutableAddress`를 사용한다.

```java
public class MemberMainV2 {
    public static void main(String[] args) {
        ImmutableAddress address = new ImmutableAddress("서울");

        MemberV2 memberA = new MemberV2("회원A", address);
        MemberV2 memberB = new MemberV2("회원B", address);

        // 회원A, 회원B의 처음 주소는 모두 서울
        System.out.println("memberA = " + memberA);
        System.out.println("memberB = " + memberB);

        // 회원B의 주소를 부산으로 변경해야함
	//memberB.getAddress().setValue("부산"); //컴파일 오류
        memberB.setAddress(new ImmutableAddress("부산"));
//        System.out.println("부산 -> memberB.address");
        System.out.println("memberA = " + memberA);
        System.out.println("memberB = " + memberB);
    }
}
```

**결과**

```java
memberA = MemberV1{name='회원A', address=Address{value='서울'}}
memberB = MemberV1{name='회원B', address=Address{value='서울'}}
memberA = MemberV1{name='회원A', address=Address{value='서울'}}
memberB = MemberV1{name='회원B', address=Address{value='부산'}}

Process finished with exit code 0
```

- 변경 클래스를 사용했던 것과 같이  `회원B`의 주소를 중간에 부산으로 변경하려고 시도한다. 하지만 `ImmutableAddress`에는 값을 변경할 수 있는 메서드가 없다 따라서 컴파일 오류가 발생한다.
- 결국 `memberB.setAddress(new ImmutableAddress("부산"))`와 같이 새로운 주소 객체를 만들어서 전달한다.

## 값 변경

불변 객체를 사용하지만 그대로 값을 변경해야 하는 메서드가 필요하다면 어떻게 해야할까?

```java
public class ImmutableObj {
    private final int value;

    public ImmutableObj(int value) {
        this.value = value;
    }

    public ImmutableObj add(int addValue) {
        int result = value + addValue;
        return new ImmutableObj(result);
    }
    public int getValue() {
        return value;
    }
}

```

- 여기서 핵심은 `add()` 메서드이다.
- 불변 객체는 값을 변경하면 더 이상 불변 객체게 아니게 되지만 여기서는 기존 값에 새로운 값을 더한다.
- 불변 객체는 기존 값을 변경하지 않고 대신에 계산 결과를 바탕으로 새로운 객체를 만들어서 반환한다.
- 이렇게 하며 불변도 유지하면서 새로운 결과도 만들 수 있다.

```java
public class ImmutableMain1 {

    public static void main(String[] args) {
        ImmutableObj obj1 = new ImmutableObj(10);
        ImmutableObj obj2 = obj1.add(20);

        // 계산 이후에도 기존값과 신규값 모두 확인 가능
        System.out.println("obj1 = " + obj1.getValue());
        System.out.println("obj2 = " + obj2.getValue());
    }
}
```

**결과**

```java
obj1 = 10
obj2 = 30

Process finished with exit code 0
```

불변 객체를 설계할 때 기존 값을 변경해야 하는 메서드가 필요할 수 있다 이때는 기존 객체의 값을 그대로 두고 대신에 변경된 결과를 새로운 객체에 담아서 반환하면 된다. 결과를 보면 기존 값이 그대로 유지되는 것을 볼 수 있다.

![1719666557553]({{site.url}}/images/2024-06-29-java-immutable/1719666557553.png)

만약 여기서 새로 생성된 반환 값을 사용하지 않으면 어떻게 될까?

```java
public class ImmutableMain2 {
	public static void main(String[] args) {
	ImmutableObj obj1 = new ImmutableObj(10);
	obj1.add(20);
	System.out.println("obj1 = " + obj1.getValue());
   }
}
```

**결과**

```java
obj1 = 10
```

아무것도 처리되지 않는 것 처럼 보인다. 불변 객체에서 변경과 관련된 메서드들은 보통 객체를 새로 만들어서 반혼하기 떄문에 꼭! 반환 값을 받아야 한다.
