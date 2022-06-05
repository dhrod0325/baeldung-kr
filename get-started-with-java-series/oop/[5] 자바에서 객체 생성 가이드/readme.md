> 원문 : https://www.baeldung.com/java-initialization

# 자바에서 객체 생성 가이드

## 1. 개요
간단히 말해서 JVM에서 객체로 작업하려면 먼저 초기화해야 합니다.

다음 섹션에서는 기본 유형 및 개체를 초기화할 수 있는 다양한 방법을 살펴보겠습니다.

## 2. 선언 vs 초기화
같은 페이지에 있는지 확인하는 것으로 시작하겠습니다.

선언은 유형 및 이름과 함께 변수를 정의하는 프로세스입니다 .

여기에서 id 변수를 선언합니다.

```java
int id;
```

반면 초기화는 값을 할당하는 것입니다. 예를 들어:

```java
id = 1;
```

시연을 위해 name 및 id 속성 을 사용하여 User 클래스를 생성합니다.

```java
public class User {
    private String name;
    private int id;
    
    // standard constructor, getters, setters,
}
```
다음으로 초기화하는 필드 유형에 따라 초기화가 다르게 작동하는 것을 볼 수 있습니다.

## 3. 객체 대 프리미티브
Java는 기본 유형과 참조 유형의 두 가지 유형의 데이터 표현을 제공합니다. 이 섹션에서는 초기화와 관련하여 둘 사이의 차이점에 대해 설명합니다.

Java에는 Java 기본 유형이라고 하는 8개의 내장 데이터 유형이 있습니다. 이 유형의 변수는 값을 직접 보유합니다.

참조 유형은 객체(클래스 인스턴스)에 대한 참조를 보유합니다. 변수가 할당된 메모리에 값을 보유하는 기본 유형과 달리 참조는 참조하는 객체의 값을 보유하지 않습니다.

**대신 참조는 개체가 위치한 메모리 주소를 저장하여 개체를 가리킵니다.**

Java는 실제 메모리 주소가 무엇인지 발견하는 것을 허용하지 않습니다. 오히려 참조를 사용하여 개체를 참조할 수 있습니다.

User 클래스 에서 참조 유형을 선언하고 초기화하는 예제를 살펴보겠습니다 .

```java
@Test
public void whenIntializedWithNew_thenInstanceIsNotNull() {
    User user = new User();
 
    assertThat(user).isNotNull();
}
```

여기에서 볼 수 있듯이 새 사용자 개체 생성을 담당하는 new 키워드를 사용하여 새 항목에 참조를 할당할 수 있습니다

계속해서 객체 생성에 대해 자세히 알아보겠습니다.

## 5. 객체 생성
프리미티브와 달리 객체 생성은 조금 더 복잡합니다. 이는 단순히 필드에 값을 추가하는 것이 아니기 때문입니다. 대신 new 키워드를 사용하여 초기화를 트리거합니다. 이것은 그 대가로 생성자를 호출하고 메모리에서 개체를 초기화합니다.

생성자와 new 키워드에 대해 더 자세히 살펴보겠습니다.

new 키워드는 생성자를 통해 새 객체에 대한 메모리 할당을 담당합니다.

생성자는 일반적으로 생성된 객체의 주요 속성을 나타내는 인스턴스 변수를 초기화하는데 사용됩니다 .

생성자를 명시적으로 제공하지 않으면 컴파일러는 인수가 없고 객체에 대한 메모리만 할당하는 기본 생성자를 생성합니다.

클래스는 매개변수 목록이 다른 한( 오버로드 ) 많은 생성자를 가질 수 있습니다 . 동일한 클래스에서 다른 생성자를 호출하지 않는 모든 생성자는 명시적으로 작성되었거나 컴파일러가 super() 를 통해 삽입했는지 여부에 관계없이 상위 생성자를 호출합니다.

User 클래스 에 생성자를 추가해 보겠습니다.

```java
public User(String name, int id) {
    this.name = name;
    this.id = id;
}
```
이제 생성자를 사용 하여 속성에 대한 초기 값이 있는 User 개체를 만들 수 있습니다.
```java
User user = new User("Alice", 1);
```

## 6. 가변 범위
다음 섹션에서는 Java의 변수가 존재할 수 있는 다양한 유형의 범위와 이것이 초기화 프로세스에 미치는 영향을 살펴보겠습니다.

### 6.1. 인스턴스 및 클래스 변수
인스턴스 및 클래스 변수는 초기화할 필요가 없습니다. 이러한 변수를 선언하는 즉시 다음과 같은 기본값이 지정됩니다.

![img.png](img.png)

이제 일부 인스턴스 및 클래스 관련 변수를 정의하고 기본값이 있는지 여부를 테스트해 보겠습니다.
```java
@Test
public void whenValuesAreNotInitialized_thenUserNameAndIdReturnDefault() {
    User user = new User();
 
    assertThat(user.getName()).isNull();
    assertThat(user.getId() == 0);
}
```
### 6.2. 지역 변수
지역 변수는 기본값이 없고 컴파일러에서 초기화되지 않은 값을 사용하도록 허용하지 않으므로 사용하기 전에 초기화해야 합니다.

예를 들어 다음 코드는 컴파일러 오류를 생성합니다.

