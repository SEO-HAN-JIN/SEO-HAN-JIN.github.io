---
title : "[Java] 메모리 구조"
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

자바에서 메모리 구조에 대해서 알아보자.

&nbsp;

# 메모리 구조

![1715526385900]({{site.url}}/images/2024-05-12-java-memory/1715526385900.png)

자바의 메모리 구조는 크게 `메서드 영역`, `스택 영역`, `힙 영역` 3개로 나눌 수 있다.

- `메서드 영역` : 프로그램을 실행하는데 필요한 공통 데이터를 관리한다. 이 영역은 프로그램의 모든 영역에서 공유한다.
  - 클래스 정보 : 클래스의 실행 코드(바이트 코드), 필드, 메서드와 생성자 코드 등 모든 실행 코드가 존재한다.
  - static 영역 : `static` 변수들을 보관한다.
  - 런타임 상수 풀(최적화 영역) : 프로그램을 실행하는데 필요한 공통 릴터럴 상수를 보관한다. 예를 들어서 `"hello"`라는 리터럴 문자가 있으면 이런 문자를 공통으로 묶어서 관리하는 등 프로그램을 효율적으로 관리하기 위한 상수들을 관리한다.
- `스택 영역` : 자바 실행 시, 하나의 실행 스택이 생성된다. 각 스택 프레미은 `지역 변수`, `중간 연산 결과`, `메서드 호출 정보` 등을 포함한다.
  - 스택 프레임 : 스택 영역에 쌓이는 네모 박스가 하나의 스택 프레임이다. 메서드를 호출할 때 마다 하나의 스택 프레임이 쌓이고, 메서드가 종료되면 해당 스택 프레임이 제거된다.
- `힙 영역` : 객체(인스턴스)와 배역이 생성되는 영역이다. 가비지 컬렉션(GC)이 이루어지는 주요 영역이며, 더 이상 참조되지 않는 객체는 GC에 의해 제거된다.

&nbsp;

# 메서드 코드는 메서드 영역에

![1715527219361]({{site.url}}/images/2024-05-12-java-memory/1715527219361.png)

자바에서 특정 클래스로 100개의 인스턴스를 생성하면 `힙 영역`에 100개의 인스턴스가 생긴다. 같은 클래스로 부터 생성된 객체라도 인스턴스 내부에 변수의 값은 서로 다를 수 있지만, 메서드는 공통된 코드를 공유한다. 따라서 객체가 생성될 때 인스턴스 변수에는 메모리가 할당되지만 메서드에 대한 새로운 메모리 할당은 없다. 메서드는 메서드 영역에서 공통을 관리되고 실행된다.

즉, 인스턴스의 메서드를 호출하면 실제로는 메서드 영역에 있는 코드를 불러서 수행한다.

## 코드

아래 코드를 통해 예시를 알아보자.

```java
public class Data {
    private int value;

    public Data(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

```java
public class JavaMemoryMain2 {
    public static void main(String[] args) {
        System.out.println("main start");
        method1();
        System.out.println("main end");
    }

    static void method1() {
        System.out.println("method1 start");
        Data data1 = new Data(10);
        method2(data1);
        System.out.println("method1 end");
    }

    static void method2(Data data2) {
        System.out.println("method2 start");
        System.out.println("data.value=" + data2.getValue());
        System.out.println("method2 end");
    }
}
```

`method1()`에서 `Data data1 = new Data(10)`을 통해 `힙 영역`에 인스턴스가 생성되면서 필드인 `value`도 10으로 할당되었다. 하지만 `Data`클래스의 `getValue()`메서드는 `메서드 영역`에 있는 코드를 불러서 수행한다. 왜냐하면 만약 100개의 `Data`인스턴스가 생성된다면 필드는 각자 다 다른 값을 넣어야 하기 때문에 데이터 메모리를 따로 할당하는게 맞지만 메서드 코드는 인스턴스가 100개든 1개든 똑같이 생겼기 때문에 굳이 메모리 낭비를 할 필요가 없기 떄문이다.

&nbsp;

# 스택 영역과 힙 영역

```java
public class Data {
    private int value;

    public Data(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}

```

```java
public class JavaMemoryMain2 {
    public static void main(String[] args) {
        System.out.println("main start");
        method1();
        System.out.println("main end");
    }

    static void method1() {
        System.out.println("method1 start");
        Data data1 = new Data(10);
        method2(data1);
        System.out.println("method1 end");
    }

    static void method2(Data data2) {
        System.out.println("method2 start");
        System.out.println("data.value=" + data2.getValue());
        System.out.println("method2 end");
    }
}

```

![1715529435399]({{site.url}}/images/2024-05-12-java-memory/1715529435399.png)

- 처음 `main()`메서드를 실행한다. `main()`스택 프레임이 생성된다.

![1715529466258]({{site.url}}/images/2024-05-12-java-memory/1715529466258.png)

- `main()`에서 `method1()`을 실행한다. `method1()`스택 프레임이 생성된다.
- `method1()`은 지역 변수로 `Data data1`을 가지고 있다. 이 지역 변수도 스택 프레임에 포함된다.
- `method1()`은 `new Data(10)`를 사용해서 힙 영역에 `Data`인스턴스를 생성한다. 그리고 참조값을 `data1`에 보관한다.

![1715529496954]({{site.url}}/images/2024-05-12-java-memory/1715529496954.png)

- `method1()`은 `method2()`를 호출하면서 `Data data2`매개변수에 `x001`참조값을 넘긴다.
- 이제 `method1()`에 있는 `data1`과 `method2()`에 있는 `data2` 지역 변수(매개변수 포함)는 둘다 같은 `x001`인스턴스를 참조한다.

![1715529515168]({{site.url}}/images/2024-05-12-java-memory/1715529515168.png)

- `method2()`가 종료된다. `method2()`의 스택 프레임이 제거되면서 매개변수 `data2`도 함께 제거된다.

![1715529527761]({{site.url}}/images/2024-05-12-java-memory/1715529527761.png)

- `method1()`이 종료된다. `method1()`의 스택 프레임이 제거되면서 지역 변수 `data1`도 함께 제거된다.

![1715529541853]({{site.url}}/images/2024-05-12-java-memory/1715529541853.png)

- `method1()`의 스택 프레임이 제거되고 지역 변수 `data1`도 함께 제거되었다.
- 이제 `x001` 참조값을 가진 `Data` 인스턴스를 참조하는 곳이 더는 없다.
- 참조하는 곳이 없으므로 사용되는 곳도 없다. 결과적으로 더는 사용하지 않는 개체인 것이다. 이런 객체는 메모리만 차지하게 된다.
- GC(가비지 컬렉션)은 이렇게 참조가 모두 사라진 인스턴스를 찾아서 메모리에서 제거한다.

**참고** : 힙 영역 외부가 아닌, 힙 영역 안에서만 인스턴스끼리 서로 참조하는 경우에도 GC의 대상이 된다.
