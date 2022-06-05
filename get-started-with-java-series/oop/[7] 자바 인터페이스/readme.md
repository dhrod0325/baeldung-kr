> 원본 : https://www.baeldung.com/java-interfaces

# 자바 인터페이스

## 1. 개요
   이 튜토리얼에서는 Java의 인터페이스에 대해 이야기할 것입니다. 또한 Java가 이를 사용하여 다형성 및 다중 상속을 구현하는 방법도 살펴보겠습니다.

## 2. Java에서 인터페이스란 무엇입니까?

Java에서 인터페이스는 메소드와 상수 변수의 모음을 포함하는 추상 유형입니다. Java의 핵심 개념 중 하나이며 추상화, 다형성 및 다중 상속을 달성하는데 사용됩니다.

Java 인터페이스의 간단한 예를 살펴보겠습니다.

```java
public interface Electronic {

    // Constant variable
    String LED = "LED";

    // Abstract method
    int getElectricityUse();

    // Static method
    static boolean isEnergyEfficient(String electtronicType) {
        if (electtronicType.equals(LED)) {
            return true;
        }
        return false;
    }

    //Default method
    default void printDescription() {
        System.out.println("Electronic Description");
    }
}
```
implements 키워드 를 사용하여 Java 클래스에서 인터페이스를 구현할 수 있습니다 .

다음으로 방금 만든 전자 인터페이스를 구현하는 Computer 클래스도 만들어 보겠습니다.

```java
public class Computer implements Electronic {
    @Override
    public int getElectricityUse() {
        return 1000;
    }
}
```

### 2.1. 인터페이스 생성 규칙
인터페이스에서 다음을 사용할 수 있습니다.

- 상수 변수
- 추상 메서드
- 정적 메서드
- 기본 방법

또한 다음 사항을 기억해야 합니다.

- 인터페이스를 직접 인스턴스화할 수 없습니다.
- 인터페이스는 메서드나 변수가 없는 비어 있을 수 있습니다.
- 인터페이스 정의에서 마지막 단어를 사용할 수 없습니다 . 컴파일러 오류가 발생하기 때문입니다.
- 모든 인터페이스 선언에는  공용 또는 기본 액세스 수정자가 있어야 합니다. 추상 수정 자는 컴파일러에 의해 자동으로 추가됩니다. 인터페이스 메서드는  보호 할 수  없거나 최종적 일 수 없습니다.
- Java 9까지 인터페이스 메소드는 private 일 수 없었습니다 . 그러나 Java 9는 인터페이스에서 개인 메소드 를 정의할 수 있는 가능성을 도입했습니다.
- 인터페이스 변수는  정의에 따라 public , static 및 final 입니다. 우리는 그들의 가시성을 변경할 수 없습니다

## 3. 그것을 사용하여 무엇을 얻을 수 있습니까?

### 3.1. 행동 기능
관련 없는 클래스에서 사용할 수 있는 특정 동작 기능을 추가하기 위해 인터페이스를 사용합니다. 
예를 들어, Comparable, Comparator 및 Cloneable 은 관련 없는 클래스에 의해 구현될 수 있는 Java 인터페이스입니다.
다음은 Employee  클래스의 두 인스턴스를 비교하는 데 사용되는 Comparator 인터페이스의 예입니다. 

```java
public class Employee {

    private double salary;

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }
}

public class EmployeeSalaryComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee employeeA, Employee employeeB) {
        if (employeeA.getSalary() < employeeB.getSalary()) {
            return -1;
        } else if (employeeA.getSalary() > employeeB.getSalary()) { 
            return 1;
        } else {
            return 0;
        }
    }
}
```

### 3.2. 다중 상속

Java 클래스는 단일 상속을 지원합니다. 그러나 인터페이스를 사용하여 다중 상속을 구현할 수도 있습니다.
예를 들어, 아래 예제에서 Car클래스가 Fly 및 Transform 인터페이스를 구현 한다는 것을 알 수 있습니다. 이렇게 하면 fly 및 transform 메서드를 상속합니다.

```java
public interface Transform {
    void transform();
}

public interface Fly {
    void fly();
}

public class Car implements Fly, Transform {
    @Override
    public void fly() {
        System.out.println("I can Fly!!");
    }

    @Override
    public void transform() {
        System.out.println("I can Transform!!");
    }
}
```

