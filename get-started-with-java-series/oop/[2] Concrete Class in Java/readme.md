> 원문 : https://www.baeldung.com/java-concrete-class

# Concrete Class in Java

## 1. 소개
이 빠른 가이드에서는 Java의 "Concrete Class"라는 용어에 대해 설명합니다 .
먼저 용어를 정의하겠습니다. 그런 다음 인터페이스 및 추상 클래스와 어떻게 다른지 알아보겠습니다.

## 2. Concrete Class?
Concrete Class는 new 키워드 를 사용하여 인스턴스를 만들 수 있는 클래스입니다 .

즉,  청사진의 완전한 구현입니다 . 구체적인 구현이 완료되었습니다.

예를 들어 Car 클래스를 상상해보십시오.

```java
public class Car {
    public String honk() {
        return "beep!";
    }

    public String drive() {
        return "vroom";
    }
}
```

모든 메소드가 구현되어 있기 때문에 이를 Concrete Class라고 부르며 인스턴스화 할 수 있습니다.

```java
Car car = new Car();
```

JDK의 Concrete Class의 몇 가지 예는 HashMap , HashSet , ArrayList 및 LinkedList 입니다.

## 3. 자바 추상화 vs. 구체 클래스
그러나 모든 Java 유형이 모든 메소드를 구현하는 것은 아닙니다. 추상화 라고도 하는 이러한 유연성을 통해 모델링하려는 도메인에 대해 보다 일반적인 용어로 생각할 수 있습니다.

Java에서는 인터페이스와 추상 클래스를 사용하여 추상화를 달성할 수 있습니다.

이러한 다른 클래스와 비교하여 Concrete Class를 더 잘 살펴보겠습니다.


### 3.1. 인터페이스
인터페이스는 클래스의 청사진입니다. 즉, 구현되지 않은 메서드 서명 모음입니다.

```java
interface Driveable {
    void honk();
    void drive();
}
```

class 대신  interface 키워드를 사용합니다.

**Driveable에는 구현되지 않은 메서드가 있으므로 new 키워드 로 인스턴스화할 수 없습니다.**

그러나 Car  와 같은 Concrete Class 는 이러한 메서드를 구현할 수 있습니다.

JDK는 Map , List 및 Set 과 같은 여러 인터페이스를 제공합니다.


### 3.2. 추상 클래스
추상 클래스는 구현되지 않은 메서드가 있는 클래스 이지만 실제로는 다음 두 가지를 모두 가질 수 있습니다.

```java
public abstract class Vehicle {
    public abstract String honk();

    public String drive() {
        return "zoom";
    }
}
```

abstract 키워드로 추상 클래스를 표시한다는 점에 유의하십시오.

다시, Vehicle 에는 **구현되지 않은 메서드 honk 가 있으므로 new  키워드 를 사용할 수 없습니다.**

JDK에서 추상 클래스의 몇 가지 예는 AbstractMap 및 AbstractList 입니다.

### 3.3. 구체적인 수업
대조적으로 Concrete Class에는 구현되지 않은 메서드가 없습니다. 구현이 상속되는지 여부에 관계없이 각 메서드에 구현이 있는 한 클래스는 구체적입니다.

구체 클래스는 이전의 Car 예제 처럼 간단할 수 있습니. 인터페이스를 구현하고 추상 클래스를 확장할 수도 있습니다.

```java
public class FancyCar extends Vehicle implements Driveable {
    public String honk() { 
        return "beep";
    }
}
```

FancyCar클래스는 경적을 위한 구현을 제공하고  Vehicle 에서  드라이브 구현을 상속합니다.

따라서 구현되지 않은 메서드가 없습니다. 따라서 new 키워드로 FancyCar 클래스 인스턴스를 생성할 수 있습니다 .

```java
FancyCar car = new FancyCar();
```

또는 간단히 말해서 추상이 아닌 모든 클래스를 Concrete Class라고 부를 수 있습니다.
