> 원문 : https://www.baeldung.com/a-guide-to-java-enums

# enum 가이드

## 1. 개요
이 자습서에서는 Java enum이 무엇인지, Java enum이 해결하는 문제 및 일부 디자인 패턴을 실제로 사용할 수 있는 방법을 배웁니다.

Java 5는 처음으로 enum 키워드를 도입했습니다. 항상 java.lang.Enum 클래스를 확장하는 특수 유형의 클래스를 나타냅니다.

[사용법에 대한 공식 문서를 보려면 문서](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Enum.html)로 이동 하십시오.

이러한 방식으로 정의된 상수는 코드를 더 읽기 쉽게 만들고, 컴파일 시간 검사를 허용하고, 허용되는 값 목록을 미리 문서화하고, 잘못된 값 전달로 인한 예기치 않은 동작을 방지합니다.

다음은 피자 주문 상태를 정의하는 enum의 빠르고 간단한 예입니다. 주문 상태는 ORDERED , READY 또는 DELIVERED 일 수 있습니다 .

```java
public enum PizzaStatus {
    ORDERED,
    READY, 
    DELIVERED; 
}
```

또한 enum에는 기존의 공개 정적 최종 상수를 사용하는 경우 작성해야 하는 유용한 메서드가 많이 있습니다.

## 2. 사용자 지정 enum 메서드

enum이 무엇이고 어떻게 사용할 수 있는지에 대한 기본적인 이해를 얻었으므로 enum에 대한 몇 가지 추가 API 메서드를 정의하여 다음 단계로 넘어갈 것입니다.

```java
public class Pizza {
    private PizzaStatus status;
    
    public enum PizzaStatus {
        ORDERED,
        READY,
        DELIVERED;
    }

    public boolean isDeliverable() {
        if (getStatus() == PizzaStatus.READY) {
            return true;
        }
        return false;
    }
    
    // Methods that set and get the status variable.
}
```

## 3. "==" 연산자를 사용한 enum 비교

enum 유형은 JVM에 하나의 상수 인스턴스만 존재하도록 보장하므로 위의 예에서와 같이 "==" 연산자를 사용하여 두 변수를 안전하게 비교할 수 있습니다. 또한 "==" 연산자는 **컴파일 시간 및 런타임 안전성을 제공합니다**.

먼저 다음 스니펫 에서 런타임 안전성을 살펴 보겠습니다. 여기서 "==" 연산자를 사용하여 상태를 비교합니다. 

두 값 모두 null 일 수 있으며 NullPointerException이 발생 하지 않습니다. 반대로 equals 메서드를 사용하면 NullPointerException 이 발생합니다 .

```java
if(testPz.getStatus().equals(Pizza.PizzaStatus.DELIVERED)); 
if(testPz.getStatus() == Pizza.PizzaStatus.DELIVERED); 
```

컴파일 시간 안전성 에 관해서 는 equals 메소드를 사용하여 비교하여 다른 유형의 enum이 동일한지 판별하는 예제를 살펴보겠습니다.
enum과 getStatus 메소드의 값이 우연히 동일하기 때문입니다. 그러나 논리적으로 비교는 거짓이어야 합니다. "==" 연산자를 사용하여 이 문제를 방지합니다.

컴파일러는 비교를 비호환성 오류로 플래그 지정합니다.

```java
if(testPz.getStatus().equals(TestColor.GREEN));
if(testPz.getStatus() == TestColor.GREEN);
```

## 4. Switch 문에서 enum 사용하기

switch 문 에서 enum 유형을 사용할 수도 있습니다.

```java
public int getDeliveryTimeInDays() {
    switch (status) {
        case ORDERED: return 5;
        case READY: return 2;
        case DELIVERED: return 0;
    }
    
    return 0;
}
```

## 5. enum의 필드, 메서드 및 생성자

enum 내부에 생성자, 메서드 및 필드를 정의할 수 있으므로 매우 강력합니다.

다음으로 피자 주문의 한 단계에서 다른 단계로의 전환을 구현하여 위의 예를 확장해 보겠습니다. 이전에 사용된 if 및 switch 문 을 제거하는 방법을 살펴보겠습니다 .

