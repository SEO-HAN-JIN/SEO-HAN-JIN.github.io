---
title : "[Java] String클래스"
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

자바에서 문자를 다루는 대표적인 타입은 `char`, `String` 2가지가 있다.

기본형인 `char`는 문자 하나를 다룰 때 사용한다. `char`를 사용해서 여러 문자를 나열하려면 `char[]`을 사용해야 한다. 하지만 `char[]`을 직접 다루는 방법은 매우 불편하기 때문에 자바는 문자열을 매우 편리하게 다룰 수 있는 `String`클래스를 제공한다.

`String` 클래스를 통해 문자열을 생성하는 방법은 2가지가 있다.

```java
String str1 = "hello";
String str2 = new String("hello");
```

- 쌍따옴표 사용: `"hello"`
- 객체 생성: `new String("hello");`

`String`은 클래스이다. `int`, `boolean` 같은 기본형이 아니라 참조형이다. 따라서 `str1`변수에는 `String` 인스턴스의 참조값만 들어갈 수 있다. 따라서 `String str1 = "hello"`과 같은 코드는 뭔가 어색하다.

문자열은 매우 자주 사용된다. 그래서 편의상 쌍다옴표로 문자열을 감싸면 자바 언어에서 `new String("hello")`와 같이 변경해준다.

```java
String str = "hello";	// 기존
String str = new String("hello"); // 변경
```

&nbsp;

# String 클래스 구조

`String` 클래스는 대략 다음과 같이 생겼다.

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence,
               Constable, ConstantDesc {

    @Stable
    private final byte[] value;

    ...
}
```

## 속성(필드)

```java
private final char[] value; // 자바 9 이전
private final byte[] value; // 자바 9 이후
```

여기에는 `String`의 실제 문자열 값이 보관된다. 문자 데이터 자체는 `char[]`에 보관된다.

> 자바 9부터 `String` 클래스에서 `char[]` 대신에 `byte[]`을 사용한다. 자바에서 문자 하나를 표현하는 `char`는 `2byte`를 차지한다. 그런데 영어, 숫자는 보통 `1byte`로 표현이 가능하다. 다순히 영어, 숫자로만 표현된 경우 `1byte`를 사용하고, 그렇지 않은 나머지의 경우 `2byte`인 UTF-16 인코딩을 사용한다. 따라서 메모리를 더 효율적으로 사용할 수 있게 변경되었다.

## 기능(메서드)

- `length()` : 문자열의 길이를 반환한다.
- `charAt(int index)` : 특정 인덱스의 문자를 반환한다.
- `substring(int beginIndex, int endIndex)` : 문자열의 부분 문자열을 반환한다.
- `indexOf(String str)` : 특정 문자열이 시작되는 인덱스를 반환한다.
- `toLowerCase()`, `toUpperCase()` : 문자열을 소문자 또는 대문자로 변환한다.
- `trim()`: 문자열 양 끝의 공백을 제거한다.
- `concat(String str)`: 문자열을 더한다.

# String 클래스와 참조형

`String`은 클래스이다.  따라서 기본형이 아니라 참조형이다. 참조형은 계산할 수 없는 참조값이 들어있다. 따라서 원칙적으로 `+` 같은 연산을 사용할 수 없다.

```java
public class StringConcatMain {
    public static void main(String[] args) {
        String a = "hello"; // x001
        String b = " java"; // x002

        String result1 = a.concat(b);   // x001.concat(x002)
        String result2 = a + b;         // x001 + x002

        System.out.println("result1 = " + result1);
        System.out.println("result2 = " + result2);

    }
}
```

**결과**

```java
result1 = hello java
result2 = hello java

