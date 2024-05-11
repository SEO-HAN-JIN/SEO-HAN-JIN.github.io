---
title : "[Java] 캡슐화"
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

캡슐화에 대해서 알아보자.

&nbsp;

# ❓ 캡슐화란

`캡슐화(Encapsulcation)`는 객체 지향 프로그래밍의 중요한 개념 중 하나이다. 

캡슐화는 데이터와 해당 데이터를 처리하는 메서드를 하나로 묶어서 외부에서의 접근을 제한하는 것을 말한다.

캡슐화를 통해 데이터의 직접적인 변경을 방지하거나 제한할 수 있다.

캡슐화는 쉽게 이야기해서 속성과 기능을 하나로 묶고, 외부에 꼭 필요한 기능만 노출하고 나머지는 모두 내부로 숨기는 것이다.

## 1. 데이터를 숨겨라

- 객체에는 속성(데이터)와 기능(메서드)이 있다. 캡슐화에서 가장 필수로 숨겨야 하는 것은 속성(데이터)이다.
- 객체 내부의 데이터를 외부에서 함부로 접근하게 두면, 클래스 안에서 데이터를 다루는 모든 로직을 무시하고 데이터를 변경할 수 있다.
- 객체의 데이터는 객체가 제공하는 기능인 메서드를 통해서 접근해야 한다.

## 2. 기능을 숨겨라

- 객체의 기능 중에서 외부에서 사용하지 않고 내부에서만 사용하는 기능들이 있다. 이런 기능도 모두 감추는 것이 좋다.
- 즉, 데이터는 모두 숨기고 기능은 꼭 필요한 기능만 노출하는 것이 좋은 캡슐화이다.

&nbsp;

# ✔실습

계좌에 돈을 입금, 출금 하는 로직을 만들어보자. 데이터와 기능은 `BankAccount`클래스에 있고, 이것을 동작하는 `BankAccountMain`클래스가 있다.

```java
public class BankAccount {

    private int balance;

    public BankAccount() {
        balance = 0;
    }

    // public 메서드 : deposit
    public void deposit(int amount) {
        if (isAmountValid(amount)) {
            balance += amount;
        } else {
            System.out.println("유효하지 않는 금액입니다.");
        }
    }

    // public 메서드 : withdraw 출금
    public void withdraw(int amount) {
        if (isAmountValid(amount) && balance - amount >= 0) {
            balance -= amount;
        } else {
            System.out.println("유효하지 않은 금액이거나 잔액이 부족합니다.");
        }
    }

    // public 메서드 : getBalance
    public int getBalance() {
        return balance;
    }

    /*
        isAmountValid() : 입력 금액을 검증하는 기능은 내부에서만 필요한 기능이다. 따라서 private 을 사용했다.
     */
    private boolean isAmountValid(int amount) {
        // 금액이 0보다 커야함
        return amount > 0;
    }
}
```

```java
public class BankAccountMain {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        account.deposit(10000);
        account.withdraw(3000);
        System.out.println("balance = " + account.getBalance());
    }
}

```

`isAmountValid()`메서드 같은 경우 내부에서만 필요한 기능이기 때문에 `private`을 사용하였다. 만약 `public`으로 되어있다면 이 기능을 만들지 않은 다른 개발자가 해당 메서드를 보고 써야할지 말아야할지 혼동이 올 수 있기 때문에 `private`으로 해주어서 해당 기능을 사용하는 복잡도를 낮출 수 있다.
