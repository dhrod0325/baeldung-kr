> 원문 : https://www.baeldung.com/java-super

# super 키워드

## 1. 소개
이 빠른 자습서에서는 super Java 키워드 를 살펴보겠습니다.

간단히 말해서 super 키워드를 사용하여 상위 클래스에 액세스할 수 있습니다.

언어에서 핵심 키워드의 응용 프로그램을 살펴보겠습니다.

## 2. 생성자가 있는  슈퍼 키워드

super() 를 사용 하여 부모 기본 생성자를 호출 할 수 있습니다. 생성자의 첫 번째 명령문이어야 합니다.

이 예에서는 String 인수와 함께 super(message)를 사용합니다.

```java
public class SuperSub extends SuperBase {
    public SuperSub(String message) {
        super(message);
    }
}
```

자식 클래스 인스턴스를 만들고 뒤에서 무슨 일이 일어나는지 봅시다.

```java
SuperSub child = new SuperSub("message from the child class");
```

new 키워드는 **상위 생성자를 먼저 호출하고 String 인수를 전달하는 SuperSub 의 생성자를 호출 합니다.**

## 3. 부모 클래스 변수에 접근하기

메시지 인스턴스 변수를 사용하여 부모 클래스를 생성해 보겠습니다.

```java
public class SuperBase {
    String message = "super class";

    // default constructor

    public SuperBase(String message) {
        this.message = message;
    }
}
```

이제 같은 이름의 변수를 사용하여 자식 클래스를 만듭니다.

```java
public class SuperSub extends SuperBase {
    String message = "child class";

    public void getParentMessage() {
        System.out.println(super.message);
    }
}
```

super 키워드를 사용하여 자식 클래스에서 부모 변수에 액세스할 수 있습니다.

## 4. 메서드 재정의가 있는 슈퍼 키워드

더 진행하기 전에 [메서드 재정의 가이드](https://www.baeldung.com/java-method-overload-override) 를 검토 하는 것이 좋습니다.

부모 클래스에 인스턴스 메서드를 추가해 보겠습니다.

```java
public class SuperBase {
    String message = "super class";

    public void printMessage() {
        System.out.println(message);
    }
}
```

그리고 자식 클래스에서 printMessage() 메서드를 재정의합니다 .

```java
public class SuperSub extends SuperBase {
    String message = "child class";

    public SuperSub() {
        super.printMessage();
        printMessage();
    }

    public void printMessage() {
        System.out.println(message);
    }
}
```

super를 사용하여 자식 클래스에서 재정의된 메서드에 액세스 할 수 있습니다.

생성자의 super.printMessage() 는 SuperBase 에서 부모 메서드를 호출합니다.