Process finished with exit code 0
```

- 자바에서 문자열을 더할 때는 `String`이 제공하는 `concat()`과 같은 메서드를 사용해야 한다.
- 하지만 문자열은 자주 다루기 때문에 자바 언어에서 편의상 `+`연산을 제공한다.

# String 클래스 - 비교

`String` 클래스를 비교할 때는 `==` 비교가 아니라 항상 `equals()` 비교를 해야 한다.

- **동일성(Identity)**:  `==`연산자를 사용해서 두 객체의 참조가 동일한 객체를 가리키고 있는지 확인
- **동등성(Equality)**: `equals()`메서드를 사용하여 두 객체가 논리적으로 같은지 확인

```java
public class StringEqualsMain1 {
    public static void main(String[] args) {
        String str1 = new String("hello");  // x001
        String str2 = new String("hello");  // x002
        System.out.println("new String() == 비교 " + (str1 == str2));
        System.out.println("new String() equals 비교: " + (str1.equals(str2)));

        String str3 = "hello";  // x003
        String str4 = "hello";  // x004
        System.out.println("리터럴 == 비교: " + (str3 == str4));
        System.out.println("리터럴 equals 비교: "  + (str3.equals(str4)));
    }
}
```

**결과**

```java
new String() == 비교 false
new String() equals 비교: true
리터럴 == 비교: true
리터럴 equals 비교: true

Process finished with exit code 0
```

- `str1` 과 `str2`는 `new String()` 을 사용해서 각각 인스턴스를 생성했다. 서로 다른 인스턴스 이므로 동일성(`==`) 비교에 실패한다.
- 둘은 내부에 같은 `"hello"` 값을 가지고 있기 때문에 논리적으로 같다. 따라서 동등성(`equals()`) 비교에 성공한다.
- 리터럴 `==` 비교에서 `true` 인 이유가 뭘까? 아래 `문자열 풀`에서 나온다.

> **equals() 재정의**
>
> `String` 클래스는 내부 문자열 값을 비교하도록 `equals()` 메서드를 제정의 해두었다.
>
> ```java
> Object
>     public boolean equals(Object obj) {
>         return (this == obj);
>     }
> ======================================================
> String
>     public boolean equals(Object anObject) {
>         if (this == anObject) {
>             return true;
>         }
>         return (anObject instanceof String aString)
>                 && (!COMPACT_STRINGS || this.coder == aString.coder)
>                 && StringLatin1.equals(value, aString.value);
>     }
> ```

# 문자열 풀(String Pool)

![1719673823237]({{site.url}}/images/2024-06-29-java-string/1719673823237.png)

- `String str3 = "hello"` 와 같이 문자열 리터럴을 사용하는 경우 자바는 메모리 효율성과 성능 최적화를 위해 문자열 풀을 사용한다.
- 자바가 실행되는 시점에 클래스에 문자열 리터럴이 있으면 문자열 풀에 `String`인스턴스를 미리 만들어둔다. 이때 같은 문자열이 있으면 만들지 않는다.
- `String str3 = "hello"`와 같이 문자열 리터럴을 사용하면 문자열 풀에서 `"hello"`라는 문자를 가진 `String` 인스턴스를 찾는다. 그리고 찾은 인스턴스의 참조 (`x003`)를 반환한다.
- `String str4 = "hello"`의 경우 `"hello"`문자열 리터럴을 사용하므로 문자열 풀에서 `str3`과 같은 `x003`참조를 사용한다.
- 문자열 풀 덕분에 같은 문자를 사용하는 경우 메모리 사용을 줄이고 문자를 만드는 시간도 줄어들기 때문에 성능도 최적화 할 수 있다.

따라서 문자열 리터럴을 사용하는 경우 같은 참조값을 가지므로 `==`비교에 성공한다.

> 참고로 문자열 풀은 힙 영역을 사용한다. 그리고 문자열 풀에서 문자를 찾을 때는 해시 알고리즘을 사용하기 때문에 매우 빠른 속도로 원하는 `String` 인스턴스를 찾을 수 있다.

&nbsp;

# String클래스 - 불변객체

`String`은 불변 객체이다. 따라서 생성 이후에 절대로 내부의 문자열 값은 변경할 수 없다.

## 예시1

```java
public class StringImmutable1 {
    public static void main(String[] args) {
        String str = "hello";
        str.concat("java");
        System.out.println("str = " + str);
    }
}
```

**결과**

```java
str = hello

