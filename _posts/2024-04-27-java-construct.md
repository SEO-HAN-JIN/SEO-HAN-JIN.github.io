---
title : "[Java] 생성자"
categories: Java
tag: [java]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
# search: false # 검색을 원하지 않을 때
---
# 개요

자바에서 생상자와 this, 기본생성자, 오버로딩 등을 알아보자

# ❓ 생성자가 필요한 이유

생성자를 알아보기 전에 왜 생성자가 필요한지 알아보자.

## 코드1

```java
public class MemberInit {
    String name;
    int age;
    int grade;
}
```

```java
public class MethodMain1 {
    public static void main(String[] args) {
        MemberInit member1 = new MemberInit();
        member1.name = "user1";
        member1.age = 15;
        member1.grade = 90;

        MemberInit member2 = new MemberInit();
        member2.name = "user2";
        member2.age = 16;
        member2.grade = 80;

        MemberInit[] members = {member1, member2};

        for (MemberInit s : members) {
            System.out.println("이름: " + s.name + " 나이: " + s.age + " 성적: " + s.grade);
        }

    }
}
```

`코드1`의 경우 멤버를 추가할 때마다 반복적으로 코드를 작성해야하 한다. 이것을 코드2를 통해 개선해보자.

## 코드2

```java
public class MemberInit {
    String name;
    int age;
    int grade;
}
```

```java
public class MethodMain2 {
    public static void main(String[] args) {
        MemberInit member1 = new MemberInit();
        initMember(member1, "user1", 15, 90);

        MemberInit member2 = new MemberInit();
        initMember(member2, "user2", 16, 80);

        MemberInit[] members = {member1, member2};

        for (MemberInit s : members) {
            System.out.println("이름: " + s.name + " 나이: " + s.age + " 성적: " + s.grade);
        }
    }

    static void initMember(MemberInit member, String name, int age, int grade) {
        member.name = name;
        member.age = age;
        member.grade = grade;
    }
}
```

`initMemeber(...)` 메서드를 사용해서 반복을 제거했다. 그런데 이 메서드는 `Member` 객체의 멤버 변수를 사용한다. 이 경우에는 객체 지향 방법으로 속성과 기능을 한 곳(`Member`)에 두는 것이 더 나은 방법이다. 즉, `Member`가 자기 자신의 데이터를 변경하는 기능(메서드)을 제공하는 것이 좋다.

## 코드3

`MemberInit `에서 데이터를 변경하는 기능을 제공하도록 변경보자.

```java
public class MemberInit {
    String name;
    int age;
    int grade;

    void initMember(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

```java
public class MethodMain3 {
    public static void main(String[] args) {
        MemberInit member1 = new MemberInit();
        member1.initMember("user1", 15, 90);

        MemberInit member2 = new MemberInit();
        member2.initMember("user2", 16, 80);

        MemberInit[] members = {member1, member2};

        for (MemberInit s : members) {
            System.out.println("이름: " + s.name + " 나이: " + s.age + " 성적: " + s.grade);
        }
    }
}
```

# ❓ this란?

위의 `MemberInit`의 코드를 다시 보자.

```java
public class MemberInit {
    String name;
    int age;
    int grade;

