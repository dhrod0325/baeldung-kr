> 원문 : https://www.baeldung.com/java-inheritance-composition

# Java의 상속 및 구성(Is-a 대 Has-관계)

## 1. 개요
상속 및 구성은 추상화, 캡슐화 및 다형성과 함께 객체 지향 프로그래밍 (OOP)의 초석입니다.

이 자습서에서는 상속 및 구성의 기본 사항을 다루며 두 가지 유형의 관계 간의 차이점을 찾는 데 중점을 둘 것입니다.

## 2. 상속의 기본
상속은 강력하지만 남용되고 오용되는 메커니즘입니다.

간단히 말해서, 상속을 통해 기본 클래스(기본 유형이라고도 함)는 주어진 유형에 공통적인 상태와 동작을 정의하고 하위 클래스(하위 유형이라고도 함)가 해당 상태 및 동작의 특수 버전을 제공할 수 있도록 합니다.

상속으로 작업하는 방법에 대한 명확한 아이디어를 얻기 위해 순진한 예제를 만들어 보겠습니다. 

기본 클래스 Person 은 사람 에 대한 공통 필드와 메서드를 정의하고 하위 클래스 Waitress 및 Actor 는 추가로 세분화된 메서드 구현을 제공합니다.

```java
public class Person {
    private final String name;

    // other fields, standard constructors, getters
}

public class Waitress extends Person {
    public String serveStarter(String starter) {
        return "Serving a " + starter;
    }
    
    // additional methods/constructors
}

public class Actress extends Person {
    
    public String readScript(String movie) {
        return "Reading the script of " + movie;
    } 
    
    // additional methods/constructors
}
```

Waitress 및 Actor 클래스의 인스턴스도 Person 의 인스턴스인지 확인하는 단위 테스트를 만들어 유형 수준에서 "is-a" 조건이 충족됨을 보여줍시다.

```java
@Test
public void givenWaitressInstance_whenCheckedType_thenIsInstanceOfPerson() {
    assertThat(new Waitress("Mary", "mary@domain.com", 22))
      .isInstanceOf(Person.class);
}
    
@Test
public void givenActressInstance_whenCheckedType_thenIsInstanceOfPerson() {
    assertThat(new Actress("Susan", "susan@domain.com", 30))
      .isInstanceOf(Person.class);
}
```

여기서 상속의 의미론적 측면을 강조하는 것이 중요합니다. Person 클래스의 구현을 재사용하는 것 외에도 기본 유형 Person 과 하위 유형 Waitress 및 Actor 사이 에 잘 정의된 "is-a" 관계를 만들었습니다. 웨이트리스와 여배우는 사실상 사람입니다.

이것은 우리에게 다음과 같은 질문을 하게 할 수 있습니다. 

> 어떤 사용 사례에서 상속이 올바른 접근 방식입니까?

하위 유형이 "is-a" 조건을 충족하고 주로 클래스 계층 아래에서 추가 기능을 제공 한다면 상속이 갈 길입니다.

물론 재정의된 메서드가 Liskov 대체 원칙 에 의해 촉진된 기본 유형/하위 유형 대체성을 유지하는 한 메서드 재정의가 허용됩니다.

또한 하위 유형은 기본 유형의 API를 상속 한다는 점을 염두에 두어야 합니다. 일부 경우에는 과도하거나 바람직하지 않을 수 있습니다.

그렇지 않으면 대신 구성을 사용해야 합니다.

## 3. 디자인 패턴의 상속

가능한 한 상속보다 합성을 선호해야 한다는 데 합의가 이루어졌지만 상속이 그 자리를 차지하는 몇 가지 일반적인 사용 사례가 있습니다.

### 3.1. 레이어 상위 유형 패턴
이 경우 계층별로 공통 코드를 기본 클래스(상위 유형)로 이동하기 위해 상속을 사용합니다.

다음은 도메인 계층에서 이 패턴의 기본 구현입니다.

```java
public class Entity {
    protected long id;
    
    // setters
}

public class User extends Entity {
    // additional fields and methods   
}
```

