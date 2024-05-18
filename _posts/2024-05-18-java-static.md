---
title : "[Java] static"
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

자바에서 static에 대해서 알아보자.

&nbsp;

# `static` 변수

멤버 변수에  `static` 키워드가 왜 필요한지 예제를 통해서 알아보자.

## 예제1

```java
public class Data1 {
    public String name;
    public int count;

    public Data1(String name) {
        this.name = name;
        count++;
    }
}
```

```java
public class DataCountMain1 {
    public static void main(String[] args) {
        Data1 data1 = new Data1("A");
        System.out.println("A count=" + data1.count);

        Data1 data2 = new Data1("B");
        System.out.println("B count=" + data2.count);

        Data1 data3 = new Data1("C");
        System.out.println("C count=" + data3.count);
    }
}
```

**결과**

```java
A count=1
B count=1
C count=1
```

결과에서 보면 알듯이 인스턴스에 사용되는 멤버 변수 `count`값은 인스턴스끼리 서로 공유되지 않는다. 따라서 이 문제를 해결하려면 변수를 서로 공유해야 한다.

![1716025459309]({{site.url}}/images/2024-05-18-java-static/1716025459309.png)

## 예제2

```java
public class Data3 {
    public String name;   
    public static int count;  

    public Data3(String name) {
        this.name = name;
        count++;
    }
}
```

```java
public class DataCountMain3 {
    public static void main(String[] args) {

        Data3 data1 = new Data3("A");
        System.out.println("A count=" + Data3.count); // Data3.count : 클래스에 직접 접근하는 느낌이다.

        Data3 data2 = new Data3("B");
        System.out.println("B count=" + Data3.count);

        Data3 data3 = new Data3("C");
        System.out.println("C count=" + Data3.count);

        // 인스턴스를 통한 접근 - 비추천 (이유 : 코드를 읽을 때 마치 인스턴스 변수에 접근하는 것 처럼 오해할 수 있기 때문)
        Data3 data4 = new Data3("D");
        System.out.println(data4.count);

        // 클래스를 통한 접근 - 추천 (이유 : 정적 변수는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확하다. 따라서 정적 변수에 접근할 떄는 클래스를 통해 접근하자)
        System.out.println(Data3.count);
    }
}
```

**결과**

```java
A count=1
B count=2
C count=3
4
4
```

- `public String name` (인스턴수 변수) : 인스턴스를 생성해야 만들어지기 때문이다.
- `public static int count` (클래스 변수 or 정적 변수 or static 변수) : `static`이 붙은 변수, 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고 클래스 자체에 소속되어 있다. 클래스 변수는 자바 프로그램을 시작할 때 딱 1개가 만들어진다. 인스턴스와는 다르게 보통 여러개 공유하는 목적으로 사용된다.
- 변수와 생명주기
  - 지역 변수(매개변수 포함) : 지역 변수는 스택 영역에 있느 스택 프레임 안에 보관된다. 메서드가 종료되면 스택 프레임도 제거되는데 이때 해당 스택 프레임에 포함된 지역변수도 함께 제거된다.
  - 인스턴스 변수 : 인스턴스에 있는 멤버 변수를 인스턴스 변수라 한다. 인스턴스 변수는 힙 영역을 사용한다. 힙 영역은 GC(가비지 컬렉션)가 발생하기 전까지는 생존하기 때문에 보통 지역변수보다 생존 주기가 길다.
  - 클래스 변수 : 클래스 변수는 메서드 영역의 `static` 영역에 보관되는 변수이다. 메서드 영역은 프로그램 전체에서 사용되는 공용 공간이다. 클래스 변수는 해당 클래스가 `JVM`에 로딩 되는 순간 생성된다. 그리고 `JVM`이 종료될 때 까지 생명주기가 이어진다. 따라서 가장 긴 생명주기를 가진다.
- `static`이 정적이라는 이유는 힙 영역에 생성되는 인스턴스 변수는 동적으로 생성되고 제거된다. 반면에 `static`인 정적 변수는 거의 프로그램 실행 시점에 딱 만들어지고, 프로그램 종료 시점에 제거된다.

`Data3` 클래스에서 생성자에는 `count++`코드가 있다. `count`는 `static`이 붙은 정적 변수다. 정적 변수는 인스턴스 영역이 아니라 메서드 영역에서 관리한다. 따라서 이 경우 메서드 영역에 있는 `count`의 값이 하나 증가된다.

![1716026790031]({{site.url}}/images/2024-05-18-java-static/1716026790031.png)

&nbsp;

# `static` 메서드

