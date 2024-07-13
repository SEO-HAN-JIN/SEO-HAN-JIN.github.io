---
title : "[Java] 래퍼 클래스"
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

래퍼와 클래스에 대해서 알아보자.

&nbsp;

# 래퍼 클래스

# 기본형의 한계

자바는 객체지향 언어이다. 그런데 객체가 아닌 것이 있다. 바로 `int`, `double` 같은 기본형(Primitive Type) 이다.

- **객체가 아님** : 기본형 데이터는 객체가 아니기 때문에, 객체 지향 프로그래밍의 장점인 유용한 메서드(`compareTo`) 등의 기능을 제공할 수 없다.
- **null 값을 가질 수 없음** : 기본형 데이터 타입은 `null` 값을 가질 수 없다. 때로는 데이터가 `없음` 이라는 상태를 나타내야 할 필요가 있는데, 기본형은 항상 값을 가지기 때문에 이런 표현을 할 수 없다.

&nbsp;

# 자바 래퍼 클래스

자바는 기본형에 대응하는 래퍼 클래스를 기본으로 제공한다.

- `byte` > `Byte`
- `short` > `Short`
- `int` > `Integer`
- `long` > `Long`
- `float` > `Float`
- `double` > `Double`
- `char` > `Character`
- `boolean` > `Boolean`

자바가 제공하는 기본 래퍼 클래스는 다음과 같은 특징을 가지고 있다.

- 불변이다.
- `equals`로 비교해야 한다.

```java
public class WrapperClassMain {
    public static void main(String[] args) {
//        Integer newInteger = new Integer(10);       // 미래에 삭제 예정, 대신에 valueOf()를 사용
        Integer newInteger = Integer.valueOf(10);       // x001 스트링 풀 처럼 가져옴
        Integer integerObj = Integer.valueOf(10);       // x001 // -128 ~ 127 자주 사용하는 숫자 값 재사용, 불변
        Long longObj = Long.valueOf(100);
        Double doubleObj = Double.valueOf(10.5);
  
        System.out.println("newInteger = " + newInteger);
        System.out.println("integerObj = " + integerObj);
        System.out.println("longObj = " + longObj);
        System.out.println("doubleObj = " + doubleObj);

        System.out.println("내부 값 읽기");
        int intValue = integerObj.intValue();
        System.out.println("intValue = " + intValue);
        long longValue = longObj.longValue();
        System.out.println("longValue = " + longValue);

        System.out.println("비교");
        System.out.println("==: " + (newInteger == integerObj));
        System.out.println("equals: " + (newInteger.equals(integerObj)));
    }
}
```

**결과**

```java
newInteger = 10
integerObj = 10
longObj = 100
doubleObj = 10.5
내부 값 읽기
intValue = 10
longValue = 100
비교
==: true
equals: true

Process finished with exit code 0

```

- `Integer newInteger = new Integer(10)` : 미래에 삭제 예정, 대신에 `valueOf()`를 사용
- `Integer newInteger = Integer.valueOf(10)` : 기본형에서 래퍼클래스로 변경 시 `Integer.valueOf()`를 사용, 스트링 풀 처럼 자주 사용하는 -128 ~ 127 사이의 숫자 값을 재사용한다. 불변
- `int intValue = IntegerObj.intValue();` : 래퍼클래스에서 기본형으로 변경 시 `.intValue()`사용

&nbsp;

# 박싱, 언박싱

**래퍼 클래스 생성 - 박싱(Boxing)**

- 기본형을 래퍼 클래스로 변경하는 것을 **박싱(Boxing)** 이라 한다.
- `new Integer(10)`은 직접 사용하면 안된다. 향수 제거 예정
- 대신에 `Integer.valueOf(10)`를 사용
  - 내부에서 `new Integer(10)`을 사용해서 객체를 생성하고 돌려준다.
- `Integer.valueOf()`에는 성능 최적화 기능이 있다. 개발자들이 일반적으로 자주 사용하는 -128 ~ 127 범위의 `Integer` 클래스를 미리 생성해준다. 해당 범위의 값을 조회하면 미리 생성된 `Integer`객체를 반환한다. 해당 범위의 값이 없으면 `new Integer()`를 호출한다.
  - 마치 문자열 풀과 비슷하게 자주 사용하는 숫자를 미리 생성해두고 재사용한다.

**intValue() - 언박싱(Unboxing)**

- 래퍼 클래스에 들어있는 기본형 값을 다시 꺼내는 메서드이다.

**비교는 equals() 사용**