```java
public class Pizza {
    private PizzaStatus status;
    
    public enum PizzaStatus {
        ORDERED (5){
            @Override
            public boolean isOrdered() {
                return true;
            }
        },
        READY (2){
            @Override
            public boolean isReady() {
                return true;
            }
        },
        DELIVERED (0){
            @Override
            public boolean isDelivered() {
                return true;
            }
        };

        private int timeToDelivery;

        public boolean isOrdered() {return false;}

        public boolean isReady() {return false;}

        public boolean isDelivered(){return false;}

        public int getTimeToDelivery() {
            return timeToDelivery;
        }

        PizzaStatus (int timeToDelivery) {
            this.timeToDelivery = timeToDelivery;
        }
    }

    public boolean isDeliverable() {
        return this.status.isReady();
    }

    public void printTimeToDeliver() {
        System.out.println("Time to delivery is " + 
          this.getStatus().getTimeToDelivery());
    }
    
    // Methods that set and get the status variable.
}
```

아래의 테스트 스니펫은 이것이 어떻게 작동하는지 보여줍니다:

```java
@Test
public void givenPizaOrder_whenReady_thenDeliverable() {
    Pizza testPz = new Pizza();
    testPz.setStatus(Pizza.PizzaStatus.READY);
    assertTrue(testPz.isDeliverable());
}
```

## 6. EnumSet 및 EnumMap

### 6.1. enum 집합

EnumSet 은 Enum 유형과 함께 사용하기 위한 특수한 Set 구현입니다.

HashSet과 비교할 때 사용되는 내부 비트 벡터 표현 으로 인해 특정 Enum 상수 집합 을 매우 효율적이고 간결 하게 표현 합니다. 또한 기존의 int 기반 "비트 플래그"에 대한 유형 안전 대안을 제공하므로 더 읽기 쉽고 유지 관리가 쉬운 간결한 코드를 작성할 수 있습니다.

EnumSet 은 RegularEnumSet 및 JumboEnumSet 의 두 가지 구현이 있는 추상 클래스이며, 그 중 하나는 인스턴스화 시 enum의 상수 수에 따라 선택됩니다.

따라서 대부분의 시나리오에서 enum 상수 컬렉션으로 작업하고 싶을 때마다 이 세트 를 사용 하는 것이 좋습니다. 가능한 모든 상수를 반복하고 싶을 뿐입니다.

아래 코드 조각에서 EnumSet 을 사용하여 상수의 하위 집합을 만드는 방법을 볼 수 있습니다.

```java
public class Pizza {

    private static EnumSet<PizzaStatus> undeliveredPizzaStatuses =
      EnumSet.of(PizzaStatus.ORDERED, PizzaStatus.READY);

    private PizzaStatus status;

    public enum PizzaStatus {
        ...
    }

    public boolean isDeliverable() {
        return this.status.isReady();
    }

    public void printTimeToDeliver() {
        System.out.println("Time to delivery is " + 
          this.getStatus().getTimeToDelivery() + " days");
    }

    public static List<Pizza> getAllUndeliveredPizzas(List<Pizza> input) {
        return input.stream().filter(
          (s) -> undeliveredPizzaStatuses.contains(s.getStatus()))
            .collect(Collectors.toList());
    }

    public void deliver() { 
        if (isDeliverable()) { 
            PizzaDeliverySystemConfiguration.getInstance().getDeliveryStrategy().deliver(this); 
            this.setStatus(PizzaStatus.DELIVERED); 
        } 
    }
    
    // Methods that set and get the status variable.
}
```

다음 테스트를 실행 하면 Set 인터페이스 의 EnumSet 구현의 힘을 보여줍니다.

```java
@Test
public void givenPizaOrders_whenRetrievingUnDeliveredPzs_thenCorrectlyRetrieved() {
    List<Pizza> pzList = new ArrayList<>();
    Pizza pz1 = new Pizza();
    pz1.setStatus(Pizza.PizzaStatus.DELIVERED);

    Pizza pz2 = new Pizza();
    pz2.setStatus(Pizza.PizzaStatus.ORDERED);

    Pizza pz3 = new Pizza();
    pz3.setStatus(Pizza.PizzaStatus.ORDERED);

    Pizza pz4 = new Pizza();
    pz4.setStatus(Pizza.PizzaStatus.READY);

    pzList.add(pz1);
    pzList.add(pz2);
    pzList.add(pz3);
    pzList.add(pz4);

    List<Pizza> undeliveredPzs = Pizza.getAllUndeliveredPizzas(pzList); 
    assertTrue(undeliveredPzs.size() == 3); 
}
```

