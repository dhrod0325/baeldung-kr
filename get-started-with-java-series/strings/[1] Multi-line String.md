# Java Multi-line String

## 1. 개요
이 기사에서는 Java에서 여러 줄 문자열을 선언하는 방법을 배웁니다.
이제 Java 15가 출시되었으므로 텍스트 블록이라는 새로운 기본 기능을 사용할 수 있습니다.
이 기능을 사용할 수 없는 경우 다른 방법도 검토하겠습니다.

## 2. 텍스트 블록
문자열을 """ (큰따옴표 3개) 로 선언하여  텍스트 블록 을 사용할 수 있습니다 .

```java
public String textBlocks() {
    return """
        Get busy living
        or
        get busy dying.
        --Stephen King""";
}
```

지금까지 여러 줄 문자열을 선언하는 가장 편리한 방법입니다. 사실, 우리는 줄 구분 기호나 들여쓰기 공백을 다룰 필요가 없습니다.

이 기능은 Java 15에서 사용할 수 있지만 미리보기 기능을 활성화하면 Java 13 및 14에서도 사용할 수 있습니다 .

다음 장에서는 이전 버전의 Java를 사용하거나 텍스트 블록 을 적용할 수 없는 경우에 적합한 다른 방법을 검토합니다.

## 3. 줄 구분 기호 얻기
각 운영 체제는 새 줄을 정의하고 인식하는 고유한 방법을 가질 수 있습니다. Java에서는 운영 체제 줄 구분 기호를 얻는 것이 매우 쉽습니다.

```java
String newLine = System.getProperty("line.separator");
```
다음 섹션에서 이 newLine 을 사용 하여 여러 줄 문자열을 만들 것입니다.

## 4. String concat
문자열 연결은 여러 줄 문자열을 만드는 데 사용할 수 있는 쉬운 기본 방법입니다.
```java
public String stringConcatenation() {
    return "Get busy living"
            .concat(newLine)
            .concat("or")
            .concat(newLine)
            .concat("get busy dying.")
            .concat(newLine)
            .concat("--Stephen King");
}
```
+ 연산자를 사용하는 것은 동일한 결과를 얻는 또 다른 방법입니다. Java 컴파일러는 concat() 및 + 연산자를 같은 방식으로 번역합니다.

```java
public String stringConcatenation() {
    return "Get busy living"
            + newLine
            + "or"
            + newLine
            + "get busy dying."
            + newLine
            + "--Stephen King";
}
```

## 5. String join
Java 8은 일부 문자열과 함께 구분 기호를 인수로 사용하는 String#join 을 도입했습니다. 모든 입력 문자열이 구분 기호와 함께 결합된 최종 문자열을 반환합니다.

```java
public String stringJoin() {
    return String.join(newLine,
                       "Get busy living",
                       "or",
                       "get busy dying.",
                       "--Stephen King");
}
```

## 6. StringBuilder
StringBuilder 는 String 을 빌드하는 도우미 클래스 입니다. StringBuilder 는 StringBuffer를 대체하기 위해 Java 1.5에서 도입되었습니다. 루프에서 거대한 문자열을 빌드하는 데 좋은 선택입니다.
```java
public String stringBuilder() {
    return new StringBuilder()
            .append("Get busy living")
            .append(newLine)
            .append("or")
            .append(newLine)
            .append("get busy dying.")
            .append(newLine)
            .append("--Stephen King")
            .toString();
}
```

## 7. StringWriter
StringWriter 는 여러 줄 문자열을 만드는 데 사용할 수 있는 또 다른 방법입니다. PrintWriter 를 사용하기 때문에 여기서는 newLine 이 필요하지 않습니다. println 함수는 자동으로 새 줄을 추가합니다.
```java
public String stringWriter() {
    StringWriter stringWriter = new StringWriter();
    PrintWriter printWriter = new PrintWriter(stringWriter);
    printWriter.println("Get busy living");
    printWriter.println("or");
    printWriter.println("get busy dying.");
    printWriter.println("--Stephen King");
    return stringWriter.toString();
}
```

## 8. Guava Joiner
이와 같은 간단한 작업을 위해 외부 라이브러리를 사용하는 것은 의미가 없지만 프로젝트에서 이미 다른 용도로 라이브러리를 사용하고 있다면 활용할 수 있습니다. 예를 들어 Google의 Guava 라이브러리는 매우 유명합니다. Guava에는 여러 줄 문자열을 작성할 수 있는 Joiner 클래스 가 있습니다.

```java
public String guavaJoiner() {
    return Joiner.on(newLine).join(ImmutableList.of("Get busy living",
        "or",
        "get busy dying.",
        "--Stephen King"));
}
```

## 9. 파일에서 불러오기
Java는 파일을 있는 그대로 읽습니다. 즉, 텍스트 파일에 여러 줄의 문자열이 있는 경우 파일을 읽을 때 동일한 문자열을 갖게 됩니다. Java에서 파일을 읽는 방법에는 여러 가지가 있습니다 .

실제로 코드에서 긴 문자열을 분리하는 것이 좋습니다.
```java
public String loadFromFile() throws IOException {
    return new String(Files.readAllBytes(Paths.get("src/main/resources/stephenking.txt")));
}
```
