---
title : "[Java] Class 클래스"
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

자바에서 `Class` 클래스는 클래스의 정보(메타데이터)를 다루는데 사용된다. `Class` 클래스를 통해 개발자는 실행중인 자바 애플리케이션 내에서 필요한 클래스의 속성과 메서드에 대한 정보를 조회하고 조작할 수 있다.

- 타입 정보 얻기 : 클래스의 이름, 슈퍼클래스, 인터페이스, 접근 제한자 등과 같은 정보를 조회 할 수 있다.
- 리플레션 : 클레스에 정의된 메서드, 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메서드를 호출하는 등의 작업을 할 수 있다.
- 동적 로딩과 생성 : `Class.forName()`메서드를 사용하여 클래스를 동적으로 로드하고, `newInstance()`메서드를 통해 새로운 인스턴스를 생성할 수 있다.
- 애토테이션 처리 : 클래스에 적용된 애노테이션을 조회하고 처리하는 기능을 제공한다.



```java
public class ClassMetaMain {
    public static void main(String[] args) throws ClassNotFoundException {
        // Class 조회
//        Class clazz = String.class; // 1. 클래스에서 조회
//         Class clazz = new String().getClass(); // 2. 인스턴스에서 조회
         Class clazz = Class.forName("java.lang.String");  // 3. 문자열로 조회

        // 모든 필드 출력
        Field[] fields = clazz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println("field " + field.getType() + " " + field.getName());
        }

        // 모든 메서드 출력
        Method[] methods = clazz.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("method = " + method);
        }

        // 상위 클래스 정보 출력
        System.out.println("Supercalss: " + clazz.getSuperclass().getName());

        // 인터페이스 정보 출력
        Class[] interfaces = clazz.getInterfaces();
        for (Class i : interfaces) {
            System.out.println("Interface: " + i.getName());
        }
    }
}
```

**결과**

```java
...
Supercalss: java.lang.Object
Interface: java.io.Serializable
Interface: java.lang.Comparable
Interface: java.lang.CharSequence
Interface: java.lang.constant.Constable
Interface: java.lang.constant.ConstantDesc

Process finished with exit code 0
```

**class vs clazz**

`class`는 자바의 예약어다. 따라서 패키지명, 변수명으로 사용할 수 없다. 이러한 이유로 자바 개발자들은 `clazz`는 `class`와 유사하게 들리고, 이 단어가 `class`를 의미한다.

**`Class` 클래스 조회 방법**

- `Class clazz = String.class` : 클래스에서 조회
- `Class clazz = new String().getClass()` : 인스턴스에서 조회
- `Class clazz = Class.forName("java.lang.String")` : 문자열로 조회

**Class 클래스의 주요 기능**

- **getDeclaredFields()** : 클래스의 모든 필드를 조회한다.
- **getDelcaredMethods()** : 클래스의 모든 메서드를 조회한다.
- **getSuperclass()** : 클래스의 부모 클래스를 조회한다.
- **getInterfaces()** : 클래스의 인터페스들을 조회한다.

## 클래스 생성하기

```java
public class Hello {
    public String hello() {
        return "hello!";
    }
}

```

```java
public class ClassCreateMain {
    public static void main(String[] args) throws Exception {
//        Class helloClass = Hello.class;
        Class helloClass = Class.forName("lang.clazz.Hello");

        Hello hello = (Hello) helloClass.getDeclaredConstructor().newInstance();
        String result = hello.hello();
        System.out.println("hello = " + hello);
        System.out.println("result = " + result);
    }
}
```

**결과**

```java
hello = lang.clazz.Hello@b4c966a
result = hello!

Process finished with exit code 0

```

**getDeclaredConstrutor().newInstance()**

- `getDeclaredConstructor()` : 생성자를 선택한다.
- `newInstance()` : 선택된 생성자를 기반으로 인스턴스를 생성한다.

**리플렉션**

`Class`를 사용하면 클래스의 메타 정보를 기반으로 클래스에 정의된 메서드 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메서드를 호출하는 작업을 할 수 있다. 이런 작업을 리플렉션이라 한다. 추가로 애노테이션 정보를 읽어서 특별한 기능을 수행할 수도 있다.

&nbsp;

# System 클래스

```java
public class SystemMain {
    public static void main(String[] args) {
        // 현재 시간(밀리초)를 가져온다.
        long currentTimeMillis = System.currentTimeMillis();
        System.out.println("currentTimeMillis = " + currentTimeMillis);

        // 현재 시간(나노초)를 가져온다.
        long currentTimeNano = System.nanoTime();
        System.out.println("currentTimeNano = " + currentTimeNano);

        // 환경 변수를 읽는다.
        System.out.println("getenv = " + System.getenv());

        // 시스템 속성을 읽는다.
        System.out.println("properties = " + System.getProperties());
        System.out.println("Java version: " + System.getProperty("java.version"));

        // 배열을 고속으로 복사한다.
        char[] originalArray = {'h', 'e', 'l', 'l', 'o'};
        char[] copiedArray = new char[5];
        System.arraycopy(originalArray, 0, copiedArray, 0, originalArray.length);

        // 배열 출력
        System.out.println("copiedArray = " + copiedArray);
        System.out.println("copiedArray = " + Arrays.toString(copiedArray));

        // 프로그램 종료
        System.exit(0);
        System.out.println("hello");
    }
}
```

