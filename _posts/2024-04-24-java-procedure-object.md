---
title : "[Java]절차 지향, 객체 지향 프로그래밍"
categories: Java
tag: [java]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
# search: false # 검색을 원하지 않을 때
---

# 개요

자바에서 절차 지향 프로그래밍와 객체 지향 프로그래밍이 무엇인지 알아보자.


# 절차 지향 프로그래밍

절차 지향 프로그래밍을 음악 플레이어를 켜고, 끄고, 볼륨을 증가 감소하는 프로그래밍을 만들어보자.

## 코드1

하나의 메서드(`main`)안에 모든 로직을 넣어 놓았다.

```java
public static void main(String[] args) {
        int volume = 0;
        boolean isOn = false;

        // 음악 플레이어 켜기
        isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");

        // 볼륨 증가
        volume++;
        System.out.println("음악 플레이어 볼륨: " + volume);

        // 볼륨 증가
        volume++;
        System.out.println("음악 플레이어 볼륨: " + volume);

        // 볼륨 감소
        volume--;
        System.out.println("음악 플레이어 볼륨: " + volume);

        // 음악 플에이어 상태
        System.out.println("음악 플레이어 상태 확인");
        if (isOn) {
            System.out.println("음악 플레이어 ON, 볼륨: " + volume);
        } else{
            System.out.println("음악 플레이어 OFF");
        }

        // 음악 플레이어 끄기
        isOn = false;
        System.out.println("음악 플레이어를 종료합니다.");
    }
```

## 코드2

음악 플레이어의 데이터를 별도의 클래스로 분리하고,  `음악 플레이어 켜기`, `음악 플레이어 끄기`, `볼륨 증가`, `볼륨 감소`, `음악 플레이어 상태` 등의 주요 로직을 메서드로 만들었다. (모듈화)

> 모듈화란?
>
> 쉽게 이야기 해서 레고 블럭을 생각하면 된다. 필요한 블럭을 가져다 꼽아서 사용할 수 있다. 음악 플레이어의 기능을 필요하면 해당 기능을 메서드 호출만으로 손쉽게 사용할 수 있다. 이제 음악 플레이어와 관련된 메서드를 조립해서 프로그램을 작성 할 수 있다.

```java
public class MusicPlayerData {
    int volume = 0;
    boolean isOn = false;
}
```

```java
public class MusicPlayerMain2 {
    public static void main(String[] args) {
        MusicPlayerData data = new MusicPlayerData();
        // 음악 플레이어 켜기
        on(data);
        // 볼륨 증가
        volumeUp(data);
        // 볼륨 증가
        volumeUp(data);
        // 볼륨 감소
        volumeDown(data);
        // 음악 플에이어 상태
        showStatus(data);
        // 음악 플레이어 끄기
        off(data);
    }

    static void on(MusicPlayerData data) {
        data.isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");
    }

    static void off(MusicPlayerData data) {
        data.isOn = false;
        System.out.println("음악 플레이러를 종료합니다.");
    }

    static void volumeUp(MusicPlayerData data) {
        data.volume++;
        System.out.println("음악 플레이어 볼륨: " + data.volume);
    }

    static void volumeDown(MusicPlayerData data) {
        data.volume--;
        System.out.println("음악 플레이어 볼륨: " + data.volume);
    }

    static void showStatus(MusicPlayerData data) {
        System.out.println("음악 플레이어 상태 확인");
        if (data.isOn) {
            System.out.println("음악 플레이어 ON, 볼륨: " + data.volume);
        } else{
            System.out.println("음악 플레이어 OFF");
        }
    }
}
```

각각의 기능을 메서드로 만든 덕분에 각각의 기능이 모듈화 되었다.

장점

- 중복 제거: 로직 중복이 제거되었다. 같은 로직이 필요하면 해당 메서드를 여러번 호출하면 된다.
- 변경 영향 범위: 기능을 수정할 때 해당 메서드 내부만 변경하면 된다.
- 메서드 이름 추가: 메서드 이름을 통해 코드를 더 쉽게 이해할 수 있다.

절차 지향 프로그래밍의 한계

데이터와 기능이 분리되어 있다. 음악 플레이어의 데이터는 `MusicPlayerData`에 있는데, 그 데이터를 사용하는 기능은 `MusicPlayerMain2`클래스에 있는 각각의 메서드에 분리되어있다. (`on()`, `off()`, `volumeUp()`, ...) 그래서 음악 플레이어와 관련된 데이터는 MusicPlayerData의 데이터를 사용해야 하고, 음악 플레이어와 관련된 기능은 `MusicPlayerMain2`의 각 메서드를 사용해야한다.

데이터와 그 데이터를 사용하는 기능은 매우 밀접하게 연관되어있다. 각각의 메서드를 보면 대부분 `MusicPlayerData`의 데이터를 사용한다. 따라서 이후에 관련된 데이터가 수정되면 `MusicPlayerMain2 `부분의 메서드들도 함께 변경해야 한다. 그리고 이렇게 데이터와 기능이 분리되어 있으면 유지보수 관점에서도 관리 포인트가 2곳으로 늘어난다.

