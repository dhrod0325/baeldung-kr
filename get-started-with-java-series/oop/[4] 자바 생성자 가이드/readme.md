> 원문 : https://www.baeldung.com/java-constructors

# 자바 생성자 가이드

## 1. 소개
생성자는 객체 지향 설계의 문지기입니다 .

이 튜토리얼에서는 생성되는 객체 의 내부 상태 를 초기화하기 위한 단일 위치의 역할을 하는 방법을 볼 것 입니다.

은행 계좌를 나타내는 간단한 객체를 만들어 보겠습니다.

## 2. 은행 계좌 설정
은행 계좌를 나타내는 클래스를 만들어야 한다고 상상해보십시오. 이름, 생성 날짜 및 잔액이 포함됩니다.

또한 세부 정보를 콘솔에 출력하기 위해 toString 메서드를 재정의해 보겠습니다 .

```java
class BankAccount {
    String name;
    LocalDateTime opened;
    double balance;
    
    @Override
    public String toString() {
        return String.format("%s, %s, %f", 
          this.name, this.opened.toString(), this.balance);
    }
}
```

이제 이 클래스는 은행 계좌에 대한 정보를 저장하는 데 필요한 모든 필드를 포함하지만 아직 생성자는 포함하지 않습니다.

**즉, 새 객체를 생성하면 필드 값이 초기화되지 않습니다.**
```java
BankAccount account = new BankAccount();
account.toString();
```

위의 toString  메서드를 실행하면 개체 이름 과 열린 개체 가 여전히 null 이기 때문에 예외가 발생합니다.

```
java.lang.NullPointerException
    at com.baeldung.constructors.BankAccount.toString(BankAccount.java:12)
    at com.baeldung.constructors.ConstructorUnitTest
      .givenNoExplicitContructor_whenUsed_thenFails(ConstructorUnitTest.java:23)
```

## 3. 인수가 없는 생성자

생성자로 수정해 보겠습니다.

```java
class BankAccount {
    public BankAccount() {
        this.name = "";
        this.opened = LocalDateTime.now();
        this.balance = 0.0d;
    }
}
```

방금 작성한 생성자에 대한 몇 가지 사항에 주목하십시오. 

첫째, 메서드이지만 반환 유형이 없습니다. 생성자가 생성하는 개체의 형식을 암시적으로 반환하기 때문입니다.  
이제 new BankAccount() 를 호출 하면 위의 생성자가 호출됩니다.

둘째, 인수가 필요하지 않습니다. 이 특정 종류의 생성자를 o-인수 생성자 라고 합니다. 
그런데 왜 처음에는 필요하지 않았습니까? 생성자를 명시적으로 작성하지 않으면 컴파일러가 인수가 없는 기본 생성자를 추가하기 때문 입니다. 
이것이 우리가 생성자를 명시적으로 작성하지 않았음에도 불구하고 처음으로 객체를 생성할 수 있었던 이유입니다. 기본적으로 인수가 없는 생성자는 모든 멤버를 기본값으로 설정합니다.
객체의 경우 이는 null이며 이전에 본 예외가 발생했습니다.

## 4. 매개변수화된 생성자
이제 생성자의 진정한 이점은 객체에 상태를 주입할 때 캡슐화 를 유지하는 데 도움이 된다는 것입니다.

따라서 이 은행 계좌로 정말 유용한 작업을 수행하려면 실제로 일부 초기 값을 객체에 주입할 수 있어야 합니다.

그렇게 하려면 매개변수화된 생성자 , 즉 몇 가지 인수를 취하는 생성자를 작성해 보겠습니다 .

```java
class BankAccount {
    public BankAccount() { ... }
    public BankAccount(String name, LocalDateTime opened, double balance) {
        this.name = name;
        this.opened = opened;
        this.balance = balance;
    }
}
```
이제 BankAccount 클래스 를 사용하여 유용한 작업을 수행할 수 있습니다 .
```java
LocalDateTime opened = LocalDateTime.of(2018, Month.JUNE, 29, 06, 30, 00);
BankAccount account = new BankAccount("Tom", opened, 1000.0f); 
account.toString();
```