```java
public void print(){
    int i;
    System.out.println(i);
}
```

## 7. 최종 키워드
필드에 적용된 마지막 키워드 는 초기화 후 필드 값을 더 이상 변경할 수 없음을 의미합니다. 이런 식으로 Java에서 상수를 정의할 수 있습니다.

User 클래스 에 상수를 추가해 보겠습니다.

```java
private static final int YEAR = 2000;
```

상수는 선언될 때나 생성자에서 초기화되어야 합니다.

## 8. 자바의 이니셜라이저
Java에서 이니셜라이저는 연관된 이름이나 데이터 유형 이 없고 메소드, 생성자 또는 다른 코드 블록 외부에 배치되는 코드 블록입니다.

Java는 정적 및 인스턴스 이니셜라이저의 두 가지 유형의 이니셜라이저를 제공합니다. 각각 어떻게 사용할 수 있는지 봅시다.

### 8.1. 인스턴스 이니셜라이저
이것을 사용하여 인스턴스 변수를 초기화할 수 있습니다.

시연하기 위해 User 클래스 의 인스턴스 이니셜라이저를 사용하여 사용자 ID 에 값을 제공하겠습니다.

```java
{
    id = 0;
}
```
### 8.2. 정적 초기화 블록
정적 이니셜라이저 또는 정적 블록 - 정적 필드 를 초기화하는 데 사용되는 코드 블록입니다 . 즉, static 키워드로 표시된 간단한 이니셜라이저입니다 .

```java
private static String forum;
static {
    forum = "Java";
}
```

## 9. 초기화 순서

물론 다른 유형의 필드를 초기화하는 코드를 작성할 때 초기화 순서를 주시해야 합니다.

Java에서 초기화 문의 순서는 다음과 같습니다.

- 정적 변수 및 정적 이니셜라이저 순서대로
- 인스턴스 변수 및 인스턴스 이니셜라이저 순서대로
- 생성자

```java
class Man {
    public final static String NAME = "test"; // 1번째
    
    private String name = 1; // 2번째
    
    public Man() {
        this.name = 2; //3번쨰
    }
}
```

## 10. 객체 수명 주기
이제 객체를 선언하고 초기화하는 방법을 배웠으므로 객체가 사용되지 않을 때 어떻게 되는지 알아보겠습니다.

객체 파괴에 대해 걱정해야 하는 다른 언어와 달리 **Java는 가비지 수집기를 통해 사용되지 않는 객체를 처리합니다.**

Java의 모든 객체는 **프로그램의 힙 메모리에 저장됩니다**. 실제로 힙은 Java 애플리케이션에 할당된 사용되지 않은 메모리의 큰 풀을 나타냅니다.

반면에 **가비지 수집기는 더 이상 도달할 수 없는 개체를 삭제 하여 자동 메모리 관리를 처리하는 Java 프로그램입니다**.

Java 객체가 도달할 수 없게 되려면 다음 상황 중 하나가 발생해야 합니다.

- 개체에는 더 이상 개체를 가리키는 참조가 없습니다.
- 개체를 가리키는 모든 참조가 범위를 벗어났습니다.

결론적으로, 객체는 일반적으로 new 키워드를 사용하여 클래스에서 먼저 생성됩니다. 그러면 객체는 수명을 다하고 메서드와 필드에 대한 액세스를 제공합니다.

마지막으로 더 이상 필요하지 않으면 가비지 수집기가 이를 파괴합니다.

## 11. 객체 생성을 위한 기타 방법
이 섹션에서는 new 키워드 이외의 방법 으로 객체를 생성하고 이를 적용하는 방법, 특히 리플렉션, 복제 및 직렬화에 대해 간략하게 살펴보겠습니다.

리플렉션은 런타임에 클래스, 필드 및 메서드를 검사하는 데 사용할 수 있는 메커니즘입니다. 다음은 리플렉션을 사용하여 사용자 개체를 만드는 예입니다 .

```java
@Test
public void whenInitializedWithReflection_thenInstanceIsNotNull() 
  throws Exception {
    User user = User.class.getConstructor(String.class, int.class)
      .newInstance("Alice", 2);
 
    assertThat(user).isNotNull();
}
```

이 경우 리플렉션을 사용하여 User 클래스의 생성자를 찾고 호출합니다.

다음 방법인 복제는 개체의 정확한 복사본을 만드는 방법입니다. 이를 위해 User 클래스는 Cloneable 인터페이스를 구현해야 합니다.

```java
public class User implements Cloneable { //... }
```

이제 clone() 메서드를 사용하여 사용자 객체 와 동일한 속성 값을 갖는 새로운 clonedUser 객체를 생성할 수 있습니다.
```java
@Test
public void whenCopiedWithClone_thenExactMatchIsCreated() 
  throws CloneNotSupportedException {
    User user = new User("Alice", 3);
    User clonedUser = (User) user.clone();
 
    assertThat(clonedUser).isEqualTo(user);
}
```

우리는 또한 생성자를 호출하지 않고 객체에 대한 메모리를 할당하기 위해 sun.misc.Unsafe 클래스를 사용할 수 있습니다:

```java
User u = (User) unsafeInstance.allocateInstance(User.class);
```