### 6.2. EnumMap

EnumMap 은 enum 상수를 키로 사용하기 위한 특수한 Map 구현입니다. 대응하는 HashMap 과 비교하여 내부적으로 배열로 표현되는 효율적이고 간결한 구현입니다.

```java
EnumMap<Pizza.PizzaStatus, Pizza> map;
```

실제로 사용하는 방법의 예를 살펴보겠습니다.

```java
public static EnumMap<PizzaStatus, List<Pizza>> 
  groupPizzaByStatus(List<Pizza> pizzaList) {
    EnumMap<PizzaStatus, List<Pizza>> pzByStatus = 
      new EnumMap<PizzaStatus, List<Pizza>>(PizzaStatus.class);
    
    for (Pizza pz : pizzaList) {
        PizzaStatus status = pz.getStatus();
        if (pzByStatus.containsKey(status)) {
            pzByStatus.get(status).add(pz);
        } else {
            List<Pizza> newPzList = new ArrayList<Pizza>();
            newPzList.add(pz);
            pzByStatus.put(status, newPzList);
        }
    }
    return pzByStatus;
}
```

다음 테스트를 실행 하면 Map 인터페이스 의 EnumMap 구현의 힘을 보여줍니다.

```java
@Test
public void givenPizaOrders_whenGroupByStatusCalled_thenCorrectlyGrouped() {
    List<Pizza> pzList = new ArrayList<>();
    Pizza pz1 = new Pizza();
    pz1.setStatus(Pizza.PizzaStatus.DELIVERED);

    Pizza pz2 = new Pizza();
    pz2.setStatus(Pizza.PizzaStatus.ORDERED);

    Pizza pz3 = new Pizza();
    pz3.setStatus(Pizza.PizzaStatus.ORDERED);

    Pizza pz4 = new Pizza();
    pz4.setStatus(Pizza.PizzaStatus.READY);

    pzList.add(pz1);
    pzList.add(pz2);
    pzList.add(pz3);
    pzList.add(pz4);

    EnumMap<Pizza.PizzaStatus,List<Pizza>> map = Pizza.groupPizzaByStatus(pzList);
    assertTrue(map.get(Pizza.PizzaStatus.DELIVERED).size() == 1);
    assertTrue(map.get(Pizza.PizzaStatus.ORDERED).size() == 2);
    assertTrue(map.get(Pizza.PizzaStatus.READY).size() == 1);
}
```

## 7. Enum을 사용하여 디자인 패턴 구현

### 7.1. 싱글톤 패턴

일반적으로 Singleton 패턴을 사용하여 클래스를 구현하는 것은 매우 간단합니다. enum은 싱글톤을 구현하는 빠르고 쉬운 방법을 제공합니다.

또한 enum 클래스 는 내부적으로 Serializable 인터페이스를 구현하므로 JVM에서 클래스가 싱글톤임을 보장합니다. 이는 역직렬화 중에 새 인스턴스가 생성되지 않도록 해야 하는 기존 구현과 다릅니다.

아래 코드 스니펫에서 싱글톤 패턴을 구현하는 방법을 볼 수 있습니다.

```java
public enum PizzaDeliverySystemConfiguration {
    INSTANCE;
    PizzaDeliverySystemConfiguration() {
        // Initialization configuration which involves
        // overriding defaults like delivery strategy
    }

    private PizzaDeliveryStrategy deliveryStrategy = PizzaDeliveryStrategy.NORMAL;

    public static PizzaDeliverySystemConfiguration getInstance() {
        return INSTANCE;
    }

    public PizzaDeliveryStrategy getDeliveryStrategy() {
        return deliveryStrategy;
    }
}
```

### 7.2. Strategy 패턴

일반적으로 Strategy 패턴은 서로 다른 클래스에서 구현되는 인터페이스를 사용하여 작성됩니다.