이제 클래스에는 2개의 생성자가 있습니다. 인수가 없는 명시적 생성자 및 매개변수화된 생성자.

생성자를 원하는 만큼 생성할 수 있지만 너무 많이 생성하지 않는 것이 좋습니다. 이것은 약간 혼란스러울 것입니다.

코드에서 생성자가 너무 많으면 몇 가지 생성 디자인 패턴 이 도움이 될 수 있습니다.

## 5. 복사 생성자
생성자는 초기화만으로 제한될 필요는 없습니다. 다른 방법으로 개체를 만드는 데 사용할 수도 있습니다. 
기존 계정에서 새 계정을 만들 수 있어야 한다고 상상해 보십시오.
새 계정은 이전 계정과 이름이 같아야 하며 오늘 생성 날짜와 자금이 없어야 합니다. 복사 생성자를 사용하여 그렇게 할 수 있습니다.
```java
public BankAccount(BankAccount other) {
    this.name = other.name;
    this.opened = LocalDateTime.now();
    this.balance = 0.0f;
}
```
이제 다음 동작이 있습니다.
```java
LocalDateTime opened = LocalDateTime.of(2018, Month.JUNE, 29, 06, 30, 00);
BankAccount account = new BankAccount("Tim", opened, 1000.0f);
BankAccount newAccount = new BankAccount(account);

assertThat(account.getName()).isEqualTo(newAccount.getName());
assertThat(account.getOpened()).isNotEqualTo(newAccount.getOpened());
assertThat(newAccount.getBalance()).isEqualTo(0.0f);
```

## 6. 연결 생성자
물론 일부 생성자 매개변수를 유추하거나 일부에 기본값을 제공할 수 있습니다.

예를 들어 이름만 있는 새 은행 계좌를 만들 수 있습니다.

따라서 name 매개변수를 사용하여 생성자를 만들고 다른 매개변수에 기본값을 지정해 보겠습니다.

```java
public BankAccount(String name, LocalDateTime opened, double balance) {
    this.name = name;
    this.opened = opened;
    this.balance = balance;
}

public BankAccount(String name) {
    this(name, LocalDateTime.now(), 0.0f);
}
```

this 키워드 로 다른 생성자를 호출합니다.

슈퍼클래스 생성자를 연결하려면 this 대신 super 를 사용해야 한다는 것을 기억해야 합니다 .

또한 this 또는 super 식은 항상 첫 번째 명령문이어야 함을 기억하십시오.

## 7. 값 유형
Java에서 생성자의 흥미로운 사용은 Value Objects 생성에 있습니다. 값 개체는 초기화 후 내부 상태를 변경하지 않는 개체입니다.

즉, 개체는 변경할 수 없습니다 . Java의 불변성은 약간 미묘한 차이가 있으므로 객체를 만들 때 주의를 기울여야 합니다.

변경할 수 없는 클래스를 만들어 보겠습니다.

```java
class Transaction {
    final BankAccount bankAccount;
    final LocalDateTime date;
    final double amount;

    public Transaction(BankAccount account, LocalDateTime date, double amount) {
        this.bankAccount = account;
        this.date = date;
        this.amount = amount;
    }
}
```

이제 클래스의 멤버를 정의할 때 final 키워드를 사용합니다. 즉, 각 멤버는 클래스의 생성자 내에서만 초기화될 수 있습니다.

나중에 다른 메서드 내에서 다시 할당할 수 없습니다. 우리는 그 값을 읽을 수는 있지만 변경할 수는 없습니다.

Transaction 클래스에 대해 여러 생성자를 만드는 경우 각 생성자는 모든 최종 변수를 초기화해야 합니다. 그렇게 하지 않으면 컴파일 오류가 발생합니다.
