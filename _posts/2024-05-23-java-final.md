---
title : "[Java] final"
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

`final`에 대해서 알아보자.

&nbsp;

# ❓ `final`이란

`final` 키워드는 이름 그대로 끝! 이라는 뜻이다. 변수에 `final` 키워드가 붙으면 더는 값을 변경할 수 없다.

&nbsp;

# `final`은 최초 한번만 할당이 가능하다.

## 1. 지역 변수의 경우

```java
public class FinalLocalMain {
    public static void main(String[] args) {
        // final 지역 변수
        final int data1;
        data1 = 10; // 최초 한번만 할당 가능
//        data1 = 20; // 컴파일 오류

        // final 지역 변수2
        final int data2 = 10;
//        data2 = 20; // 컴파일 오류
        method(10);
    }

    static void method(final int parameter) {
//        parameter = 20; // 컴파일 오류
    }
}
```

- `final`을 지역변수에 설정할 경우 최초 한번만 할당할 수 있다. 이후에 변수의 값을 변경하려면 컴파일 오류가 발생한다.
- `final`을 지역 변수 선언시 바로 초기화 한 경우 이미 값이 할당되었기 때문에 값을 할당할 수 없다.
- 매개변수에 `final`이 붙으면 매서드 내부에서 매개변수의 값을 변경할 수 없다. 따라서 메서드 호출 시점에 사용된 값이 끝까지 사용된다.

## 2. 멤버 변수인 경우

```java
public class ConstructInit {

    final int value;

    public ConstructInit(int value) {
        /*
            final을 이 필드에 사용할 경우에는 생성자를 통해서 한번만 초기화를 할 수 있다.
         */
        this.value = value;
    }

}
```

- `final`을 필드에 사용할 경우 해당 필드는 생성자를 통해서 한번만 초기화 될 수 있다.

```java
public class FieldInit {
    /*
        static final 이 붙은 변수를 `상수`라 한다.
     */
    static final int CONST_VALUE = 10;
    final int value = 10;

//    public FieldInit(int value) {
        /*
            초기값이 있는 경우 안된다.
         */
//        this.value = value;
//    }
}
```

```java
public class FinalFieldMain {
    public static void main(String[] args) {
        // final 필드 - 생성자 초기화
        /*
            ConstructInit과 같이 생성자를 사용해서 final 필드를 초기화 하는 경우, 각 인스턴스마다 final 필드에 다른 값을 할당할 수 있다.
            물론 final을 사용했기 때문에 생성 이후에 이 값을 변경하는 것은 불가능하다.
         */
        System.out.println("생성자 초기화");
        ConstructInit constructInit1 = new ConstructInit(10);
        ConstructInit constructInit2 = new ConstructInit(20);
        System.out.println(constructInit1.value);
        System.out.println(constructInit2.value);

        // final 필드 - 필드 초기화
        /*
            - FieldInit과 같이 final 필드를 필드에서 초기화 하는경우, 모든 인스턴스가 아래 그림과 같이 같은 값을 가진다.
            - 여기서 FieldInit 인스턴스의 모든 value 값은 10이 된다.
            - 왜냐하면 생성자 초기화와 다르게 필드 초기화는 필드의 코드에 해당 값이 미리 정해져있기 때문이다.
            - 모든 인스턴스가 같은 값을 사용하기 때문에 결과적으로 메모리를 낭비하게 된다.(물론 JVM에 따라서 내부 최적화를 시도할 수 있다.)
                또 메모리 낭비를 떠나서 같은 값이 계속 생성되는 것은 개발자가 보기에 명확한 중복이다. 이럴 때 사용하는 좋은 것이 바로 static 영역이다.
         */
        System.out.println("필드 초기화");
        FieldInit fieldInit1 = new FieldInit();
        FieldInit fieldInit2 = new FieldInit();
        FieldInit fieldInit3 = new FieldInit();
        System.out.println(fieldInit1.value);
        System.out.println(fieldInit2.value);
        System.out.println(fieldInit3.value);

        // 상수
        /*
            static final
            - FieldInit.MY_VALUE 는 static 영역에 존재한다. 그리고 final 키워드를 사용해서 초기화 값이 변하지 않는다.
            - static 영역은 단 하나만 존재하는 영역이다. MY_VALUE 변수는 JVM 상에서 하나만 존재하므로 앞서 설명한 중복과 메모리 비효율 문제를 모두 해결할 수 있다.
            이런 이유로 필드에 final + 필드 초기화를 사용하는 경우 static을 붙여서 사용하는 것이 효과적이다.
         */
        System.out.println("상수");
        System.out.println(FieldInit.CONST_VALUE);
    }
}
```

![1716394035842]({{site.url}}/images/2024-05-23-java-final/1716394035842.png)

&nbsp;

# 상수

상수는 변하지 않고, 항상 일정한 값을 갖는 수를 말한다. 자바에서는 보통 단 하나만 존재하는 변하지 않는 고정된 값을 상수라 한다. 이렇나 이유로 상수는 `static final`키워드를 사용한다.

## 자바 상수 특징

