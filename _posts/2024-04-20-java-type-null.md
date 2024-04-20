---
title: "[Java] 변수와 초기화"
---
# 개요

자바에서 변수의 초기화와 null을 선언할 때의 경우를 확인해 보자.


# 변수와 초기화

멤버 변수 : 자동 초기화

- 인스턴스의 멤버 변수는 인스턴스 생성할 때 자동으로 초기화된다.
- 숫자(int) = 0 , boolean = false, 참조형 = null (null 값은 참조할 대상이 없다는 뜻으로 사용된다.)
- 개발자가 초기값을 직접 지정할 수 있다.

지역 변수 : 수동 초기화

- 지역 변수는 항상 직접 초기화해야 한다.


아래 코드와 같이 초기화 하지 않은 변수와, 10으로 초기화한 변수 2개를 선언한다.

```java
public class InitData {
    int value1; 	// 초기화 하지 않음
    int value2 = 10;	// 10으로 초기화

}
```

new로 생성하는 것들은 java가 인스턴스를 만들면서 값들을 초기화 해준다.

```java
    public static void main(String[] args) {
        InitData data = new InitData();
        System.out.println("value1 = " + data.value1);
        System.out.println("value2 = " + data.value2);
    }

```

결과

```java
value1 = 0
value2 = 10
```

`value1`을 초기화 하지 않아도 인스턴스 생성 시 자동으로 0으로 초기화가 된다.


# null

아래 코드를 보면 `data`에 인스턴스를 생성 후 다시 `data `에 `null` 을 할당 했다. 따라서 앞서 생성한 `Data `인스턴스를 더는 아무도 참조하지 않는다. 이렇게 아무도 참조하지 않게 되면 기존의 참조값을 다시 구할 방법이 없다. 따라서 해당 인스턴스에 다시 접근할 방법이 없다. 이렇게 아무도 참조하지 않는 인스턴스는 사용되지 않고 메모리 용량만 차지할 뿐이다.

```java
    public static void main(String[] args) {
        Data data = null;
        System.out.println("1. data = " + data);
        data = new Data();
        System.out.println("2. data = " + data);
        data = null;
        System.out.println("3. data = " + data);
    }
```

결과

```java
1. data = null
2. data = ref.Data@404b9385
3. data = null
```


> **GC - 아무도 참조하지 않는 인스턴스의 최후**
>
> C와 같은 과거 프로그래밍 언어는 개발자가 직접 명령어를 사용해서 인스턴스를 메모리에서 제거해야 했다. 만약 실수로 인스턴스 삭제를 누락하면 메모리에 사용하지 않는 객체가 가득해져서 메모리 부족 오류가 발생하게 된다.
>
> 자바는 이런 과정을 자동으로 처리해준다. 아무도 참조하지 않는 인스턴스가 있으면 JVM의 GC(가비지 컬렉션)가 더이상 사용하지 않는 인스턴스라 판단하고 해당 인스턴스를 자동으로 메모리에서 제거해준다.
>
> 객체는 해당 객체를 참조하는 곳이 있으면, JVM이 종료할 때 까지 계속 생존한다. 그런데 중간에 해당 객체를 참조하는 곳이 모두 사라지면 그때 JVM은 필요없는 객체로 판단하고 GC(가비지 컬렉션)을 사용해서 제거한다.
