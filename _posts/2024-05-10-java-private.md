---
title : "[Java] 접근제어자"
categories: Java
tag: [java]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
# search: false # 검색을 원하지 않을 때
---

  

# 개요

자바에서 접근제어자를 알아보자.

  

# ❓ 접근제어자가 필요한이유

스피커의 볼륨을 조절하는 기능을 구현하는 코드를 통해 접근제어자가 필요한 이유를 알아보자.

```java
public class Speaker {

   int volume;

    Speaker(int volume) {
        this.volume = volume;
    }

    void volumeUp() {
        if (volume >= 100) {
            System.out.println("음량을 증가할 수 없습니다. 최대 음략입니다.");
        } else {
            volume += 10;
            System.out.println("음량을 10 증가합니다.");
        }
    }

    void volumeDown() {
        volume -= 10;
        System.out.println("volumeDown 호출");
    }

    void showVolume() {
        System.out.println("현재 음량 : " + volume);
    }
}
```

```java
public class SpeakerMain {

    public static void main(String[] args) {
        Speaker speaker = new Speaker(90);
        speaker.showVolume();

        speaker.volumeUp();
        speaker.showVolume();

        speaker.volumeUp();
        speaker.showVolume();

        // 필드에 직접 접근
        System.out.println("volume 필드 직접 접근 수정");
        speaker.volume = 200;
        speaker.showVolume();
    }

}
```

스피커의 볼륨을 조절하는 기능이 있는 `Speaker`클래스와 그것을 실행하는 `SpeakerMain`클래스가 있다. 여기서 스피커의 볼륨은 100을 넘기면 음량을 증가할 수 없도록 만들어 놓았는데 `SpeakerMain`클래스 15 라인을 보면 `Speaker`클래스의 필드에 직접 접근하여 200으로 변경해주었다.

```java
현재 음량 : 90
음량을 10 증가합니다.
현재 음량 : 100
음량을 증가할 수 없습니다. 최대 음략입니다.
현재 음량 : 100
volume 필드 직접 접근 수정
현재 음량 : 200

Process finished with exit code 0

```

결과는 200으로 변경되어 의도와 달리 코드가 동작하였다.

우리는 접근제어자를 이용하여 이러한 비정상적인 동작은 방지할 수 있다.

## 코드 변경

`Speaker`클래스에서 `volume`필드의 접근제어자를 `private`으로 변경해보자.

```java
public class Speaker {
	...
	private int volume;
	...
}
```

이렇게 변경하면 `SpeakerMain`클래스에서 `Speaker`클래스의 필드에 직접 접근을 할 수 없다.

&nbsp;

# 접근 제어자의 종류

### 접근 제어자 종류

- `private` : 모든 외부 호출을 막는다.
- `default (package-private)` : 같은 패키지 안에서 호출을 허용한다.
- `protected` : 같은 패키지 안에서 호출을 허용한다. 패키지가 달라도 상속 관계의 호출을 허용한다.
- `public` : 모든 외부 호출을 허용한다.

### 접근 제어자 허용 범위

`private -> default -> protected -> public`

### `default` 접근 제어자

접근 제어자를 명시하지 않으면 같은 패키지 안에서 호출을 허용하는 `default`접근 제어자가 적용된다. 실제로는 `package-private`이 더 정확한 표현일지도 모른다.

### 접근 제어자 위치

접근 제어자는 필드와 메서드, 생성자에 사용될 수 있다. 추가로 클래스 레벨에도 일부 접근 제어자를 사용할 수 있다.

&nbsp;

# 실습

실습 코드를 통해 접근 제어자를 알아보자.

## 같은 패키지

아래 2개의 클래스(`AccessData`, `AccessDataMain`)는 같은 패키지 안에 있다.

```java
package access.a;

public class AccessData {
    public int publicField;
    int defaultField;
    private int privateField;

    public void publicMethod() {
        System.out.println("publicMethod 호출 " + publicField);
    }

    void defaultMethod() {
        System.out.println("defaultMethod 호출 " + defaultField);
    }

    private void privateMethod() {
        System.out.println("privateMethod 호출 " + privateField);
    }

    /*
        내부 호출을 보여줌.
        내부 호출은 자기 자신에게 접근하는 것이다. 따라서 private 을 포함한 모든 곳에 접근할 수 있다.
     */
    public void innerAccess() {
        System.out.println("내부 호출");
        publicField = 100;
        defaultField = 200;
        privateField = 300;
        publicMethod();
        defaultMethod();
        privateMethod();
    }
}
```

```java
package access.a;

public class AccessInnerMain {
    public static void main(String[] args) {
        AccessData accessData = new AccessData();
        // public 호출 가능
        accessData.publicField = 1;
        accessData.publicMethod();

        // 같은 패키지 default 호출 가능
        accessData.defaultField = 2;
        accessData.defaultMethod();

        // private 호출 불가
//        accessData.privateField = 3;
//        accessData.privateMethod();

        accessData.innerAccess();
    }
}
```

같은 패키지 안에 있기 때문에 `public`, `default`는 호출이 가능했지만, `private`은 호출이 불가능 했다.

## 다른 패키지

위의 코드와 다른 패키지에 클래스를 만들어 확인해보자.

```java
package access.b;

import access.a.AccessData;

public class AccessOuterMain {
    public static void main(String[] args) {
        AccessData accessData = new AccessData();
        // public 호출 가능
        accessData.publicField = 1;
        accessData.publicMethod();

        // 다른 패키지 default 호출 불가
//        accessData.defaultField = 2;
//        accessData.defaultMethod();

        // private 호출 불가
//        accessData.privateField = 3;
//        accessData.privateMethod();

        accessData.innerAccess();
    }
}
```

같은 패키지와 달리 `default`도 호출이 불가능해 졌다.

`innerAccess`메서드 같은 경우 해당 메서드의 접근제어자는 `public`이고, 이 메서드는 내부 호출하여 자기 자신에게 접근하는 것이기 때문에 모든 곳에 접근이 가능하다.

&nbsp;

# 클래스 레벨 접근 제어자

- 클래스 레벨의 접근 제어자는 `public`, `default`만 사용할 수 있다.
  - `private`, `protected`는 사용할 수 없음
- `public`클래스는 반드시 파일명과 이름이 같아야 한다.
  - 하나의 자바 파일에 `public`클래스는 하나만 등장할 수 있다.
  - 하나의 자바 파일에 `default` 접근 제어자를 사용하는 클래스는 무한정 만들 수 있다.

## 같은 패키지

같은 패키지 안에서 클래스는 모두 접근이 가능하다.

```java
package access.a;

public class PublicClass {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();
        DefaultClass1 defaultClass1 = new DefaultClass1();
        DefaultClass2 defaultClass2 = new DefaultClass2();
    }
}

class DefaultClass1 {

}

class DefaultClass2 {

}

```

```java
package access.a;

public class PublicClassInnerMain {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();
        DefaultClass1 defaultClass1 = new DefaultClass1();
        DefaultClass2 defaultClass2 = new DefaultClass2();
    }
}
```

## 다른 패키지

다른 패키지의 클래스는 `public`외에 접근이 불가능.

```java
package access.b;

import access.a.PublicClass;

public class PublicClassOuterMain {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();

        // 다른 패키지 접근 불가
//        DefaultClass1 defaultClass1 = new DefaultClass1();
//        DefaultClass2 defaultClass2 = new DefaultClass2();
    }
}

```
