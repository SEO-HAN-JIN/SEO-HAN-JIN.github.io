---
title : "[Java] Object"
categories: Java
tag: [java]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
# search: false # 검색을 원하지 않을 때
---
&nbsp;

# 개요

`Object`에 대해서 알아보자.

&nbsp;

# `java.lang` 패키지

자바가 기본적으로 제공하는 라이브러리(클래스 모음) 중에 가장 기본이 되는 것이 `java.lang` 패키지이다.

- `Object` : 모든 자바 객체의 부모 클래스
- `String` : 문자열
- `Integer`, `Long`, `Double` : 래퍼 타입, 기본형 데이터 타입을 객체로 만든 것
- `Class` : 클래스 메타 정보
- `System` : 시스템과 관련된 기본 기능들을 제공

## `import` 생략 가능

`java.lang` 패키지는 모든 자바 애플리케이션에 자동으로 임포트된다. 따라서 임포트 구문을 사용하지 않아도된다.

```java
package lang;

import java.lang.System;	// 생략 가능

public class LangMain {
    public static void main(String[] args) {
        System.out.println("hello java");
    }
}

```

`System` 클래스는 `java.lang` 패키지 소속이다. 따라서 생략할 수 있다.

&nbsp;

# Object 클래스

자바에서 모든 클래스의 최상위 부모 클래스는 항상 `Object`클래스이다.

```java
// 부모가 없으면 묵시적으로 Object 클래스를 상속받는다. >> 묵시적 상속
public class Parent {
    // extends Object << 아무것도 상속을 받지 않으면 무조건 자동으로 Object를 상속함.

    public void parentMethod() {
        System.out.println("Parent.parentMethod");
    }
}
```

```java
// Parent를 상속받았기 때문에 Object를 상속받지 않는다. >> 명시적 상속
public class Child extends Parent{

    public void childMethod() {
        System.out.println("Child.childMethod");
    }
}
```

![1719125789203]({{site.url}}/images/2024-06-23-java-object/1719125789203.png)

> **명시적(Implicit) vs 명시적(Explicit)**
>
> 묵시적: 개발자가 코드에 직접 기술하지 않아도 시스템 또는 컴파일러에 의해 자동으로 수행되는 것을 의미
>
> 명시적: 개발자가 코드에 직접 기술해서 작동하는 것을 의미

```java
public class ObjectMain {

    public static void main(String[] args) {
        Child child = new Child();
        child.childMethod();
        child.parentMethod();

        // toString()은 Object 클래스의 메서드
        String string = child.toString();
        System.out.println(string);
    }
}

```

- `child.toString()`을 호출한다.
- 먼저 본인 타입인 `Child`에서 `toString()`을 찾는다. 없으므로 부모 타입으로 올라가서 찾는다.
- 부모 타입인 `Parent`에서 찾는다. 없으므로 부모 타입으로 올라가서 찾는다.
- 부모 타입인 `Object`에서 찾는다. `Object`에 `toString()`이 있으므로 이 메서드를 호출한다.

## 자바에서 `Object` 클래스가 최상위 부모 클래스인 이유

- 공통 기능 제공
  - 모든 객체에 필요한 기본기능인 `toString()`, `equals()`, `getClass()`등을 지원해준다.
- 다형성의 기본 구현
  - `Object`는 모든 객체를 다 담을 수 있다. 타입이 다른 객체들을 어딘가에 보관해야 한다면 `Object`에 보관하면 된다.

## `Object` 다형성

```java
public class Car {
    public void move() {
        System.out.println("자동차 이동");
    }
}

```

```java
public class Dog {
    public void sound() {
        System.out.println("멍멍");
    }
}

```

```java
public class ObjectPolyExample1 {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Car car = new Car();

        action(dog);
        action(car);
    }

    private static void action(Object obj) {
        // obj.sound(); // 컴파일 오류, Object는 sound()가 없다.
        // obj.move(); // 컴파일 오류, Object는 move()가 없다.


        // 객체에 맞는 다운캐스팅 필요
        if (obj instanceof Dog dog) {
            dog.sound();
        } else if (obj instanceof Car car) {
            car.move();
        }
    }
}
```

![1719139712616]({{site.url}}/images/2024-06-23-java-object/1719139712616.png)

**결과**

```java
멍멍
자동차 이동
```

`Object`는 모든 타입의 부모다. 따라서 어떤 객체든지 인자로 전달할 수 있다.

```java
1. 
Object dog = new Dog(); // Dog -> Object
Object car = new Car();	// Car -> Object

2. 
action(dog);
void action(Object obj = dog(Dog))
```

## Object를 활용한 다형성의 한계

- `Object`는 모든 객체를 대상으로 다형적 참조를 할 수 있다.
- `Object`를 통해 전달 받은 객체를 호출하려면 각 객체에 맞는 다운캐스팅 과정이 필요하다.
  - 위의 코드에서  `obj.sound()`를 호출하려고하면 `obj`는 `Object`타입이므로 `sound()`를 찾을 수 없다. 따라서 다운캐스팅을 해야 한다.
  - `if (obj instanceof Dog dog) {dog.sound()}`

## toString()

`Object.toString()`메서드는 객체의 정보를 문자열 형태로 제공한다. 이 메서드는 `Object`클래스에 정의되므로 모든 클래스에서 상속받아 사용할 수 있다.

