---
title : "[Java] 추상 클래스와 인터페이스"
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

추상클래스와 인터페이스에 대해서 알아보자.

&nbsp;

# 추상 클래스란

부모 클래스는 제공했지만, 실제 생성되면 안되는 클래스를 추상 클래스라 한다. 추상 클래스는 이름 그대로 추상적인 개념을 제공하는 클래스이다. 따라서 실체인 인스턴스가 존재하지 않는다. 대신에 상속을 목적으로 사용되고, 부모 클래스 역할을 담당한다.

```java
public abstract class AbstractAnimal {
    public abstract void sound();

    public void move() {
        System.out.println("동물이 움직입니다.");
    }
}

```

- `abstract` 키워드를 붙여주면 된다.
- 추상 클래스는 기존 클래스와 완전히 같다. 다만 `new AbstractAnimal()`와 같이 직접 인스턴스를 생성하지 못하는 제약이 추가되었다.

## 추상 메서드

실체가 존재하지 않고, 메서드 바디가 없다.

```java
public abstract void sound();
```

- `abstract` 키워드를 붙여주면 된다.
- 추상 메서드가 하나라도 있는 클래스는 추상 클래스로 선언해야 한다.
- 추상 메서드는 상속 받는 자식 클래스가 반드시 오버라이딩 해서 사용해야 한다.

## 예제

```java
public abstract class AbstractAnimal {
    public abstract void sound();

    public void move() {
        System.out.println("동물이 움직입니다.");
    }
}
```

```java
public class Cat extends AbstractAnimal{

    @Override
    public void sound() {
        System.out.println("냐옹");
    }
}

```

```java
public class Caw extends AbstractAnimal{

    @Override
    public void sound() {
        System.out.println("음메");
    }
}
```

```java
public class Dog extends AbstractAnimal{

    @Override
    public void sound() {
        System.out.println("멍멍");
    }
}
```

```java
public class AbstractMain {
    public static void main(String[] args) {
        // 추상클래스 생성 불가
//        AbstractAnimal animal = new AbstractAnimal();

        Dog dog = new Dog();
        Cat cat = new Cat();
        Caw caw = new Caw();

        cat.sound();
        cat.move();

        soundAnimal(dog);
        soundAnimal(cat);
        soundAnimal(caw);
    }

    private static void soundAnimal(AbstractAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }

}

```

**결과**

```java
냐옹
동물이 움직입니다.
동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료
동물 소리 테스트 시작
냐옹
동물 소리 테스트 종료
동물 소리 테스트 시작
음메
동물 소리 테스트 종료

Process finished with exit code 0

```

`AbstractAnimal animal = new AbstractAnimal()` : 추상 클래스는 생성이 불가능하다.

![1718475742507]({{site.url}}/images/2024-06-05-java-abstract/1718475742507.png)

- 추상 클래스 덕분에 실수로 `Animal`인스턴스를 생성할 문제를 근본적으로 방지해준다.
- 추상 메서드 덕분에 새로운 동물의 자식 클래스를 만들때 실수로 `sound()`를 오버라이딩 하지 않을 문제를 근본적으로 방지해준다.

## 순수 추상 클래스

모든 메서드가 추상 메서드인 순수 추상 클래스는 코드를 실행할 바디 부분이 전혀 없다.

```java
public abstract class AbstractAnimal {
    public abstract void sound();

    public abstract void move();
}
```

- 인스턴스를 생성할 수 없다.
- 상속시 자식은 모든 메서드를 오버라이딩 해야 한다.
- 주로 다형성을 위해 사용된다.

"상속시 자식은 모든 메서드를 오버라이딩 해야한다."라는 특징은 상속 받는 클래스 입장에서 보면 부모인 모든 메서드를 구현해야 한다. 이런 순수 추상 클래스를 더 편리하게 사용할 수 있도록 인터페이스라는 개념을 제공한다.

&nbsp;

# 인터페이스

```java
public interface InterfaceAnimal {
    /*public abstract */void sound();
    void move();
}

```

- 인터페이스는 `class`가 아니라 `interface`키워드를 사용하면 된다.
- 인터페이스의 메서드는 모두 `public`, `abstract`이다.
- 메서드에 `public abstract`를 생략할 수 있다. (권장)
- 인터페이스는 다중 구현(다중 상속)을 지원한다.

## 멤버 변수

```java
public interface InterfaceAnimal {
	public static final double MY_PI = 3.14;
}
```

- 인터페이스에서 멤버 변수는 `public`, `static`, `final`이 모두 포함되었다고 간주된다. (수정 할 수 없다는 뜻)
- `double MY_PI = 3.14;` 로 생략 가능 (권장)

## 예제

```java
public interface InterfaceAnimal {
    /*public abstract */void sound();
    void move();
}
```

```java
public class Cat implements InterfaceAnimal {
    @Override
    public void sound() {
        System.out.println("냐용");
    }

    @Override
    public void move() {
        System.out.println("개 이동");
    }
}

```

```java
public class Caw implements InterfaceAnimal{
    @Override
    public void sound() {
        System.out.println("음메");
    }

    @Override
    public void move() {
        System.out.println("소 이동");
    }
}
```

```java
public class Dog implements InterfaceAnimal{

    @Override
    public void sound() {
        System.out.println("멍멍");
    }

    @Override
    public void move() {
        System.out.println("개 이동");
    }
}

```