- `static final`키워드를 사용한다.
- 대문자를 사용하고 구분은 `_`(언더스코어)라 한다. (관례)
  - 일반적인 변수와 상수를 구분하기 위해
- 필드를 직접 접근해서 사용한다.
  - 상수는 기능이 아니라 고정된 값 자체를 사용하는 것이 목적이다.
  - 상수는 값을 변경할 수 없다. 따라서 필드에 직접 접근해도 데이터가 변하는 문제가 발생하지 않는다.

```java
public class Constant {
    // 수학 상수
    public static final double PI = 3.14;

    // 시간 상수
    public static final int HOURS_IN_DAYS = 24;
    public static final int MINUTES_IN_HOURS = 60;
    public static final int SECONDS_IN_MINUTES = 60;

    // 애플리케이션 설정 상수
    public static final int MAX_USERS = 1000;
}
```

- 애플리케이션 안에는 다양한 상수가 존재할 수 있다. 수학, 시간 등등 실생활에서 사용하는 상수부터, 애플리케이션의 다양한 설정을 위한 상수들도 있다.
- 보통 이런 상수들은 애플리케이션 전반에서 사용되기 때문에 public 를 자주 사용한다. 물론 특정 위치에서만 사용된다면 다른 접근 제어자를 사용하면 된다.
- 상수는 중앙에서 값을 하나로 관리할 수 있다는 장점도 있다.
- 상수는 런타임에 변경할 수 없다. 상수를 변경하려면 프로그램을 종료하고, 코드를 변경한 다음에 프로그램을 다시 실행해야 한다.

## 예제

```java
public class ConstantMain1 {
    public static void main(String[] args) {
        System.out.println("프로그램 최대 참여자 수 : " + 1000);
        int currentUserCount = 999;
        process(currentUserCount++);
        process(currentUserCount++);
        process(currentUserCount++);
    }

    private static void process(int currentUserCount) {
        System.out.println("참여자 수 :" + currentUserCount);
        if (currentUserCount > 1000) {
            System.out.println("대기자로 등록합니다.");
        } else {
            System.out.println("게임에 참여합니다.");
        }
    }
}
```

**결과**

```java
프로그램 최대 참여자 수 : 1000
참여자 수 :999
게임에 참여합니다.
참여자 수 :1000
게임에 참여합니다.
참여자 수 :1001
대기자로 등록합니다.

Process finished with exit code 0

```

이 예제에서는 문제가 몇가지 있다. 첫번째로 `프로그램 최대 참여자 수` 가 1000명이 아니라 2000명으로 바꿔야 한다면 수정해야하는 부분이 많아진다. 두번째로는 개발자 입장에서는 숫자 1000을 보고 이게 무엇을 의미하는지 파악하기 힘들다는 것이다.

### 개선

```java
public class ConstantMain1 {
    public static void main(String[] args) {
        System.out.println("프로그램 최대 참여자 수 : " + Constant.MAX_USERS);
        int currentUserCount = 999;
        process(currentUserCount++);
        process(currentUserCount++);
        process(currentUserCount++);
    }

    private static void process(int currentUserCount) {
        System.out.println("참여자 수 :" + currentUserCount);
        if (currentUserCount > Constant.MAX_USERS) {
            System.out.println("대기자로 등록합니다.");
        } else {
            System.out.println("게임에 참여합니다.");
        }
    }
}
```

1000을 `Constant.MAX_USERS`로 변경하는 것이다. 이렇게 하면 상수가 중앙에서 일관되게 관리할 수 있다는 장점이 있다.

&nbsp;

# 변수와 참조

`final`로 선언한 변수와 참조 대상의 값을 확인해보자.

## 예제

```java
public class Data {
    public int value;
}

```

`final`이 아닌 일반 변수가 있는 클래스가 있다.

```java
public class FinalRefMain {
    public static void main(String[] args) {
        final Data data = new Data();
//        data = new Data();    // 에러!! 참조값을 더 이상 바꿀 수 없음

        // 참조 대상의 값은 변경 가능
        data.value = 10;
        System.out.println(data.value);
        data.value = 20;
        System.out.println(data.value);
    }
}
```

`final`을 이용해서 참조형 변수 `Data`를 선언했다. `final`로 선언했기 때문에 한번만 선언이 가능하며 더 이상 선언하려고 하면 컴파일 에러가 발생한다. 하지만 참조대상의 값은 변경이 가능하다. 

- 참조형 변수 `data`에 `final`이 붙었다. 이 경우 참조형 변수에 들어있는 참조값을 다른 값으로 변경하지 못한다. 쉽게 이야기해서 이제 다른 객체를 참조할 수 없다. 그런데 이것의 정확한 뜻을 잘 이해해야 한다. 참조형 변수에 들어있는 참조값만 변경하지 못한다는 뜻이다. 이 변수 이외에 다른 곳에 영향을 주는 것이 아니다.
- `Data.value`는 `final`이 아니다. 따라서 값을 변경할 수 있다.

즉, 참조형 변수 `final`이 붙으면 참조 대상 자체를 다른 대상으로 변경하지 못하는 것이지, 참조하는 대상의 값은 변경할 수 있다.
