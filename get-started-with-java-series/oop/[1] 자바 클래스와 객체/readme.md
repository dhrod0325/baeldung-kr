> 원문 : https://www.baeldung.com/java-classes-objects 

# 자바 클래스와 객체

## 1. 개요
이 빠른 자습서에서는 Java 프로그래밍 언어의 두 가지 기본 빌딩 블록인 클래스와 객체를 살펴보겠습니다. 
이것들은 우리가 실제 엔티티를 모델링하는 데 사용하는 객체 지향 프로그래밍(OOP)의 기본 개념입니다.
OOP에서 클래스는 객체에 대한 청사진 또는 템플릿입니다. 엔티티 유형을 설명하는 데 사용합니다.
반면에 객체는 클래스에서 생성된 살아있는 개체입니다. 그들은 필드 내에 특정 상태를 포함하고 메서드와 함께 특정 동작을 나타냅니다.

## 2. 수업
간단히 말해서, 클래스는 객체의 정의 또는 유형을 나타냅니다. Java에서 클래스는 필드, 생성자 및 메소드를 포함할 수 있습니다.
Car 를 나타내는 간단한 Java 클래스를 사용하는 예를 살펴보겠습니다 .

```java
class Car {

    // fields
    String type;
    String model;
    String color;
    int speed;

    // constructor
    Car(String type, String model, String color) {
        this.type = type;
        this.model = model;
        this.color = color;
    }
    
    // methods
    int increaseSpeed(int increment) {
        this.speed = this.speed + increment;
        return this.speed;
    }
    
    // ...
}
```
이 Java 클래스는 일반적으로 자동차를 나타냅니다. 이 클래스에서 모든 유형의 자동차를 만들 수 있습니다. 
필드를 사용하여 상태를 유지하고 생성자를 사용하여 이 클래스에서 개체를 만듭니다.
모든 Java 클래스에는 기본적으로 빈 생성자가 있습니다. 위에서 했던 것처럼 특정 구현을 제공하지 않는 경우 사용합니다. 
기본 생성자가 Car 클래스 를 찾는 방법은 다음과 같습니다.

> Car(){}

이 생성자는 단순히 객체의 모든 필드를 기본값으로 초기화합니다. 문자열은 null 로 초기화 되고 정수는 0으로 초기화됩니다.
이제 객체를 생성할 때 객체에 필드가 정의되기를 원하기 때문에 클래스에는 특정 생성자가 있습니다.

> Car(String type, String model) {
// ...
}

요약하자면, 우리는 자동차를 정의하는 클래스를 작성했습니다. 그 속성은 클래스의 개체 상태를 포함하는 필드로 설명되고, 그 동작은 메서드를 사용하여 설명됩니다.

## 3. 개체
클래스는 **컴파일 시간에 변환되지만 개체는 런타임에 클래스에서 생성됩니다** .
클래스의 객체를 인스턴스라고 하며 생성자를 사용하여 객체를 생성하고 초기화합니다.

```java
Car focus = new Car("Ford", "Focus", "red");
Car auris = new Car("Toyota", "Auris", "blue");
Car golf = new Car("Volkswagen", "Golf", "green");
```

이제 우리는 단일 클래스에서 모두 다른 Car 객체를 만들었습니다. **한 곳에서 객체들을 정의한 다음 여러 곳에서 여러 번 재사용하는 것이 모든 것의 요점입니다.**

지금까지 3개의 Car 개체가 있으며 속도가 0이므로 모두 주차되어 있습니다. 우리는 증가 속도 메소드를 호출하여 이것을 변경할 수 있습니다:

```java
focus.increaseSpeed(10);
auris.increaseSpeed(20);
golf.increaseSpeed(30);
```

이제 우리는 자동차의 상태를 변경했습니다. 모두 다른 속도로 움직이고 있습니다.
게다가, 우리는 우리 클래스, 그 생성자, 필드, 메소드에 대한 접근 제어를 정의할 수 있고 정의해야 합니다. 
다음 섹션에서 볼 수 있듯이 액세스 한정자를 사용하여 그렇게 할 수 있습니다.

## 4. 액세스 수정자
이전 예제에서는 코드를 단순화하기 위해 액세스 수정자를 생략했습니다. 그렇게 함으로써 우리는 실제로 기본 package-private 수정자를 사용했습니다. 
해당 수정자는 동일한 패키지의 다른 클래스에서 클래스에 대한 액세스를 허용합니다.

일반적으로 생성자에 대해 public 한정자를 사용하여 다른 모든 객체에서 액세스를 허용합니다.
```java
public Car(String type, String model, String color) {
    // ...
}
```

우리 클래스의 모든 필드와 메서드는 또한 특정 한정자에 의해 액세스 제어를 정의해야 합니다. 
클래스에는 일반적으로 공개 수정자가 있지만 필드를 비공개 로 유지하는 경향이 있습니다 .

필드는 객체의 상태를 유지하므로 해당 상태에 대한 액세스를 제어하려고 합니다. 일부는 비공개 로 유지하고 나머지 는 공개 로 유지할 수 있습니다 . getter 및 setter라는 특정 메서드를 사용하여 이를 달성합니다.

완전히 지정된 액세스 제어가 있는 클래스를 살펴보겠습니다.

```java
public class Car {
    private String type;
    // ...

    public Car(String type, String model, String color) {
       // ...
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public int getSpeed() {
        return speed;
    }

    // ...
}
```

우리 클래스는  public 으로 표시되어 있어 모든 패키지에서 사용할 수 있습니다. 또한 생성자는 public 입니다. 
즉, 다른 객체 내부에서 이 클래스에서 객체를 생성할 수 있습니다.

우리의 필드는  private 로 표시되어 있습니다. 즉, 개체에서 직접 액세스할 수 없지만 getter 및 setter를 통해 액세스할 수 있습니다.

type 및 model 필드에는 객체의 내부 데이터가 있기 때문에 getter 및 setter가 없습니다. 초기화하는 동안 생성자를 통해서만 정의할 수 있습니다.

또한 color는 액세스 및 변경할 수 있고 speed는 엑세스는 가능하지만 변경할 수는 없습니다.
우리는 전문화된 public 메소드 인 growthSpeed() 및 reductionSpeed() 를 통해 속도 조정을 시행 했습니다.

즉, 액세스 제어를 사용하여 개체의 상태를 캡슐화합니다.