Process finished with exit code 0

```

- `String.concat()` 메서드를 사용하면 기존 문자열에 새로운 문자열을 연결해서 합칠 수 있다.
- 실행 결과를 보면 문자가 합쳐지지 않았다.

## 예시2

```java
public class StringImmutable2 {
    public static void main(String[] args) {
        String str1 = "hello";
        String str2 = str1.concat("java");
        System.out.println("str1 = " + str1);
        System.out.println("str2 = " + str2);
    }
}

```

**결과**

```java
str1 = hello
str2 = hellojava

Process finished with exit code 0
```

- `String`은 불변 객체이다. 따라서 변경이 필요한 경우 기존 값을 변경하지 않고, 대신에 새로운 결과를 만들어서 반환한다.

![1719675201616]({{site.url}}/images/2024-06-29-java-string/1719675201616.png)

- `String.concat()`은 내부에서 새로운 `String` 객체를 만들어서 반환한다. 따라서 불변과 기존 객체의 값을 유지한다.

## String이 불변으로 설계된 이유

문자열 풀에 있는 `String` 인스턴스의 값이 중간에 변경되면 같은 문자열을 참고하는 다른 변수의 값도 함께 변경된다.

![1719673823237]({{site.url}}/images/2024-06-29-java-string/1719673823237.png)

예를들어

- `String`은 자바 내부에서 문자열 풀을 통해 최적화 한다.
- 만약 `String` 내부의 값을 변경할 수 있다면, 기존에 문자열 풀에서 같은 문자를 참조하는 변수의 모든 문자가 함께 변경되어 버리는 문제가 발생한다.

# StringBuilder - 가변 String

불변인 `String` 클래스에도 단점이 있다.

두 문자를 더하는 경우 다음과 같이 동작한다.

```java
"A" + "B"
String("A") + String("B")		// 문자는 String 타입이다.
String("A").concat(String("B"))		// 문자의 더하기는 concat을 사용한다.
new String("AB")			// String은 불변이다. 따라서 새로운 객체가 생성된다.
```

불변인 `String`의 내부 값은 변경할 수 없다. 따라서 변경된 값을 기반으로 새로운 `String`객체를 생성한다.

불변인 `String` 클래스의 단점은 문자를 더하거나 변경할 때마다 계속해서 새로운 객체를 생성해야 한다는 점이다. 문자를 자주 더하거나 변경해야 하는 상황이라면 더 많은 `String` 객체를 만들고, GC 해야 한다. 결과적으로 컴퓨터의 CPU, 메모리를 자원을 더 많이 사용하게 된다. 그리고 문자열의 크기가 클수록, 문자열을 더 자주 변경할수록 시스템의 자원을 더 많이 소모한다.

이 문제를 해결하는 방법은 단순하다. 불변이 아닌 가변 `String`이 존재하면 된다. 가변은 내부의 값을 바로 변경하면 되기 때문에 새로운 객체를 생성할 필요가 없다. 따라서 성능과 메모리 사용면에서 불변보다 더 효율적이다.

`StringBuilder`는 내부에 `final`이 아닌 변경할 수 있는 `byte[]`을 가지고 있다.

```java
public final class StringBuilder {
 char[] value;// 자바 9 이전
 byte[] value;// 자바 9 이후
 //여러 메서드
 public StringBuilder append(String str) {...}
 public int length() {...}
 ...
}
```

## 예시

```java
public class StringBuilderMain1_1 {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        sb.append("A");
        sb.append("B");
        sb.append("C");
        sb.append("D");
        System.out.println("sb = " + sb);

        sb.insert(4, "Java");
        System.out.println("insert = " + sb);