```java
public class ToStringMain1 {
    public static void main(String[] args) {
        Object object = new Object();
        String string = object.toString();

        // toString() 반환값 출력
        System.out.println(string);

        // object 직접 출력
        System.out.println(object);
    }
}
```

**결과**

```java
java.lang.Object@10f87f48
java.lang.Object@10f87f48

Process finished with exit code 0
```

여기서 `toString()`의 결과를 출력한 코드와 `Object`를 `println()`에 직접 출력한 코드의 결과가 완전히 같다. 그 이유는 `System.out.println()`메서드는 내부에서 `toString()`을 호출한다. `Object` 타입이 `println()`에 인수로 전달되면 내부에서 `obj.toString()` 메서드를 호출해서 결과를 출력한다.

```java
    public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    }
```

### toString 오버라이딩

```java
public class Dog {
    private String dogName;
    private int age;

    public Dog(String dogName, int age) {
        this.dogName = dogName;
        this.age = age;
    }
  
    @Override
    public String toString() {
        return "Dog{" +
                "dogName='" + dogName + '\'' +
                ", age=" + age +
                '}';
    }
}
```

`Dog`클래스에서 `toString()`을 재정의 했다. `Object.toString()` 메서드가 클래스 정보와 참조값을 제공하지만 이 정보만으로는 객체의 상태를 적절히 나타내지 못한다.

## Object와 OCP

`Object`가 없다면 서로 아무 관계가 없는 객체의 정보를 출력하기 어려울 것이다.

**기존**

```java
public class BadObjectPrinter {
 public static void print(Car car) { //Car 전용 메서드
 String string = "객체 정보 출력: " + car.carInfo(); //carInfo() 메서드 만듬
 System.out.println(string);
 }
 public static void print(Dog dog) { //Dog 전용 메서드
 String string = "객체 정보 출력: " + dog.dogInfo(); //dogInfo() 메서드 만듬
 System.out.println(string);
 }
}
```

구체적인 특정 클래스인 `Car`, `Dog` 를 사용하는 것을 `BadObjectPrinter`는 `Car`, `Dog`에 의존한다고 표현한다.

**개선**

```java
public class ObjectPrinter {
 public static void print(Object obj) {
 String string = "객체 정보 출력: " + obj.toString();
 System.out.println(string);
 }
}
```

`ObjectPrinter`클래스가 `Object`클래스를 사용하는 것을 `ObjectPrinter` 클래스가 `Object` 클래스에 의존한다고 표현한다. `ObjectPrinter`는 구체적인 것에 의존하는 것이 아니라 추상적인 것에 의존한다.

![1719149288577]({{site.url}}/images/2024-06-23-java-object/1719149288577.png)

## equals()

- 동일성 : `==` 연산자를 사용해서 두 객체의 참조가 동일한 객체를 가리키고 있는지 확인
- 동등성 : `equals()` 메서드를 사용하여 두 객체가 논리적으로 동등하지 확인

"동일"은 완전히 같음을 의미하고 "동등"은 같은 가치나 수준을 의미하지만 그 형태나 외간 등이 완전히 같지는 않을 수 있다. 즉 동일성은 물리적으로 같은 메모리에 있는 객체 인스턴스인지 참조값을 확인하는 것이고, 동등성은 논리적으로 같은지 확인하는 것이다.

### 예제

```java
public class UserV1 {

    private String id;

    public UserV1(String id) {
        this.id = id;
    }
}
```

```java
public class EqualsMainV1 {
    public static void main(String[] args) {
        UserV1 user1 = new UserV1("id-100");
        UserV1 user2 = new UserV1("id-100");

        System.out.println("identity = " + (user1 == user2));
        System.out.println("equality = " + (user1.equals(user2)));
    }
}
```

**결과**

```java
identity = false
equality = false

Process finished with exit code 0
```

![1719150540337]({{site.url}}/images/2024-06-23-java-object/1719150540337.png)

**동일성 비교**

```java
user1 == user2
x001 == x002

false //결과
```

**동등성 비교**

```java
public boolean equals(Object obj) {
 return (this == obj);
}
```

`Object`의 `equals()`는 `==` 으로 동일성 비교를 제공한다.

```java
user1.equals(user2);
return (user1 == user2) // Object.equals 메서드 안
return (x001 == x002)	// Object.equals 메서드 안
return false;
false
```

## equals() 구현

`id`가 같으면 논리적으로 같은 객체로 정의하기 위해 `equals()`를 재정의 하자.

```java
public class UserV2 {

    private String id;

    public UserV2(String id) {
        this.id = id;
    }


 @Override
 public boolean equals(Object obj) {
 UserV2 user = (UserV2) obj;
 return id.equals(user.id);
 }
  
}
```

```java
public class EqualsMainV2 {
    public static void main(String[] args) {
        UserV2 user1 = new UserV2("id-100");
        UserV2 user2 = new UserV2("id-100");

        System.out.println("identity = " + (user1 == user2));
        System.out.println("identity = " + (user1.equals(user2)));
    }
}
```

**결과**

```java
identity = false
identity = true

Process finished with exit code 0
```

실제로 정확하게 동작하려면 IDE의 도움을 받아 `equals()` 코드를 자동으로 만들어준다.

```java
//변경 - 정확한 equals 구현, IDE 자동 생성
@Override
public boolean equals(Object o) {
 if (this == o) return true;
 if (o == null || getClass() != o.getClass()) return false;
 User user = (User) o;
 return Objects.equals(id, user.id);
}
```
