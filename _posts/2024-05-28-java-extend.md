---
title : "[Java] 상속"
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

상속이 왜 필요한지와 상속에 대해서 알아보자.

&nbsp;

# 상속이 필요한 이유

아래 구조를 코드를 통해서 알아보자.

![1716911051359]({{site.url}}/images/2024-05-28-java-extend/1716911051359.png)`

```java
public class ElectricCar {

    public void move() {
        System.out.println("차를 이동합니다.");
    }

    public void charge() {
        System.out.println("충전합니다.");
    }
}
```

```java
public class GasCar1 {
    public void move() {
        System.out.println("차를 이동합니다.");
    }
    public void fillUp() {
        System.out.println("기름을 주유합니다.");
    }
}
```

```java
public class CarMain {

    public static void main(String[] args) {
        ElectricCar1 electricCar = new ElectricCar();
        electricCar.move();
        electricCar.charge();

        GasCar1 gasCar = new GasCar();
        gasCar.move();
        gasCar.fillUp();
    }
```

**결과**

```java
차를 이동합니다.
충전합니다.
차를 이동합니다.
기름을 주유합니다.

Process finished with exit code 0

```

전기차와 가솔린차는 자동차(`Car`)의 좀 더 구체적인 개념이다. 이 둘의 공통 기능은 이동(`move()`)이다. 이런 경우 상속 관계를 사용하는 것이 효과적이다.

&nbsp;

# 상속 관계를 이용해보자.

상속은 객체 지향 프로그래밍의 핵심 요소 중 하나로, 기존 클래스으 필드와 메서드를 새로운 클래스에서 재사용하게 해준다. 기존 클래스의 기능을 그대로 물려받는 것이다. 상속을 사용하려면 `extends`키워드를 사용하면 되낟. **대상은 하나만 선택**할 수 있다.

용어 정리

- 부모 클래스 (슈퍼 클래스) : 상속을 통해 자신의 필드의 메서드를 다른 클래스에 제공하는 클래스
- 자식 클래스 (서브 클래스) : 부모 클래스로부터 필드와 메서드를 상속받는 클래스

![1716911476548]({{site.url}}/images/2024-05-28-java-extend/1716911476548.png)

```java
public class Car {

    public void move() {
        System.out.println("차를 이동합니다.");
    }
}
```

```java
public class ElectricCar extends Car{

    public void charge() {
        System.out.println("충전합니다.");
    }

}

```

```java
public class GasCar extends Car{

    public void fillUp() {
        System.out.println("기름을 주유합니다.");
    }

}

```

```java
public class CarMain {
    public static void main(String[] args) {
        ElectricCar electricCar = new ElectricCar();
        electricCar.move();
        electricCar.charge();

        GasCar gasCar = new GasCar();
        gasCar.move();
        gasCar.fillUp();
    }
}
```

**결과**

```java
차를 이동합니다.
충전합니다.
차를 이동합니다.
기름을 주유합니다.

Process finished with exit code 0

```

전기차와 가솔린차가 `Car`를 상속 받은 덕분에 `electricCar.move()`, `gasCar.move()`를 사용할 수 있다.

상속은 부모의 기능을 자식이 물려받는 것이다. 따라서 자식이 부모의 기능을 물려 받아서 사용할 수 있다. 반대로 부모 클래스는 자식 클래스에 접근할 수 없다. 자식 클래스는 부모 클래스의 기능을 물려 받기 때문에 접근할 수 있지만, 그 반대는 아니다.

&nbsp;

# 상속과 메모리 구조 (중요)

상속 관계를 객체로 생성할 때 메모리 구조를 확인해보자.

## `ElectricCar electricCar = new ElectricCar();`

![1716911865293]({{site.url}}/images/2024-05-28-java-extend/1716911865293.png)

`new ElectricCar()`를 호출하면 `ElectricCar`뿐만 아니라 상속 관계에 있는 `Car`까지 함께 포함해서 인스턴스를 생성한다. 참조값은 `x001`로 하나이지만 실제로 그 안에서는 `Car`, `ElectricCar`라는 두가지 클래스 정보가 공존하는 것이다. 상속이라고 해서 단순하게 부모의 필드와 메서드만 물려 받는게 아니다. 상속 관계를 사용하면 부모 클래스도 함께 포함해서 생성된다. 외부에서 볼때는 하나의 인스턴스를 생성하는 것 같지만 내부에서는 부모와 자식이 모두 생성되고 공간도 구분된다.

## `electricCar.charge()`호출

![1716912012891]({{site.url}}/images/2024-05-28-java-extend/1716912012891.png)

`electricCar.charge()`를 호출하면 참조값을 확인해서 `x001.charge()`를 호출한다. 따라서 `x001`을 찾아서 `charge()`를 호출하면 되는 것이다. 그런데 상속 관계의 경우에는 내부의 부모와 자식이 모두 존재한다. 이때 부모인 `Car`를 통해서 `charge()`를 찾을지 아니면 `ElectricCar`를 통해서 `charge()`를 찾을지 선택해야 한다. 이때는 호***출하는 변수의 타입(클래스)을 기준으로 선택***한다. `electricCar`변수의 타입이 `ElectricCar`이므로 인스턴스 내부에 같은 타입인 `ElectricCar`를 통해서 `charge()`를 호출한다.

## `elecricCar.move()`호출

![1716912227641]({{site.url}}/images/2024-05-28-java-extend/1716912227641.png)

`electricCar.move()`를 호출하면 먼저 `x001`참조로 이동한다. 내부에는 `Car`, `ElecricCar`두가지 타입이 있다. 이때 호출하는 변수인 `electricCar`의 타입이 `ElectricCar`이므로 이 타입을 선택한다. 그런데 `ElectricCar`에는 `move()`메서드가 없다. 상속 관계에서는 자식 타입에 대한 기능이 없으면 부모 타입으로 올라가서 찾는다. 이 경우 `ElectricCar`의 부모인 `Car`로 올라가서 `move()`를 찾는다. 부모인 `Car`에 `move()`가 있으므로 부모에 있는 `move()`메서드를 호출한다.

만약 부모에서도 해당 기능을 찾지 못하면 더 상위 부모에서 필요한 기능을 찾아본다. 부모에 부모로 계속 올라가면서 필드나 메서드를 찾는 것이다. 계속 찾아도 없으면 컴파일 오류가 발생한다.
