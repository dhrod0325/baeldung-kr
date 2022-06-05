> 원문 : https://www.baeldung.com/java-abstract-class

# 자바의 추상 클래스

## 1. 개요
메소드를 구현할 때 구현의 일부를 나중에 완료하기 위해 연기하고 싶은 경우가 많습니다. 추상 클래스를 통해 Java에서 이를 쉽게 수행할 수 있습니다.

이 튜토리얼에서 우리는 자바 추상 클래스의 기초와 어떤 경우에 도움이 될 수 있는지 배울 것 입니다

## 2. 추상 클래스의 핵심 개념
추상 클래스를 언제 사용해야 하는지 알아보기 전에 가장 관련성이 높은 특성을 살펴보겠습니다.

- class 키워드 앞에 abstract 수정자를 사용하여 추상 클래스를 정의합니다.
- 추상 클래스는 하위 클래스화할 수 있지만 인스턴스화할 수는 없습니다.
- 클래스가 하나 이상의 추상 메서드를 정의하는 경우 클래스 자체를 추상 으로 선언해야 합니다.
- 추상 클래스는 **추상 메서드와 구체적인 메서드를 모두 선언할 수 있습니다.**
- **추상 클래스에서 파생된 하위 클래스는 모든 기본 클래스의 추상 메서드를 구현하거나 추상 자체여야 합니다.**

이러한 개념을 더 잘 이해하기 위해 간단한 예를 만들겠습니다.

기본 추상 클래스가 보드 게임의 추상 API를 정의하도록 합시다.

```java
public abstract class BoardGame {
    //... field declarations, constructors

    public abstract void play();

    //... concrete methods
}
```

그런 다음 play 메서드를 구현하는 하위 클래스를 만들 수 있습니다 .

```java
public class Checkers extends BoardGame {
    @Override
    public void play() {
        //... implementation
    }
}
```

## 3. 추상 클래스를 사용하는 경우

이제 인터페이스 및 구체적인 클래스 보다 추상 클래스를 선호해야 하는 몇 가지 일반적인 시나리오를 분석해 보겠습니다 .

- 여러 관련 하위 클래스가 공유하는 몇 가지 공통 기능을 한 곳(코드 재사용)에 캡슐화하려고 합니다.
- 하위 클래스가 쉽게 확장하고 개선할 수 있는 API를 부분적으로 정의해야 합니다.
- 하위 클래스는 보호된 액세스 수정자가 있는 하나 이상의 공통 메서드 또는 필드를 상속해야 합니다.

> 코드 재사용을 위해 상속을 사용하는것은 올바르게 상속을 사용하는 경우가 아닙니다. 상속은 객체들의 그룹화를 위하여 사용하세요

이러한 모든 시나리오는 Open/Closed 원칙 에 대한 완전한 상속 기반 준수의 좋은 예입니다 .

게다가 추상 클래스를 사용하면 기본 유형과 하위 유형을 암시적으로 다루기 때문에 다형성 도 활용하고 있습니다 .

코드 재사용은 클래스 계층 구조 내에서 "is-a" 관계가 유지되는 한 추상 클래스를 사용해야 하는 매우 강력한 이유입니다.

그리고 Java 8은 기본 메소드를 사용하여 또 다른 주름을 추가합니다. 이 메소드는 때때로 추상 클래스를 완전히 생성해야 하는 자리를 대신할 수 있습니다.

## 4. 파일 판독기의 샘플 계층
추상 클래스가 테이블에 가져오는 기능을 더 명확하게 이해하기 위해 다른 예를 살펴보겠습니다.

### 4.1. 기본 추상 클래스 정의
따라서 여러 유형의 파일 판독기를 갖고 싶다면 파일 읽기에 공통적인 것을 캡슐화하는 추상 클래스를 만들 수 있습니다.

```java
public abstract class BaseFileReader {
    
    protected Path filePath;
    
    protected BaseFileReader(Path filePath) {
        this.filePath = filePath;
    }
    
    public Path getFilePath() {
        return filePath;
    }
    
    public List<String> readFile() throws IOException {
        return Files.lines(filePath)
          .map(this::mapFileLine).collect(Collectors.toList());
    }
    
    protected abstract String mapFileLine(String line);
}
```

필요한 경우 하위 클래스가 액세스할 수 있도록 filePath를 보호하도록 만들었습니다. 더 중요한 것은 파일 내용에서 실제로 한 줄의 텍스트를 구문 분석하는 방법을 미해결 상태로 두었습니다.

우리의 계획은 간단합니다. 우리의 구체적인 클래스는 각각 파일 경로를 저장하거나 파일을 탐색하는 특별한 방법이 없지만 각 줄을 변환하는 특별한 방법이 있습니다.

처음에는 BaseFileReader 가 불필요해 보일 수 있습니다. 그러나 이것은 깨끗하고 쉽게 확장할 수 있는 디자인의 기초입니다. 이를 통해 고유한 비즈니스 논리에 집중할 수 있는 다양한 버전의 파일 판독기를 쉽게 구현할 수 있습니다.

### 4.2. 서브클래스 정의
자연스러운 구현은 아마도 파일의 내용을 소문자로 변환하는 것입니다:
```java
public class LowercaseFileReader extends BaseFileReader {

    public LowercaseFileReader(Path filePath) {
        super(filePath);
    }

    @Override
    public String mapFileLine(String line) {
        return line.toLowerCase();
    }   
}
```

또는 다른 것은 파일의 내용을 대문자로 변환하는 것일 수 있습니다:

```java
public class UppercaseFileReader extends BaseFileReader {

    public UppercaseFileReader(Path filePath) {
        super(filePath);
    }

    @Override
    public String mapFileLine(String line) {
        return line.toUpperCase();
    }
}
```

이 간단한 예제에서 볼 수 있듯이 각 하위 클래스는 파일 읽기의 다른 측면을 지정할 필요 없이 고유한 동작에 집중할 수 있습니다 .

### 4.3. 서브클래스 사용

마지막으로 추상 클래스에서 상속받은 클래스를 사용하는 것은 다른 구체적인 클래스와 다르지 않습니다.

```java
@Test
public void givenLowercaseFileReaderInstance_whenCalledreadFile_thenCorrect() throws Exception {
    URL location = getClass().getClassLoader().getResource("files/test.txt")
    Path path = Paths.get(location.toURI());
    BaseFileReader lowercaseFileReader = new LowercaseFileReader(path);
        
    assertThat(lowercaseFileReader.readFile()).isInstanceOf(List.class);
}
```