예제를 통해서 `static`메서드를 알아보자.

## 예제1

```java
public class DecoUtils1 {

    public String deco(String str) {
        return "*" + str + "*";
    }
}
```

```java
public class DecoMain1 {

    public static void main(String[] args) {
        String s = "hello java";
        DecoUtils1 decoUtils1 = new DecoUtils1();
        String deco = decoUtils1.deco(s);

        System.out.println("before: " + s);
        System.out.println("after : " + deco);
    }
}
```

**결과**

```java
before: hello java
after : *hello java*
```

`deco()` 메서드를 호출하기 위해서는 `DecoUtil`의 인스턴스를 먼저 생성해야한다. 그런데 `deco()` 라는 기능은 멤버 변수도 없고, 단순히 기능만 제공할 뿐이다. 인스턴스가 필요한 이유는 변수(인스턴스 변수)등을 사용하는 목적이 큰데, 이 메서드는 사용하는 인스턴스 변수도 없고 단순히 기능만 제공할 뿐이다.

## 예제2

```java
public class DecoUtils2 {

    public static String deco(String str) {
        return "*" + str + "*";
    }
}
```

```java
public class DecoMain2 {

    public static void main(String[] args) {
        String s = "hello java";
        String deco = DecoUtils2.deco(s);

        System.out.println("before: " + s);
        System.out.println("after : " + deco);
    }
}
```

**결과**

```java
before: hello java
after : *hello java*

```

- 정적 메서드 : 인스턴스 생성 없이 클래스 명을 통해서 바로 호출할 수 있다.
- 클래스 메서드 : 메서드 앞에서 `static`을 붙일 수 있다. 이것을 `정적 메서드` 또는 `클래스 메서드`라 한다. 정적 메서드라는 용어는 `static` 이 정적이라는 뜻이기 때문이고 `클래스 메서드`라는 용어는 인스턴스 생성 없이 마치 클래스에 있는 메서드를 바로 호출하는 것 처럼 느껴지기 때문이다.
- 인스턴스 메서드 : `static`이 붙지 않은 메서드는 인스턴스를 생성해야 호출할 수 있다. 이것을 인스턴스 메서드라 한다.

`DecoMain2`클래스에서 볼 수 있듯이 정적 메서드 덕분에 불필요한 객체 생성 없이 편리하게 메서드를 사용할 수 있다.

## 예제3

```java
public class DecoData {
    private int instanceValue;
    private static int staticValue;

    public static void staticCall() {
//        instanceValue++;    // 인스턴스 변수 접근, compile error
//        instanceMethod();   // 인스턴스 메서드 접근, compile error

        staticValue++; // 정적 변수 접근
        staticMethod();
    }

    public static void staticCall(DecoData data) {
        data.instanceValue++;
        data.instanceMethod();
    }

    public void instanceCall() {
        instanceValue++;    // 인스턴스 변수 접근, compile error
        instanceMethod();   // 인스턴스 메서드 접근, compile error

        staticValue++; 	    // 정적 변수 접근
        staticMethod();
    }

    private void instanceMethod() {
        System.out.println("instanceValue=" + instanceValue);
    }

    private static void staticMethod() {
        System.out.println("staticVAlue=" + staticValue);
    }
}
```

```java
public class DecoDataMain {

    public static void main(String[] args) {
        System.out.println("1. 정적 호출");
        DecoData.staticCall();

        System.out.println("2. 인스턴스 호출1");
        DecoData data1 = new DecoData();
        data1.instanceCall();

        System.out.println("3. 인스턴스 호출2");
        DecoData data2 = new DecoData();
        data2.instanceCall();

        DecoData.staticCall(data1);
    }
}
```

**결과**

```java
1. 정적 호출
staticVAlue=1
2. 인스턴스 호출1
instanceValue=1
staticVAlue=2
3. 인스턴스 호출2
instanceValue=1
staticVAlue=3
instanceValue=2

Process finished with exit code 0

```

`DecoData`클래스에서 `staticCall()`메서드를 보면 정적 메서드가 인스턴스의 기능을 사용할 수가 없다. 그 이유는 정적 메서드는 클래스의 이름을 통해 바로 호출할 수 있다. 그래서 인스턴스 처럼 참조값의 개념이 없다. 특정 인스턴스의 기능을 사용하려면 참조값을 알아야 하는데, 정적 메서드는 참조값 없이 호출한다. 따라서 정적 메서드 내부에서 인스턴스 변수나 인스턴스 메서드를 사용할 수 없다.

![1716027010506]({{site.url}}/images/2024-05-18-java-static/1716027010506.png)