**결과**

```java
currentTimeMillis = 1720860493765
currentTimeNano = 146949581865300
getenv = {USERDOMAIN_ROAMINGPROFILE=DESKTOP-G30EQ2L, LOCALAPPDATA=C:\Users\seoha\AppData\Local, PROCESSOR_LEVEL=23, USERDOMAIN=DESKTOP-G30EQ2L, FPS_BROWSER_APP_PROFILE_STRING=Internet Explorer, LOGONSERVER=\\DESKTOP-G30EQ2L, JAVA_HOME=C:\openjdk-17.0.2_windows-x64_bin\jdk-17.0.2, ...}
Java version: 22.0.1
copiedArray = [C@3941a79c
copiedArray = [h, e, l, l, o]

Process finished with exit code 0

```

- 표준 입력, 출력, 오류 스트림 : `System.in`, `System.out`, `System.err`은 각각 표준 입력, 표준 출력, 표준 오류 스트림을 나타낸다.
- 시간 측정 : `System.currentTimeMillis()`와 `System.nanoTime()`은 현재 시간을 밀리초 또는 나노초 단위로 제공한다.
- 환경변수 : `System.getenv()` 메서드를 사용하여 OS에서 설정한 환경 변수의 값을 얻을 수 있다.
- 시스템 속성 : `System.getProperties()`를 사용해 현재 시스템 속성을 얻거나 `System.getProperty(String key)`로 특정 속성을 얻을 수 있다. 시스템 속성은 자바에서 사용하는 설정 값이다.
- 시스템 종료 : `System.exit(int status)`메서든느 프로그램을 종료하고, OS에 프로그램 종료의 상태 코드를 전달한다.
  - 상태 코드 `0` : 정상종료
  - 상태 코드 `0`이 아님 : 오류나 예외적인 종료
- 배열 고속 복사 : `System.arraycopy`는 시스템 레벨에서 최적화된 메모리 복사 연산을 사용한다. 직접 반복문을 사용해서 배열을 복사할 때 보다 수 배 이상 빠른 성능을 제공한다.

&nbsp;

# Math, Random 클래스

## 1. Math 클래스

- `abs(x)` : 절대값
- `max(a, b)` : 최대값
- `min(a, b)` : 최소값
- `exp(x)` : e^x 계산
- `log(x)` : 자연 로그
- `log10(x)` : 로그 10
- `pow(a, b)` : a의 b제곱
- `ceil(x)` : 올림
- `floor(x)` : 내림
- `rint(x)` : 가장 가까운 정수로 반올림
- `round(x)`: 반올림
- `sin(x)` : 사인
- `cos(x)` : 코사인
- `tan(x)` : 탄젠트
- `sqrt(x)` : 제곱근
- `cbrt(x)` : 세제곱근
- `random()` : 0.0과 1.0 사이의 무작위 값 생성

## 2. Random 클래스

```java
public class RandomMain {
    public static void main(String[] args) {
        Random random = new Random();
//        Random random = new Random(1);  // seed가 같으면 Random의 결과가 같다.

        int randomInt = random.nextInt();
        System.out.println("randomInt = " + randomInt);

        double randomDouble = random.nextDouble();// 0.0d ~ 1.0d
        System.out.println("randomDouble = " + randomDouble);

        boolean randomBoolean = random.nextBoolean();
        System.out.println("randomBoolean = " + randomBoolean);

        // 범위 조회
        int randomRange1 = random.nextInt(10);// 0 ~ 9 까지 출력
        System.out.println("0 ~ 9: = " + randomRange1);

        int randomRange2 = random.nextInt(10) + 1;// 1 ~ 10 까지 출력
        System.out.println("1 ~ 10 = " + randomRange2);
    }
}
```

**결과**

```java
randomInt = 1377829418
randomDouble = 0.5175471465006226
randomBoolean = true
0 ~ 9: = 8
1 ~ 10 = 3

```

- `random.nextIn()` : 랜덤 `int`값을 변환한다.
- `nextDouble()` : `0.0d` ~ `1.0d` 사이의 랜덤 `double`값을 반환한다.
- `nextBoolean()` : 랜덤 `boolean` 값을 반환한다.
- `nextInt(int bound)` : `0` ~ `bound` 미만의 숫자를 랜덤으로 반환한다. 예를 들어 3을 입력하면 `0, 1, 2` 를 반환한다.

### 씨드 - Seed

랜덤은 내부에서 씨드(Seed) 값을 사용해서 랜덤 값을 구한다. 그런데 이 씨드 값이 같으면 항상 같은 결과가 출력된다.

```java
//        Random random = new Random();
        Random random = new Random(1);  // seed가 같으면 Random의 결과가 같다.
```

**결과**

```java
randomInt = -1155869325
randomDouble = 0.10047321632624884
randomBoolean = false
0 ~ 9: = 4
1 ~ 10 = 5

Process finished with exit code 0
```

- `new Random()` : 생서자를 비워두면 내부에서 `System.nanoTime()`에 여러가지 복잡한 알고리즘을 섞어서 씨드값을 생성한다. 따라서 반복 실행해도 항상 달라진다.
- `new Random(int seed)` : 생성자에 씨드 값을 직접 전달할 수 있다. 씨드 값이 같으면 여러번 반복 실행해도 실행 결과가 같다.