새 전략을 추가한다는 것은 새 구현 클래스를 추가하는 것을 의미합니다. enum을 사용하면 더 적은 노력으로 이를 달성할 수 있으며 새 구현을 추가한다는 것은 단순히 일부 구현으로 다른 인스턴스를 정의하는 것을 의미합니다.

아래 코드 스니펫은 전략 패턴을 구현하는 방법을 보여줍니다.

``` java
public enum PizzaDeliveryStrategy {
    EXPRESS {
        @Override
        public void deliver(Pizza pz) {
            System.out.println("Pizza will be delivered in express mode");
        }
    },
    NORMAL {
        @Override
        public void deliver(Pizza pz) {
            System.out.println("Pizza will be delivered in normal mode");
        }
    };

    public abstract void deliver(Pizza pz);
}
```

그런 다음 Pizza 클래스 에 다음 메서드를 추가합니다.

```java
public void deliver() {
    if (isDeliverable()) {
        PizzaDeliverySystemConfiguration.getInstance().getDeliveryStrategy()
          .deliver(this);
        this.setStatus(PizzaStatus.DELIVERED);
    }
}

@Test
public void givenPizaOrder_whenDelivered_thenPizzaGetsDeliveredAndStatusChanges() {
    Pizza pz = new Pizza();
    pz.setStatus(Pizza.PizzaStatus.READY);
    pz.deliver();
    assertTrue(pz.getStatus() == Pizza.PizzaStatus.DELIVERED);
}
```

## 8. 자바 8과 enum

Java 8에서 Pizza 클래스를 다시 작성할 수 있으며 람다 및 Stream API 를 사용하여 getAllUndeliveredPizzas() 및 groupPizzaByStatus( ) 메서드가 얼마나 간결해졌는지 확인할 수 있습니다.

```java
public static List<Pizza> getAllUndeliveredPizzas(List<Pizza> input) {
    return input.stream().filter(
      (s) -> !deliveredPizzaStatuses.contains(s.getStatus()))
        .collect(Collectors.toList());
}

public static EnumMap<PizzaStatus, List<Pizza>> 
  groupPizzaByStatus(List<Pizza> pzList) {
    EnumMap<PizzaStatus, List<Pizza>> map = pzList.stream().collect(
      Collectors.groupingBy(Pizza::getStatus,
      () -> new EnumMap<>(PizzaStatus.class), Collectors.toList()));
    return map;
}
```

## 9. Enum의 JSON 표현

Jackson 라이브러리를 사용하면 enum 유형이 POJO인 것처럼 JSON 표현을 가질 수 있습니다. 아래 코드 스니펫에서 동일한 항목에 대해 Jackson 주석을 사용하는 방법을 확인할 수 있습니다.

```java
@JsonFormat(shape = JsonFormat.Shape.OBJECT)
public enum PizzaStatus {
    ORDERED (5){
        @Override
        public boolean isOrdered() {
            return true;
        }
    },
    READY (2){
        @Override
        public boolean isReady() {
            return true;
        }
    },
    DELIVERED (0){
        @Override
        public boolean isDelivered() {
            return true;
        }
    };

    private int timeToDelivery;

    public boolean isOrdered() {return false;}

    public boolean isReady() {return false;}

    public boolean isDelivered(){return false;}

    @JsonProperty("timeToDelivery")
    public int getTimeToDelivery() {
        return timeToDelivery;
    }

    private PizzaStatus (int timeToDelivery) {
        this.timeToDelivery = timeToDelivery;
    }
}
```

다음과 같이 Pizza 및 PizzaStatus 를 사용할 수 있습니다 .

```java
Pizza pz = new Pizza();
pz.setStatus(Pizza.PizzaStatus.READY);
System.out.println(Pizza.getJsonString(pz));
```

그러면 Pizza 상태에 대한 다음 JSON 표현이 생성됩니다.

```json
{
  "status" : {
    "timeToDelivery" : 2,
    "ready" : true,
    "ordered" : false,
    "delivered" : false
  },
  "deliverable" : true
}
```

enum 유형의 JSON 직렬화/역직렬화(사용자 정의 포함)에 대한 자세한 내용은 Jackson – 직렬화 enum을 JSON 개체로 참조할 수 있습니다 .