- 래퍼 클래스는 객체이기 때문에 `==`비교를 하면 인스턴스의 참조값을 비교한다.
- 래퍼 클래스는 내부의 값을 비교하도록 `equals()`를 재정의해두었다. 따라서 값을 비교하려면 `equals()`를 사용해야 한다.

&nbsp;

# 오토 박싱, 오토 언박싱

- 박싱 : `valueOf()`
- 언박싱 : `xxxValue()` (ex `intValue()`, `doubleValue()`)

```java
public class AutoboxingMain2 {
    public static void main(String[] args) {
        // Primitive -> Wrapper
        int value = 7;
        Integer boxedValue = value; // 오토 박싱(Auto-boxing)

        // Wrapper -> Primitive
        int unboxedValue = boxedValue;  // 오토 언박싱(Auto-Unboxing)

        System.out.println("boxedValue = " + boxedValue);
        System.out.println("unboxedValue = " + unboxedValue);

    }
}
```

**결과**

```java
boxedValue = 7
unboxedValue = 7

Process finished with exit code 0
```

&nbsp;

# 주요 메서드와 성능

## 1. 메서드

```java
public class WrapperUtilsMain {
    public static void main(String[] args) {
        Integer i1 = Integer.valueOf(10);// 숫자, 래퍼 객체 변환
        Integer i2 = Integer.valueOf("10");// 문자열, 래퍼 객체 변환
        int intValue = Integer.parseInt("10");// 문자열 전용, 기본형 변환

        // 비교
        int compareResult = i1.compareTo(20);
        System.out.println("compareResult = " + compareResult);

        // 산술 연산
        System.out.println("sum: " + Integer.sum(10, 20));
        System.out.println("min: " + Integer.min(10, 20));
        System.out.println("max: " + Integer.max(10, 20));
    }
}
```

**결과**

```java
compareResult = -1
sum: 30
min: 10
max: 20
Disconnected from the target VM, address: '127.0.0.1:64114', transport: 'socket'

Process finished with exit code 0

```

- `valueOf()` : 래퍼 타입을 반환한다. 숫자, 문자열 모두 지원한다.
- `parseInt()` : 문자열을 기본형으로 제공한다.
- `compareTo()` : 내 값과 인수로 넘어온 값을 비교한다. 내 값이 크면 `1`, 같으면 `0`, 내 값이 작으면 `-1` 을 반환한다.
- `Integer.sum()`, `Integer.min()`, `Integer.max()`, : `static` 메서드이다.

&nbsp;

**parseInt() vs valueOf()**

- `valueOf("10")` : 래퍼 타입을 반환한다.
- `parseInt("10")` : 기본형을 반환한다.

## 2. 성능

```java
public class WrapperVsPrimitive {
    public static void main(String[] args) {
        int iterations = 1_000_000_000; // 반복 횟수 설정, 10억
        long startTime, endTime;

        // 기본형 long 사용
        long sumPrimitive = 0;
        startTime = System.currentTimeMillis();
        for (int i = 0; i < iterations; i++) {
            sumPrimitive += i;
        }

        endTime = System.currentTimeMillis();
        System.out.println("sumPrimitive = " + sumPrimitive);
        System.out.println("기본 자료형 long 실행 시간: " + (endTime - startTime) + "ms");

        // 래퍼 클래스 Long 사용
        Long sumWrapper = 0L;
        startTime = System.currentTimeMillis();
        for (int i = 0; i < iterations; i++) {
            sumWrapper += i;    // 오토 박싱 발생
        }

        endTime = System.currentTimeMillis();
        System.out.println("sumWrapper = " + sumWrapper);
        System.out.println("래퍼 클래스 Long 실행 시간: " + (endTime - startTime) + "ms");
    }
}
```

**결과**

```java
sumPrimitive = 499999999500000000
기본 자료형 long 실행 시간: 387ms
sumWrapper = 499999999500000000
래퍼 클래스 Long 실행 시간: 4781ms

Process finished with exit code 0
```

- 기본형 연산이 래퍼 클래스보다 훨씬 빠른 것을 알 수 있다.
- 기본형은 메모리에서 단순히 그 크기만큼의 공간을 차지한다. 예를 들어 `int`는 보통 4바이트의 메모리를 사용한다.
- 래퍼 클래스의 인스턴스는 내부에서 필드로 가지고 있는 기본형의 값 뿐만 아니라 자바에서 객체를 다루는데 필요한 객체 메타데이터를 포함하므로 더 많은 메모리를 사용한다.
