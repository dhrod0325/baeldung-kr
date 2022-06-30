## 1. 소개

이 튜토리얼에서는 Java에서 비어 있거나 비어 있는 문자열을 확인하는 몇 가지 방법을 살펴보겠습니다.
몇 가지 네이티브 접근 방식과 몇 가지 라이브러리가 있습니다.

## 2. empty vs blank

물론 문자열이 비어 있거나 비어 있는지 아는 것은 매우 일반적이지만 정의와 동일한 페이지에 있는지 확인합시다.

null이거나 길이가 없는 문자열인 경우 문자열을 비어 있는 것으로 간주합니다. 문자열이 공백만으로 구성된 경우 `blank`라고 합니다.

Java의 경우 공백은 공백, 탭 등과 같은 문자입니다. 예를 들어 `Character.isWhitespace` 를 살펴보십시오 

## 3. Empty Strings

### 3.1. Java 6 이상 사용
최소한 Java 6을 사용 중이라면 빈 문자열을 확인하는 가장 간단한 방법은 String#isEmpty 입니다.

```java
boolean isEmptyString(String string) {
    return string.isEmpty();
}
```

null-safe하게 하려면 추가 검사를 추가해야 합니다.

```java
boolean isEmptyString(String string) {
    return string == null || string.isEmpty();
}
```

### 3.2. Java 5 이하
String#isEmpty 는 Java 6과 함께 도입되었습니다. Java 5 이하에서는 String#length를 대신 사용할 수 있습니다.

```java
boolean isEmptyString(String string) {
    return string == null || string.length() == 0;
}
```

사실, String#isEmpty 는 String#length 에 대한 바로 가기일 뿐입니다 .

### 4. Blank Strings
String#isEmpty 및 String#length 모두 빈 문자열 을 확인하는데 사용할 수 있습니다.

빈 문자열도 감지 하려면 String#trim 을 사용하여 이를 수행할 수 있습니다. 검사를 수행하기 전에 모든 선행 및 후행 공백을 제거합니다.

```java
boolean isBlankString(String string) {
    return string == null || string.trim().isEmpty();
}
```

정확히 말하면, String#trim 은 U+0020 이하의 유니코드 코드를 가진 모든 선행 및 후행 문자를 제거합니다.

또한 String은 변경할 수 없으므로 trim을 호출해도 실제로 기본 문자열이 변경되지는 않습니다.

위의 접근 방식 외에도 Java 11부터 trimming 대신 isBlank() 메서드를 사용할 수도 있습니다.

```java
boolean isBlankString(String string) {
    return string == null || string.isBlank();
}
```

## 5. Bean Validation

빈 문자열을 확인하는 또 다른 방법은 정규식입니다. 이것은 예를 들어 Java Bean Validation 에서 유용합니다.

```java
@Pattern(regexp = "\\A(?!\\s*\\Z).+")
String someString;
```

## 6. With Apache Commons

종속성을 추가해도 괜찮다면 Apache Commons Lang 을 사용할 수 있습니다. 여기에는 Java용 도우미가 많이 있습니다.

Maven을 사용하는 경우 pom에 commons-lang3 종속성을 추가해야 합니다.
Gradle을 사용하는 경우 build.gradle에 commons-lang3 종속성을 추가해야 합니다.

무엇보다도 이것은 우리에게 StringUtils를 제공합니다 .

이 클래스는 isEmpty, isBlank 등과 같은 메소드와 함께 제공 됩니다.

```java
StringUtils.isBlank(string);
```

이 호출은 자체 isBlankString 메서드와 동일합니다. null 안전하고 공백도 확인합니다.

## 7. With Guava
특정 문자열 관련 유틸리티를 제공하는 또 다른 잘 알려진 라이브러리는 Google의 Guava입니다. 

버전 23.1부터 Guava에는 android 및 jre 의 두 가지 버전이 있습니다 . 

Android 버전은 Android 및 Java 7을 대상으로 하는 반면 JRE 버전은 Java 8을 대상으로 합니다.

Guavas Strings 클래스는 Strings.isNullOrEmpty 메소드와 함께 제공됩니다.

```java
Strings.isNullOrEmpty(string)
```
