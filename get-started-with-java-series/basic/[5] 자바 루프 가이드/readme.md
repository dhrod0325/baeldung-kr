# 자바 루프 가이드

## 1. 개요
이 기사에서는 루프를 사용하여 명령문 또는 명령문 그룹을 반복적으로 실행하는 Java 언어의 핵심 측면을 살펴보겠습니다.

## 2. 루프 소개

프로그래밍 언어에서 루프는 제어하는 Boolean-expression 이 false 로 평가 될 때까지 일련의 명령 실행을 용이하게 하는 기능입니다.

Java는 프로그래밍 요구 사항에 맞는 다양한 유형의 루프를 제공합니다. 
각 루프에는 고유한 목적과 제공하기에 적합한 사용 사례가 있습니다.

다음은 Java에서 찾을 수 있는 루프 유형입니다.

- 단순 for 루프
- 향상된 for-each 루프
- while 루프
- Do-While 루프

## 3. For 루프
for 루프는 루프 카운터를 증가 및 평가하여 특정 작업을 반복할 수 있는 제어 구조입니다.   
첫 번째 반복 전에 루프 카운터가 초기화된 다음 조건 평가가 수행되고 단계 정의가 수행됩니다(일반적으로 단순 증분).   

for 루프 의 구문은 다음과 같습니다.

```java
for (initialization; Boolean-expression; step) 
  statement;
```

간단한 예에서 살펴보겠습니다.   

```java
for (int i = 0; i < 5; i++) {
    System.out.println("Simple for loop: i = " + i);
}
```

for 문에 사용되는 초기화 , 부울 식 및 단계 는 선택 사항입니다. 다음은 무한 for 루프의 예입니다.

```java
for ( ; ; ) {
    // Infinite for loop
}
```

### 3-1 루프에 레이블 지정

for 루프 에 레이블을 지정할 수도 있습니다 . 특정 for 루프 에서 중단 / 계속할 수 있도록 for 루프가 중첩되어 있으면 유용합니다 .

```java
aa: for (int i = 1; i <= 3; i++) {
    if (i == 1)
      continue;
    bb: for (int j = 1; j <= 3; j++) {
        if (i == 2 && j == 2) {
            break aa;
        }
        System.out.println(i + " " + j);
    }
}

```

### 3-2 for 루프 개선
Java 5 이후 로 향상된 for 라는 두 번째 종류의 for 루프 가 있어 배열이나 컬렉션의 모든 요소를 더 쉽게 반복할 수 있습니다.

향상된 for 루프 의 구문은 다음과 같습니다.
```java
for(Type item : items)
  statement;
```
이 루프는 표준 for 루프와 비교하여 단순화되었으므로 루프를 초기화할 때 두 가지만 선언하면 됩니다.   
따라서 다음과 같이 말할 수 있습니다. 항목의 각 요소에 대해 항목 변수에 요소를 할당하고 루프 본문을 실행합니다.   

간단한 예를 살펴보겠습니다.

```java
int[] intArr = { 0,1,2,3,4 }; 
for (int num : intArr) {
    System.out.println("Enhanced for-each loop: i = " + num);
}
```

이를 사용하여 다양한 Java 데이터 구조를 반복할 수 있습니다.   
List<String> 목록 개체가 주어지면 반복할 수 있습니다.
```java
for (String item : list) {
    System.out.println(item);
}
```

Set<String> set 에 대해서도 유사하게 반복할 수 있습니다 .
```java
for (String item : set) {
    System.out.println(item);
}
```

그리고 Map<String,Integer> 맵 이 주어지면 이를 반복할 수도 있습니다.
```java
for (Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(
      "Key: " + entry.getKey() + 
      " - " + 
      "Value: " + entry.getValue());
}
```

### 3-3 Iterable.forEach()
Java 8부터 for-each 루프를 약간 다른 방식으로 활용할 수 있습니다. 
이제 Iterable 인터페이스에 수행하려는 작업을 나타내는 람다식을 허용하는 전용 forEach() 메서드가 있습니다.

내부적으로는 단순히 작업을 표준 루프에 위임합니다.

```java
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```

예를 살펴보겠습니다.

```java
List<String> names = new ArrayList<>();
names.add("Larry");
names.add("Steve");
names.add("James");
names.add("Conan");
names.add("Ellen");

names.forEach(name -> System.out.println(name));
```

## 4. while 루프
while 루프는 Java 의 가장 기본적인 루프 문입니다. 제어하는 부울 표현식 이 true 인 동안 명령문 또는 명령문 블록을 반복 합니다.

```
while (Boolean-expression) 
    statement;
```

루프의 부울 표현식 은 루프의 첫 번째 반복 전에 평가 됩니다. 즉, 조건이 false로 평가되면 루프가 한 번도 실행되지 않을 수 있습니다.

## 5. Do-While 루프
do-while 루프는 루프의 첫 번째 반복 후에 첫 번째 조건 평가가 발생한다는 사실을 제외하고는 while 루프와 동일하게 작동합니다.

```java

do {
    statement;
} while (Boolean-expression);

```