객체 지향 프로그래밍이 나오기 전까지는 지금과 같이 데이터와 기능이 분리되어 있었다. 따라서 지금과 같은 코드가 최선이었다. 하지만 객체 지향 프로그래밍이 나오면서 데이터와 기능을 온전히 하나로 묶어서 사용할 수 있게 되었다.


# 클래스와 메서드

클래스는 데이터인 멤버 변수 뿐 아니라 기능 역할을 하는 메서드도 포함할 수 있다.


## 코드1

기능 메서드인 `add()`메서드를 기능을 수행하는 `ValueDataMain` 클래스에서 작성하는 방법을 사용해보자.

```java
public class ValueData {
    int value;
}
```

```java
public class ValueDataMain {

    public static void main(String[] args) {
        ValueData valueData = new ValueData();
        add(valueData);
        add(valueData);
        add(valueData);
        System.out.println("최종 숫자=" + valueData.value);
    }

    static void add(ValueData valueData) {
        valueData.value++;
        System.out.println("숫자 증가 value=" + valueData.value);
    }
}
```

## 코드2

이번엔 `add()` 메서드를 데이터 클래스인 `ValueData` 클래스에서 작성하는 방법을 사용해보자.

```java
public class ValueData {
    int value;

    void add() {
        value++;
        System.out.println("숫자 증가 value=" + value);
    }
}
```

```java
public class ValueObjectMain {
    public static void main(String[] args) {
        ValueData valueData = new ValueData();
        valueData.add();
        valueData.add();
        valueData.add();
        System.out.println("최종 숫자=" + valueData.value);
    }

}

```

인스턴스의 메서드 호출

인스턴스의 메서드를 호출하는 방법은 `멤버 변수`를 사용하는 방법과 같다. `.(dot)`을 찍어서 객체에 접근한 다음에 원하는 메서드를 호출하면 된다.

```java
valueData.add();	// 1
x002.add();		// 2
```

`add()` 메서드를 호출하면 메서드 내부에서 `value++`을 호출하게 된다. 이때 `value`에 접근해야 하는데, 기본적으로 본인 인스턴스에 있는 멤버 변수에 접근한다. 본인 인스턴스가 `x002 `참조값을 사용하므로 자기 자신인 `x002.value` 에 접근하게 된다.

![1713951980939](../../images/2024-04-24-java-procedure-object/1713951980939.png)

정리

- 클래스는 속성(데이터, 멤버 변수)과 기능(메서드)을 정의할 수 있다.
- 객체는 자신의 메서드를 통해 자신의 멤버 변수에 접근할 수 있다.
- 객체의 메서드 내부에서 접근하는 멤버 변수는 객체 자신의 멤버 변수이다.

# 객체 지향 프로그래밍

절차 지향 프로그래밍에서 개발한 음악 플레이어는 데이터와 기능이 분리되어 있었다. 이제 위에서 알아본 `클래스와 메서드`에서 배운대로 데이터와 기능을 하나로 묶어서 음악 플레이어라는 개념을 온전히 하나의 클래스에 담아보자.

```java
public class MusicPlayer {
    int volume = 0;
    boolean isOn = false;

    void on() {
        isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");
    }

    void off() {
        isOn = false;
        System.out.println("음악 플레이러를 종료합니다.");
    }

    void volumeUp() {
        volume++;
        System.out.println("음악 플레이어 볼륨: " + volume);
    }

    void volumeDown() {
        volume--;
        System.out.println("음악 플레이어 볼륨: " + volume);
    }

    void showStatus() {
        System.out.println("음악 플레이어 상태 확인");
        if (isOn) {
            System.out.println("음악 플레이어 ON, 볼륨: " + volume);
        } else{
            System.out.println("음악 플레이어 OFF");
        }
    }
```

```java
public class MusicPlayerMain4 {
    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();
        // 음악 플레이어 켜기
        player.on();
        // 볼륨 증가
        player.volumeUp();
        // 볼륨 증가
        player.volumeUp();
        // 볼륨 감소
        player.volumeDown();
        // 음악 플에이어 상태
        player.showStatus();
        // 음악 플레이어 끄기
        player.off();
    }
}
```

> 캡슐화
>
> `MusicPlayer` 클래스에서 이렇게 '속성과 기능'을 하나로 묶어서 기능을 메서드를 통해 외부에 지공하는 것을 캡슐화라 한다.

객체 지향 프로그래밍 덕분에 음악 플레이어 객체를 사용하는 입장에서 진짜 음악 플레이어를 만들고 사용하는 것처럼 친숙하게 느껴진다. 그래서 코드가 더 읽기 쉬운 것은 물론이고, 속성과 기능이 한 곳에 있기 때문에 변경도 더 쉬워진다. 예를 들어서 `MusicPlayer` 내부 코드가 변하는 경우에 다른 코드는 변경하지 않아도 된다. 또 음악 플레이어가 내부에서 출력하는 메시지를 변경할 때도 `MusicPlayer` 내부만 변경하면 된다. 이 경우 `MusicPlayer`를 사용하는 개발자는 코드를 전혀 변경하지 않아도 된다. 물론 외부에서 호출하는 `MusicPlayer` 의 메서드 이름을 변경한다면 `MusicPlayer` 를 사용하는 곳의 코드도 변경해야 한다.