        sb.delete(4, 8);
        System.out.println("delete = " + sb);

        sb.reverse();
        System.out.println("reverse = " + sb);

        // StringBuilder -> String
        String string = sb.toString();
        System.out.println("string = " + string);
    }
}
```

**결과**

```java
sb = ABCD
insert = ABCDJava
delete = ABCD
reverse = DCBA
string = DCBA

Process finished with exit code 0
```

- `StringBuilder` 객체를 생성한다.
- `append()` 메서드를 사용해 여러 문자열을 추가한다.
- `insert()` 메서드로 특정 위치에 문자열을 삽입한다.
- `delete()` 메서드로 특정 범위의 문자열을 삭제한다.
- `reverse()` 메서드로 문자열을 뒤집는다.
- `toString()` 메서드를 사용해 `StringBuilder`의 결과를 기반으로 `String()`을 생성해서 반환한다.

### 가변(Mutable) vs 불변(Immutable)

- `String`은 불변하다. 즉, 한 번 생성되면 그 내용을 변경할 수 없다. 따라서 문자열에 변화를 주려고 할 때마다 새로운 `String` 객체가 생성되고, 기존 객체는 버려진다. 이 과정에서 메모리와 처리 시간이 더 많이 소모한다.
- 반면에, `StringBuilder`는 가변적이다. 하나의 `StringBuilder` 객체 안에서 문자열을 추가, 삭제, 수정할 수 있으며, 이때마다 새로운 객체를 생성하지 않는다. 이로 인해 메모리 사용을 줄이고 성능을 향상시킬 수 있다.

&nbsp;

# String 최적화

자바 컴파일러는 문자열 리터럴을 더하는 부분을 자동으로 합쳐준다.

```java
// 컴파일 전
String helloWorld = "Hello, " + "World!";

// 컴파일 후
String helloWorld = "Hello, World!";

// String 변수 최적화1
// 문자열 변수의 경우 그 안에 어떤 값이 들어있는지 컴파일 시점에는 알 수 없기 때문에 단순하게 합칠 수 없다.
String result = str1 + str2;
// 이런 경우 예를 들어 다음과 같이 수행한다.
String result = new StringBuilder().append(str1).append(str2).append()
```

이렇듯 자바가 쵲거화를 처리해주기 떄문에 지금처럼 간단한 경우에는 `StringBuilder`를 사용하지 않아도 된다.

## String 최적화가 어려운 경우

다음과 같이 문자열을 루프안에서 문자열을 더하는 경우에는 최적화가 이루어지지 않는다.

```java
public class LoopStringMain {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        String result = "";
        for (int i = 0; i < 100000; i++) {
            result += "Hello Java ";
        }

        long endTime = System.currentTimeMillis();
        System.out.println("result = " + result);
        System.out.println("time ="  + (endTime - startTime) + "ms");
    }
}
```

**결과**

```java
time =8969ms
```

```java
public class LoopStringBuilderMain {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 100000; i++) {
            sb.append("Hello Java");
        }

        String result = sb.toString();
        long endTime = System.currentTimeMillis();
        System.out.println("result = " + result);
        System.out.println("time ="  + (endTime - startTime) + "ms");
    }
}

```

**결과**

```java
time =5ms
```

`StringBuilder`를 사용하니 엄청 빨라졌다.

**정리**

- 문자열을 합칠 때 대부분의 경우 최적화가 되므로 `+`연산을 사용하면된다.
- `StringBuilder`를 직접 사용하는 것이 더 좋은 경우
  - 반복문에서 반복해서 문자를 연결할 때
  - 조건문을 통해 동적으로 문자열을 조합할 때
  - 복잡한 문자열의 특정 부분을 변경해야 할 때
  - 매우 긴 대용량 문자열을 다룰 때

&nbsp;

# 메서드 체이닝 - Method Chaning

```java
public class ValueAdder {
    private int value;

    public ValueAdder add(int addValue) {
        value += addValue;
        return this;
    }

