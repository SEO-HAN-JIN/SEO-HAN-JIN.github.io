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

![1716394035842](image/2024-05-23-java-final/1716394035842.png)
