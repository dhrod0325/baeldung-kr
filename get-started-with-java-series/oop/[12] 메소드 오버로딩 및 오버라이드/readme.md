> 원문 : https://www.baeldung.com/java-method-overload-override

# 메소드 오버로딩 및 오버라이드

## 1. 개요
메소드 오버로딩 및 오버라이드는 Java 프로그래밍 언어의 핵심 개념이므로 심층적으로 살펴볼 가치가 있습니다.

이 기사에서 우리는 이러한 개념의 기본 사항을 배우고 어떤 상황에서 유용할 수 있는지 알아볼 것입니다.

## 2. 메소드 오버로딩
메서드 오버로딩은 응집력 있는 클래스 API를 정의할 수 있는 강력한 메커니즘입니다. 메서드 오버로딩이 왜 그렇게 중요한 기능인지 더 잘 이해하기 위해 간단한 예를 살펴보겠습니다.

두 개의 숫자, 세 개의 숫자 등을 곱하는 다른 방법을 구현하는 단순한 유틸리티 클래스를 작성했다고 가정합니다.

만약 우리가 메소드에 오도하거나 모호한 이름을 부여했다면, 예를 들어 multiply2() , multiply33() , multiply44(), 이것은 잘못 설계된 클래스 API가 될 것입니다. 여기에서 메소드 오버로딩이 작동합니다.

간단히 말해서, 우리는 두 가지 다른 방법으로 메소드 오버로딩을 구현할 수 있습니다:

- 이름은 같지만 인수 개수가 다른 두 개 이상의 메서드 구현
- 이름은 같지만 다른 유형의 인수를 사용하는 두 개 이상의 메서드 구현

### 2.1. 다른 수의 인수

Multiplier 클래스는 단순히 다른 수의 인수를 사용하는 두 가지 구현을 정의 하여 multiplier () 메서드를 오버로드하는 방법을 간단히 보여줍니다.

```java
public class Multiplier {
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public int multiply(int a, int b, int c) {
        return a * b * c;
    }
}
```

### 2.2. 다른 유형의 인수

유사하게, 우리는 다른 유형의 인수를 허용하도록 하여 multiply() 메서드를 오버로드할 수 있습니다.

```java
public class Multiplier {
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public double multiply(double a, double b) {
        return a * b;
    }
}
```

또한 두 가지 유형의 메서드 오버로딩으로 Multiplier 클래스를 정의하는 것이 합법적 입니다.

```java
public class Multiplier {
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public int multiply(int a, int b, int c) {
        return a * b * c;
    }
    
    public double multiply(double a, double b) {
        return a * b;
    }
}
```

**그러나 반환 유형만 다른 두 가지 메서드 구현을 가질 수는 없습니다**

이유를 이해하려면 다음 예를 살펴보겠습니다.

```java
public int multiply(int a, int b) { 
    return a * b; 
}
 
public double multiply(int a, int b) { 
    return a * b; 
}
```

이 경우 코드는 메서드 **호출의 모호성(컴파일러는 int를 리턴하는 메소드를 사용할 것인지 double을 리턴하는 메소드를 사용할 것인지 판단 할 수 없다) 때문에 컴파일되지 않을 것입니다**. 컴파일러는 호출할 multiply() 의 구현을 알지 못할 것입니다.

### 2.3. 유형 프로모션

메소드 오버로딩이 제공하는 깔끔한 기능 중 하나는 이른바 유형 승격(일명 확장 기본 변환) 입니다.

간단히 말해서, 오버로드된 메서드와 특정 메서드 구현에 전달된 인수의 형식이 일치하지 않는 경우 지정된 형식이 다른 형식으로 암시적으로 승격됩니다.

유형 승격이 작동하는 방식을 더 명확하게 이해하려면 다음과 같은 multiply() 메서드 구현을 고려하세요.

```java
public double multiply(int a, long b) {
    return a * b;
}

public int multiply(int a, int b, int c) {
    return a * b * c;
}
```