```java
public class InterfaceMain {
    public static void main(String[] args) {
        Cat cat = new Cat();
        Dog dog = new Dog();
        Caw caw = new Caw();

        soundAnimal(cat);
        soundAnimal(dog);
        soundAnimal(caw);
    }

    private static void soundAnimal(InterfaceAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }
}
```

**결과**

```java
동물 소리 테스트 시작
냐용
동물 소리 테스트 종료
동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료
동물 소리 테스트 시작
음메
동물 소리 테스트 종료

Process finished with exit code 0

```

- 인터페이스는 `class`대신에 `interface`로 선언하면 된다.
- `sound()`, `move()`는 앞에 `public abstract`가 생략되어 있다. 따라서 상속 받는 곳에서 모든 메서드를 오버라이딩 해야 한다.

![1718476758368]({{site.url}}/images/2024-06-05-java-abstract/1718476758368.png)

## 상속 vs 구현

부모 클래스의 기능을 자식 클래스가 상속 받을 때, 클래스는 상속 받는다고 표현하지만, 부모 인터페이스의 기능을 자식이 상속 받을 때는 인터페이스를 구현한다고 표현한다.

인터페이스는 모든 메서드가 추상 메서드이다. 따라서 물려받을 수 있는 기능이 없고, 오히려 인터페이스에 정의한 모든 메서드를 자식이 오버라이딩 해서 기능으 구현해야 한다. 따라서 구현한다고 표현한다.

인터페이스는 메서드 이름만 있는 설계도이고, 하위 클래스에서 모두 구현해야한다. 따라서 인터페이스의 경우 상속이 아니라 해당 인터페이스를 구현한다고 표현한다.

## 인터페이스를 사용해야 하는 이유

모든 메서드가 추상 메서드인 경우 순수 추상 클래스를 만들어도 되고, 인터페이스를 만들어도 된다. 그런데 왜 인터페이스를 사용해야 할까?

- 제약 : 인터페이스를 만드는 이유는 인터페이스를 구현하는 곳에서 인터페이스의 메서드를 반드시 구현해라는 규약(제약)을 주는 것이다. 인터페이스의 규약(제약)은 반드시 구현해야 하는 것이다. 그런데 순수 추상 클래스의 경우 미래에 누군가 그곳에 실행 가능한 메서드를 끼워넣을 수 있다. 이렇게 되면 추가된 기능을 자식 클래스에서 구현하지 않을 수도 있고, 또 더는 순수 추상 클래스가 아니게 된다. 인터페이스는 모든 메서드가 추상 메서드이다.
- 다중 구현 : 자바에서 클래스 상속은 부모를 하나만 지정할 수 있다. 반면에 인터페이스는 부모를 여러명 두는 다중 구현(다중 상속)이 가능하다.

## 다중 구현

자바는 다중 상속을 지원하지 않는다. 그래서 `extends`대상은 하나만 선택할 수 있다.

```java
public abstract class AbstractAnimal {
    public abstract void sound();
    public void move() {
        System.out.println("동물이 이동합니다.");
    }
}
```

```java
public interface Fly {
    void Fly();

}

```

```java
public class Dog extends AbstractAnimal{
    @Override
    public void sound() {
        System.out.println("멍멍");
    }
}
```

```java
public class Bird extends AbstractAnimal implements Fly{
    @Override
    public void sound() {
        System.out.println("짹쪽");
    }

    @Override
    public void Fly() {
        System.out.println("새 날기");
    }
}
```

```java
public class Chicken extends AbstractAnimal implements Fly{
    @Override
    public void sound() {
        System.out.println("꼬끼오");
    }

    @Override
    public void Fly() {
        System.out.println("닭 날기");
    }
}

```

```java
public class SoundFlyMain {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Bird bird = new Bird();
        Chicken chicken = new Chicken();

        soundAnimal(dog);
        soundAnimal(bird);
        soundAnimal(chicken);

//        flyAnimal(dog);
        flyAnimal(bird);
        flyAnimal(chicken);
    }

    // AbstractAnimal 사용 가능
    private static void soundAnimal(AbstractAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }

    // Fly 인터페이스가 있으면 사용 가능
    private static void flyAnimal(Fly fly) {
        System.out.println("날기 테스트 시작");
        fly.Fly();
        System.out.println("날기 테스트 종료");
    }
}
```

**결과**

```java
동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료
동물 소리 테스트 시작
짹쪽
동물 소리 테스트 종료
동물 소리 테스트 시작
꼬끼오
동물 소리 테스트 종료
날기 테스트 시작
새 날기
날기 테스트 종료
날기 테스트 시작
닭 날기
날기 테스트 종료

Process finished with exit code 0

```

![1718477899919]({{site.url}}/images/2024-06-05-java-abstract/1718477899919.png)

- `soundAniml(bird)`를 호출한다고 가정하자.
- 메서드 안에서 `animal.sound()`를 호출하면 참조 대상인 `x001` `Bird` 인스턴스를 찾는다.
- 호출한 `animal` 변수는 `AbstractAnimal` 타입이다. 따라서 `AbstractAnimal.sound()`를 찾는다. 해당 메서드는 `Bird.sound()`에 오버라이딩 되어있다.
- `Bird.sound()`가 호출된다.