    void initMember(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

`initMember(String name..)`의 코드를 보면 메서드의 매개변수에 정의한 `String name`과 `MemberInit`의 멤버 변수의 이름이 `String name`으로 똑같다. 나머지 변수 이름도 `age`, `grade`로 모두 같다. 멤버 변수와 메서드의 매개변수의 이름이 같다면 둘을 어떻게 구분해야할까?

- 이 경우 매개변수가 우선순위를 가진다. 따라서 `initMember(String name, ...)`메서드 안에서 `name`이라고 적으면 멤버 변수가 아닌 매개변수에 접근하게 된다.
- 멤버 변수에 접근하려면 앞에 `this.` 이라고 해주면 된다. 여기서 `this`는 인스턴스 자신의 참조값을 가리킨다.

```java
this.name = name;	// 오른쪽의 name은 매개변수에 접근
this.name = "user";	// name 매개변수의 값 사용
x001.name = "user";	// this. 는 인스턴스 자신의 참조값을 뜻함, 따라서 인스턴스 멤버 변수에 접근
```

## this 제거

`this`는 생략할 수 있다. 이 경우 변수를 찾을 때 가까운 지역변수(매개변수도 지역변수)를 먼저 찾고 없으면 그 다음으로 멤버 변수를 찾는다. 멤버 변수도 없으면 오류가 발생한다.

### 예제1 - 멤버변수와 매개변수의 이름이 같을 때

만약 아래 예제에서 `this`를 제거하면 어떻게 될까?

```java
this.name = name
```

다음과 같이 수정하면 name은 둘 다 매개변수를 뜻하게 된다. 따라서 멤버 변수의 값이 변경되지 않는다.

```java
name = name
```

### 예제2 - 멤버변수와 매개변수의 이름이 다를 때

만약 아래 예제에서 `this`를 제거하면 어떻게 될까?

```java
public class MemberThis {
    String nameField;

    void initMember(String nameParameter) {
        nameField = nameParameter;
    }
}
```

`nameFiled`는 앞에 `this`가 없어도 멤버 변수에 접근한다.

- `nameField`는 먼저 지역변수(매개변수)에서 같은 이름이 있는지 찾는다. 이 경우 없으므로 멤버 변수에서 찾는다.
- `nameParameter`는 먼저 지역변수(매개변수)에서 같은 이름이 있는지 찾는다. 이 경우 매개변수가 있으므로 매개변수를 사용한다.

## 굳이 `this`를 사용해야할까?

`예제2`의  `this.nameField `를 보면 `this`를 생략할 수 있지만, 생략하지 않고 사용해도 된다. 이렇게 `this`를 사용하면 이 코드가 멤버 변수를 사용한다는 것을 눈으로 쉽게 확인할 수 있다. 그래서 과거에 이런 스타일을 많이 사용하기도 했다. 하지만 최근에 IDE가 발전하면서 IDE가 멤버 변수와 지역 변수를 색상으로 구분해준다. 이런 점 때문에 `this`는 앞서 설명한 것 처럼 이름이 중복된 것 처럼, 꼭 필요한 경우에만 사용해도 충분할 것이다.

## 정리

1. 매개변수의 이름과 멤버 변수의 이름이 같은 경우 `this`를 사용해서 둘을 명확하게 구분해야 한다.
2. `this`는 인스턴스 자신을 가리킨다.

# ✒ 생성자

객체를 생성하는 시점에 어떤 작업을 하고 싶다면 생성자(Construct)를 이용하면 된다.

```java
public class MemberConstruct {
    String name;
    int age;
    int grade;

    MemberConstruct(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

}
```

```java
public class ConstructMain1 {
    public static void main(String[] args) {
        MemberConstruct member1 = new MemberConstruct("user1", 15, 90);
        MemberConstruct member2 = new MemberConstruct("user2", 16, 80);

        MemberConstruct[] members = {member1, member2};
        for (MemberConstruct s : members) {
            System.out.println("이름:" + s.name + " 나이:" + s.age + " 성적:" + s.grade);
        }
    }
}
```

생성자는 메서드와 비슷하지만 다음과 같은 차이가 있다.

* 생성자의 이름은 클래스 이름과 같아야 한다. 따라서 첫 글자도 대문자로 시작한다.
* 생성자는 반환 타입이 없다. 비워두어야 한다.
* 나머지는 메서드와 같다.

## 장점

* 중복 호출 제거 : 생성자는 없던 시절에는 생성 직후에 어떤 작업을 수행하기 위해 다음과 같이 메서드를 직접 한번 더 호출해야한다. 생성자 덕분에 객체를 생성하면서 동시에 생성 직후에 필요한 작업을 한번에 처리할 수 있게 되었다.

```java
// 생성자 등장 전
MemberInit member = new MemberInit();
member.initMember("user1", 15, 90);
// 생성자 등장 후
MemberConstruct member = new MemberConstruct("user1", 15, 90);

```

- 제약 (생성자 호출 필수) : 위에서 생성자 등장 전 코드를 보자. 이 경우 `initMember(...)`를 실수로 호출하지 않으면 어떻게 될까? 이 메서드를 실수로 호출하지 않아도 프로그램은 작동한다. 하지만 회원의 이름과 나이, 성적 데이터가 없는 상태로 프로그램이 동작하게 된다. 만약에 이 값들을 필수 반드시 입력해야 한다면, 시스템이 큰 문제가 발생할 수 있다.

## 기본생성자

```java
public class MemberDefault {
    String name;

    MemberDefault() {

    }
}
```

- 매개변수가 없는 생성자를 기본 생성자라 한다.
- 클래스가 생성자가 하나도 없으면 자바 컴파일러는 매개변수가 없고, 작동하는 코드가 없는 기본 생성자를 자동으로 만들어준다.
- 생성자가 하나라도 있으면 자바는 기본 생성자를 만들지 않는다.

> 왜 기본생성자를 자동으로 만들어줄까?
>
> 만약 자바에서 기본 생성자를 만들어주지 않는다면 생성자 기능이 필요하지 않는 경우에도 모든 클래스에 개발자가 직접 생성자를 정의해야 한다. 생성자 기능을 사용하지 않는 경우도 많기 때문에 이런 편의 기능을 제공한다.

## 생성자 오버로딩

생성자도 메서드 오버로딩처럼 매개변수만 다르게해서 여러 생성자를 만들 수 있다.

```java
public class MemberConstruct {
    String name;
    int age;
    int grade;

    MemberConstruct(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

    // 생성자 오버로딩 
    MemberConstruct(String name, int age) {
        this.name = name;
        this.age = age;
        this.grade= 50;
    }
}
```

## this()

`this`() 기능을 사용하면 생성자 내부에서 자신의 생성자를 호출할 수 있다. 참고로 `this`는 인스턴스 자신을 참조값을 가리킨다. 그래서 자신의 생서자를 호출한다고 생각하면 된다.

```java
public class MemberConstruct {
    String name;
    int age;
    int grade;

    MemberConstruct(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

    // 생성자 오버로딩 
    MemberConstruct(String name, int age) {
	this(name, age, 50);
    }
}

```

* `this()`는 생성자 코드의 첫줄에만 작성할 수 있다.

```java
System.out.println("go")	// this()가 생성자 코드의 첫줄에 사용되지 않아 오류가 발생한다.
this(name, age, 50);
```