이제 두 개의 int 인수로 메서드를 호출하면 두 번째 인수가 long 으로 승격됩니다. 이 경우 두 개의 int 인수 가 있는 메서드의 일치하는 구현이 없기 때문 입니다.

유형 승격을 보여주기 위한 빠른 단위 테스트를 살펴보겠습니다.

```java
@Test
public void whenCalledMultiplyAndNoMatching_thenTypePromotion() {
    assertThat(multiplier.multiply(10, 10)).isEqualTo(100.0);
}
```

반대로, 일치하는 구현으로 메서드를 호출하면 유형 승격이 발생하지 않습니다.

```java
@Test
public void whenCalledMultiplyAndMatching_thenNoTypePromotion() {
    assertThat(multiplier.multiply(10, 10, 10)).isEqualTo(1000);
}
```

다음은 메서드 오버로딩에 적용되는 형식 승격 규칙의 요약입니다.

- byte 는 short, int, long, float 또는 double 로 승격될 수 있습니다.
- short 는 int, long, float 또는 double 로 승격될 수 있습니다.
- char 는 int, long, float 또는 double 로 승격될 수 있습니다.
- int 는 long, float 또는 double 로 승격될 수 있습니다.
- long 은 float 또는 double 로 승격될 수 있습니다.
- float 는 double 로 승격될 수 있습니다.

## 2.4. 정적 바인딩

특정 메서드 호출을 메서드 본문에 연결하는 기능을 바인딩이라고 합니다.

메서드 오버로딩의 경우 바인딩은 컴파일 타임에 정적으로 수행되므로 정적 바인딩이라고 합니다.

컴파일러는 단순히 메서드의 서명을 확인하여 컴파일 타임에 바인딩을 효과적으로 설정할 수 있습니다.

## 3. 메소드 오버라이드
메서드 재정의를 사용하면 기본 클래스에 정의된 메서드에 대해 하위 클래스에서 세분화된 구현을 제공할 수 있습니다.

메서드 재정의는 강력한 기능인 반면 , 이는 OOP 의 가장 큰 기둥 중 하나인 상속 사용의 논리적 결과라는 점을 고려 하면 언제 어디서 사용할지 유스 케이스별로 신중하게 분석해야 합니다 .

이제 간단한 상속 기반("is-a") 관계를 만들어 메서드 재정의를 사용하는 방법을 살펴보겠습니다.

다음은 기본 클래스입니다.

```java
public class Vehicle {
    
    public String accelerate(long mph) {
        return "The vehicle accelerates at : " + mph + " MPH.";
    }
    
    public String stop() {
        return "The vehicle has stopped.";
    }
    
    public String run() {
        return "The vehicle is running.";
    }
}
```

다음은 고안된 하위 클래스입니다

```java
public class Car extends Vehicle {

    @Override
    public String accelerate(long mph) {
        return "The car accelerates at : " + mph + " MPH.";
    }
}
```

위의 계층 구조에서 우리는 하위 유형 Car 에 대한 보다 세련된 구현을 제공하기 위해 단순히 accelerate() 메서드를 재정의했습니다.

여기에서 응용 프로그램이 Vehicle 클래스 의 인스턴스를 사용하는 경우 accelerate() 메서드 의 두 구현 모두 동일한 서명과 동일한 반환 유형을 갖기 때문에 Car 인스턴스 에서도 작동할 수 있음을 분명히 알 수 있습니다.

Vehicle 및 Car 클래스 를 확인하기 위해 몇 가지 단위 테스트를 작성해 보겠습니다 .

```java
@Test
public void whenCalledAccelerate_thenOneAssertion() {
    assertThat(vehicle.accelerate(100))
      .isEqualTo("The vehicle accelerates at : 100 MPH.");
}
    
@Test
public void whenCalledRun_thenOneAssertion() {
    assertThat(vehicle.run())
      .isEqualTo("The vehicle is running.");
}
    
@Test
public void whenCalledStop_thenOneAssertion() {
    assertThat(vehicle.stop())
      .isEqualTo("The vehicle has stopped.");
}

@Test
public void whenCalledAccelerate_thenOneAssertion() {
    assertThat(car.accelerate(80))
      .isEqualTo("The car accelerates at : 80 MPH.");
}
    
@Test
public void whenCalledRun_thenOneAssertion() {
    assertThat(car.run())
      .isEqualTo("The vehicle is running.");
}
    
@Test
public void whenCalledStop_thenOneAssertion() {
    assertThat(car.stop())
      .isEqualTo("The vehicle has stopped.");
}
```