서비스 및 지속성 계층과 같은 시스템의 다른 계층에도 동일한 접근 방식을 적용할 수 있습니다.

### 3.2. 템플릿 메소드 패턴

템플릿 메서드 패턴에서 기본 클래스를 사용하여 알고리즘의 불변 부분을 정의한 다음 하위 클래스에서 변형 부분을 구현할 수 있습니다 .

```java
public abstract class ComputerBuilder {
    public final Computer buildComputer() {
        addProcessor();
        addMemory();
    }
    
    public abstract void addProcessor();
    
    public abstract void addMemory();
}

public class StandardComputerBuilder extends ComputerBuilder {
    @Override
    public void addProcessor() {
        // method implementation
    }
    
    @Override
    public void addMemory() {
        // method implementation
    }
}
```

## 4. Composition's 기초

구성은 구현을 재사용하기 위해 OOP에서 제공하는 또 다른 메커니즘입니다.

간단히 말해서 구성을 사용하면 다른 객체로 구성된 객체를 모델링할 수 있으므로 이들 사이에 "has-a" 관계를 정의할 수 있습니다.

더욱이, 합성은 가장 강력한 연관 형태이며 , 이는 하나의 객체를 구성하거나 포함하는 객체(들)도 해당 객체가 파괴될 때 함께 파괴된다는 것을 의미합니다.

합성이 어떻게 작동하는지 더 잘 이해하기 위해 컴퓨터를 나타내는 개체로 작업해야 한다고 가정해 보겠습니다.

컴퓨터는 마이크로프로세서, 메모리, 사운드 카드 등을 포함한 여러 부분으로 구성되어 있으므로 컴퓨터와 컴퓨터의 각 부분을 개별 클래스로 모델링할 수 있습니다.

Computer 클래스 의 간단한 구현은 다음과 같습니다.

```java
public class Computer {
    private Processor processor;
    private Memory memory;
    private SoundCard soundCard;

    // standard getters/setters/constructors
    
    public Optional<SoundCard> getSoundCard() {
        return Optional.ofNullable(soundCard);
    }
}
```

다음 클래스는 마이크로프로세서, 메모리 및 사운드 카드를 모델링합니다(간결함을 위해 인터페이스는 생략됨).

```java
public class StandardProcessor implements Processor {
    private String model;
    
    // standard getters/setters
}
public class StandardMemory implements Memory {
    private String brand;
    private String size;
    
    // standard constructors, getters, toString
}
public class StandardSoundCard implements SoundCard {
    private String brand;

    // s
}
```

상속보다 구성을 밀어붙이는 동기를 이해하는 것은 쉽습니다. 주어진 클래스와 다른 클래스 사이에 의미상 올바른 "has-a" 관계를 설정할 수 있는 모든 시나리오에서 구성은 올바른 선택입니다.

위의 예에서 Computer 는 해당 부품을 모델링하는 클래스로 "has-a" 조건을 충족합니다.

또한 이 경우 포함하는 Computer 개체는 개체를 다른 Computer 개체 내에서 재사용할 수 없는 경우에만 포함된 개체의 소유권을 가 집니다. 가능하다면 소유권이 암시되지 않는 구성 대신 집계를 사용합니다.

## 5. 추상화 없는 구성

또는 생성자에서 선언하는 대신 Computer 클래스 의 종속성을 하드 코딩하여 구성 관계를 정의할 수 있습니다.

```java
public class Computer {
    private StandardProcessor processor = new StandardProcessor("Intel I3");
    private StandardMemory memory = new StandardMemory("Kingston", "1TB");
    
    // additional fields / methods
}
```

물론, 이것은 우리가 Computer 를 Processor 및 Memory 의 특정 구현에 강하게 의존하게 만들기 때문에 엄격하고 밀접하게 결합된 디자인이 될 것 입니다.

우리는 인터페이스와 의존성 주입 이 제공하는 추상화 수준을 이용하지 않을 것입니다.

인터페이스를 기반으로 하는 초기 설계에서는 느슨하게 결합된 설계를 얻을 수 있으며, 이는 테스트하기도 쉽습니다.