    public int getValue() {
        return value;
    }
}
```

- 단순히 값을 누적해서 더하는 기능을 제공하는 클래스다.
- `add()` 메서드를 호출할 때 마다 내부의 `value`에 값을 누적한다.
- `add()` 메서드를 보면 자기 자신(`this`)의 참조값을 반환한다.

## 예시1

```java
public class MethodChainingMain1 {
    public static void main(String[] args) {
        ValueAdder adder = new ValueAdder();
        adder.add(1);
        adder.add(2);
        adder.add(3);

        int result = adder.getValue();
        System.out.println("result = " + result);
    }
}

```

**결과**

```java
result = 6
```

- `add()`메서드를 여러번 호출해서 값을 누적해서 더하고 출력한다.
- 여기서는 `add()`메서드의 반환값은 사용하지 않았다.

## 예시2

```java
public class MethodChainingMain2 {
    public static void main(String[] args) {
        ValueAdder adder = new ValueAdder();
        ValueAdder adder1 = adder.add(1);
        ValueAdder adder2 = adder1.add(2);
        ValueAdder adder3 = adder2.add(3);

        int result = adder3.getValue();
        System.out.println("result = " + result);

        System.out.println("result = " + adder);
        System.out.println("result = " + adder1);
        System.out.println("result = " + adder2);
        System.out.println("result = " + adder3);

    }
}
```

**결과**

```java
result = 6
result = lang.string.chaining.ValueAdder@5b6f7412
result = lang.string.chaining.ValueAdder@5b6f7412
result = lang.string.chaining.ValueAdder@5b6f7412
result = lang.string.chaining.ValueAdder@5b6f7412
```

- `adder.add(`)을 호출한다.
- `add()`메서드는 결과를 누적하고 자기 자신의 참조값인 `this(x00`1)를 반환한다.
- `adder1`변수는 `adder`와 같은 `x001`인스턴스를 참조한다.
- `add()`메서드는 자기 자신 (`this`)의 참조값을 반환한다. 이 반환값을 `adder1`, `adder2`, `adder3` 에 보관했다.
- 따라서 `adder`, `adder1`, `adder2`, `adder3` 은 모두 같은 참조값을 사용한다. 왜냐하면 `add()`메서드가 자기 자신(`this`)의 참조값을 반환했기 때문이다.

## 예시3

```java
public class MethodChainingMain3 {
    public static void main(String[] args) {
        ValueAdder adder = new ValueAdder();
        int result = adder.add(1).add(2).add(3).getValue();
//        int result = x001.x001.x001.x001.getValue();
        System.out.println("result = " + result);
    }
}
```

**결과**

```java
result = 6
```

`add()` 메서드를 호출하면 `ValueAdder` 인스턴스 자신의 참조값(`x001`)이 반환한다. 이 반환된 참조값을 변수에 담아두지 않아도 된다. 대신에 반환된 참조값을 즉시 사용해서 바로 메서드를 호출할 수 있다.

```java
adder.add(1).add(2).add(3).getValue();
x001.add(1).add(2).add(3).getValue();
x001.add(2).add(3).getValue();
x001.add(3).getValue();
x001.getValue();
```

메서드 호출의 결과로 자기 자신의 참조값을 반환하면, 반환된 참조값을 사용해서 메서드 호출을 계속 이어갈 수 있다. 코드를 보면 `.` 을 찍고 메섣르르 게쏙 연결해서 사용한다. 마치 메서드가 체인으로 연결된 것 처럼 보인다. 이러한 기법을 메서드 체이닝이라 한다.

`StringBuilder`의 `append()` 메서드를 보면 자기 자신의 참조값을 반환한다.

```java
public StringBuilder append(String str) {
 super.append(str);
 return this;
}
```

`StringBuilder`에서 문자열을 변경하는 대부분의 메서드도 메서드 체이닝 기법을 제공하기 위해 자기 자신의 반환한다.