이제 재정의되지 않은 run() 및 stop() 메서드가 Car 와 Vehicle 모두에 대해 동일한 값을 반환하는 방법을 보여주는 몇 가지 단위 테스트를 살펴보겠습니다 .

```java
@Test
public void givenVehicleCarInstances_whenCalledRun_thenEqual() {
    assertThat(vehicle.run()).isEqualTo(car.run());
}
 
@Test
public void givenVehicleCarInstances_whenCalledStop_thenEqual() {
   assertThat(vehicle.stop()).isEqualTo(car.stop());
}
```

우리의 경우 두 클래스의 소스 코드에 액세스할 수 있으므로 기본 Vehicle 인스턴스 에서 accelerate() 메서드를 호출 하고 Car 인스턴스에서 accelerate() 을 호출 하면 동일한 인수에 대해 다른 값이 반환된다는 것을 분명히 알 수 있습니다.

따라서 다음 테스트는 Car 인스턴스에 대해 재정의된 메서드가 호출됨을 보여줍니다 .

```java
@Test
public void whenCalledAccelerateWithSameArgument_thenNotEqual() {
    assertThat(vehicle.accelerate(100))
      .isNotEqualTo(car.accelerate(100));
}
```

### 3.1. 유형 대체 가능성

OOP의 핵심 원칙은 LSP(Liskov Substitution Principle | 리스코프 치환원칙) 와 밀접하게 관련된 유형 대체 가능성 입니다.

간단히 말해서, LSP 는 응용 프로그램이 주어진 기본 유형과 함께 작동하면 모든 하위 유형과도 작동해야 한다고 명시합니다. 그렇게 하면 유형 대체 가능성이 적절하게 보존됩니다.

메서드 재정의의 가장 큰 문제는 파생 클래스의 일부 특정 메서드 구현이 LSP를 완전히 준수하지 않아 형식 대체 가능성을 유지하지 못할 수 있다는 것입니다.

물론 다른 유형의 인수를 허용하고 다른 유형도 반환하도록 재정의된 메서드를 만드는 것이 유효하지만 다음 규칙을 완전히 준수해야 합니다.

- 기본 클래스의 메소드가 주어진 유형의 인수를 사용하는 경우 재정의된 메소드는 동일한 유형 또는 상위 유형(일명 반공변 메소드 인수) 을 취해야 합니다.
- 기본 클래스의 메서드가 void 를 반환하면 재정의된 메서드는 void 를 반환해야 합니다.
- 기본 클래스의 메소드가 프리미티브를 리턴하면 재정의된 메소드는 동일한 프리미티브를 리턴해야 합니다.
- 기본 클래스의 메서드가 특정 형식을 반환하는 경우 재정의된 메서드는 동일한 형식 또는 하위 형식(일명 공변 반환 형식) 을 반환해야 합니다.
- 기본 클래스의 메서드가 예외를 throw하는 경우 재정의된 메서드는 동일한 예외 또는 기본 클래스 예외의 하위 유형을 throw해야 합니다.

### 3.2. 동적 바인딩

메서드 재정의는 기본 유형과 하위 유형의 계층 구조가 있는 상속을 통해서만 구현될 수 있다는 점을 고려하면 기본 클래스와 하위 클래스가 모두 정의하므로 컴파일러는 컴파일 시간에 호출할 메서드를 결정할 수 없습니다. 같은 방법.

결과적으로 컴파일러는 어떤 메서드를 호출해야 하는지 알기 위해 객체의 유형을 확인해야 합니다.

이 검사는 런타임에 발생하므로 메서드 재정의는 동적 바인딩의 일반적인 예입니다.