### 3.3. 다형성
다형성 이란 무엇 인가? 런타임 중에 개체가 다른 형식을 취할 수 있는 기능입니다. 더 구체적으로 말하면 런타임에 특정 개체 유형과 관련된 재정의 메서드를 실행하는 것입니다.

Java에서는 인터페이스를 사용하여 다형성을 구현할 수 있습니다. 예를 들어,  Shape 인터페이스는 다양한 형태를 취할 수 있습니다. Circle 또는 Square일 수 있습니다.

먼저 Shape 인터페이스를 정의해 보겠습니다 .

```java
public interface Shape {
    String name();
}
```

이제  Circle 클래스도 만들어 보겠습니다.

```java
public class Circle implements Shape {
    @Override
    public String name() {
        return "Circle";
    }
}
```

또한  Square 클래스:

```java
public class Square implements Shape {
    @Override
    public String name() {
        return "Square";
    }
}
```

마지막으로 Shape 인터페이스와 그 구현 을 사용하여 다형성이 작동하는 것을 볼 시간 입니다. 일부 Shape 개체를 인스턴스화 하고 List 에 추가하고 마지막으로 루프에서 이름을 인쇄해 보겠습니다.

```java
List<Shape> shapes = new ArrayList<>();
Shape circleShape = new Circle();
Shape squareShape = new Square();

shapes.add(circleShape);
shapes.add(squareShape);

for (Shape shape : shapes) {
    System.out.println(shape.name());
}
```
## 4. 인터페이스의 기본 메소드
Java 7 이하의 기존 인터페이스는 이전 버전과의 호환성을 제공하지 않습니다.

이것이 의미하는 바는 Java 7 또는 이전 버전으로 작성된 레거시 코드가 있고 기존 인터페이스에 추상 메서드를 추가하기로 결정한 경우 해당 인터페이스를 구현하는 모든 클래스가 새 추상 메서드를 재정의해야 한다는 의미 입니다. 그렇지 않으면 코드가 깨집니다.

Java 8 은 선택 사항이며 인터페이스 수준에서 구현할 수 있는 기본 메서드를 도입하여 이 문제를 해결했습니다.

## 5. 인터페이스 상속 규칙
인터페이스를 통해 다중 상속을 달성하려면 몇 가지 규칙을 기억해야 합니다. 이에 대해 자세히 살펴보겠습니다.

### 5.1. 다른 인터페이스를 확장하는 인터페이스
인터페이스가 다른 인터페이스를 확장 하면 해당 인터페이스의 모든 추상 메서드를 상속합니다. HasColor  및  Shape 의 두 인터페이스를 만드는 것으로 시작하겠습니다  .

```java
public interface HasColor {
    String getColor();
}

public interface Box extends HasColor {
    int getHeight()
}
```

위의 예에서  Box는 extends 키워드를 사용하여 HasColor 에서 상속 합니다. 그렇게 함으로써 Box인터페이스는 getColor를 상속받습니다. 결과적으로  Box 인터페이스에는 getColor 및 getHeight 의 두 가지 메서드가 있습니다.

### 5.2. 인터페이스를 구현하는 추상 클래스

추상 클래스는 인터페이스를 구현할 때 모든 추상 및 기본 메서드를 상속합니다. Transform 인터페이스와 이를 구현하는 추상 클래스 Vehicle 을 살펴보겠습니다.

```java
public interface Transform {
    
    void transform();
    default void printSpecs(){
        System.out.println("Transform Specification");
    }
}

public abstract class Vehicle implements Transform {}
```
이 예제에서 Vehicle 클래스는 추상  변환  메서드와 기본  printSpecs 메서드의 두 가지 메서드를 상속합니다.

## 6. 기능적 인터페이스
Java는 초기부터 Comparable  (Java 1.2 이후) 및 Runnable (Java 1.0 이후)과 같은 많은 기능적 인터페이스를 가지고 있습니다.

Java 8은 Predicate , Consumer 및 Function 과 같은 새로운 기능 인터페이스를 도입 했습니다. 이에 대한 자세한 내용은  Java 8의 기능적 인터페이스에 대한 자습서를 참조하십시오.
