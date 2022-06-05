> 원문 : https://www.baeldung.com/java-this

# this 키워드

## 1. 소개
이 자습서에서는 this Java 키워드 를 살펴보겠습니다.

Java에서 this 키워드는 메서드가 호출되는 현재 객체에 대한 참조 입니다.

this 키워드를 언제 어떻게 사용할 수 있는지 알아보겠습니다.

## 2. 필드 섀도잉 명확화

this 키워드는 지역 매개변수에서 인스턴스 변수를 명확하게 하는 데 유용합니다. 

가장 일반적인 이유는 인스턴스 필드와 이름이 같은 생성자 매개변수가 있는 경우입니다.

```java
public class KeywordTest {
    private String name;
    private int age;
    
    public KeywordTest(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

여기에서 볼 수 있듯이  매개변수와 구별하기 위해 name 및 age 인스턴스 필드와 함께 this를 사용합니다.

또 다른 사용법은 로컬 범위에서 매개변수 숨김 또는 섀도잉과 함께 사용 하는 것 입니다. 사용 예는 변수 및 메서드 숨김 문서에서 찾을 수 있습니다.

## 3. 같은 클래스의 생성자 참조하기

생성자에서 this() 를 사용 하여 동일한 클래스의 다른 생성자를 호출 할 수 있습니다. 여기서 코드 사용량을 줄이기 위해 생성자 연결에 this()를 사용합니다.

가장 일반적인 사용 사례는 매개변수화된 생성자에서 기본 생성자를 호출하는 것입니다.

```java
public KeywordTest(String name, int age) {
    this();
    
    // the rest of the code
}
```

또는 인수가 없는 생성자에서 매개변수화된 생성자를 호출하고 일부 인수를 전달할 수 있습니다

```java
public KeywordTest() {
    this("John", 27);
}
```

this() 는 생성자의 첫 번째 명령문이어야 합니다. 그렇지 않으면 컴파일 오류가 발생합니다.

## 4. this를 매개변수로 전달하기

여기 에 this Keyword 인수가 정의된 printInstance() 메서드 가 있습니다.

```java
public KeywordTest() {
    printInstance(this);
}

public void printInstance(KeywordTest thisKeyword) {
    System.out.println(thisKeyword);
}
```

생성자 내에서 printInstance() 메서드를 호출합니다. 이를 통해 현재 인스턴스에 대한 참조를 전달합니다.

## 5. this 반환

this  키워드를 사용 하여 메서드에서 현재 클래스 인스턴스를 반환 할 수도 있습니다.

[코드를 복제하지 않기 위해 다음은 빌더 디자인 패턴](https://www.baeldung.com/creational-design-patterns) 에서 구현되는 방법에 대한 완전한 실제 예입니다.



## 6. 내부 클래스 내의 this 키워드

우리는 또한 this를 내부 클래스 내에서 외부 클래스 인스턴스에 액세스하는 데 사용합니다.

```java
public class KeywordTest {
    private String name;

    class ThisInnerClass {

        boolean isInnerClass = true;

        public ThisInnerClass() {
            KeywordTest thisKeyword = KeywordTest.this;
            String outerString = KeywordTest.this.name;
        }
    }
}
```

여기에서 생성자 내부에서 KeywordTest.this 호출 을 사용하여 KeywordTest 인스턴스에 대한 참조를 얻을 수 있습니다. 

더 깊이 들어가 KeywordTest.this.name 필드와 같은 인스턴스 변수에 액세스할 수 있습니다.
