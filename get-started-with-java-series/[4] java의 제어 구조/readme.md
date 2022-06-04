# Java의 제어 구조

## 1. 개요
가장 기본적인 의미에서 프로그램은 명령어 목록입니다. 제어 구조는 해당 명령을 통해 취하는 경로를 변경할 수 있는 프로그래밍 블록입니다.
이 자습서에서는 Java의 제어 구조를 탐색합니다.

세 가지 종류의 제어 구조가 있습니다.

**두 개 이상의 경로 중에서 선택하는 데** 사용하는 조건부 분기 . Java에는 세 가지 유형이 있습니다. if/else/else if , 삼항 연산자 및 switch.   
**여러 값/객체를 반복하고 특정 코드 블록을 반복적으로 실행하는데** 사용되는 루프입니다 . Java의 기본 루프 유형은 for , while 및 do while 입니다.   
루프에서 제어 흐름을 변경하는데 사용되는 분기 문. Java에는 break 와 continue 의 두 가지 유형이 있습니다.

## 2. If/Else/Else If
   if/else 문은 가장 기본적인 제어 구조 이지만 프로그래밍에서 의사 결정의 가장 기초로 간주될 수도 있습니다.

if 는 단독으로 사용할 수 있지만 가장 일반적인 사용 시나리오는 if/else 를 사용하여 두 경로 중에서 선택하는 것입니다 .
```java
if (count > 2) {
    System.out.println("Count is higher than 2");
} else {
    System.out.println("Count is lower or equal than 2");
}
```
**이론적으로 if/else 블록 을 무한히 연결하거나 중첩할 수 있지만 이는 코드 가독성을 저하시키므로 권장하지 않습니다.**

이 기사의 나머지 부분에서 대안적인 진술을 탐색할 것입니다.

## 3. 삼항 연산자
삼항 연산자 를 if/else 문 처럼 작동하는 약식 표현으로 사용할 수 있습니다.
if/else 예제를 다시 살펴보겠습니다 .
```java
if (count > 2) {
    System.out.println("Count is higher than 2");
} else {
    System.out.println("Count is lower or equal than 2");
}
```
이것을 다음과 같이 삼항으로 리팩토링할 수 있습니다.

```java
System.out.println(count > 2 ? "Count is higher than 2" : "Count is lower or equal than 2");
```

삼항은 코드를 더 읽기 쉽게 만드는 좋은 방법이 될 수 있지만 **항상 if/else를 대체하는 좋은 방법은 아닙니다.**

## 4. Switch
선택할 수 있는 경우가 여러 개인 경우 switch 문을 사용할 수 있습니다.

간단한 예를 다시 보겠습니다.

```java
int count = 3;
switch (count) {
case 0:
    System.out.println("Count is equal to 0");
    break;
case 1:
    System.out.println("Count is equal to 1");
    break;
default:
    System.out.println("Count is either negative, or higher than 1");
    break;
}
```

세 개 이상의 if/else  문은 읽기 어려울 수 있습니다. 가능한 해결 방법 중 하나로 위와 같이 스위치를 사용할 수 있습니다 .

또한 스위치  에는 사용하기 전에 기억해야 할 범위와 입력 제한 이 있다는 점을 염두에 두십시오.

## 5. Loops
같은 코드를 연속해서 여러 번 반복해야 할 때 루프를 사용 합니다.   
비교 가능한 for 및 while 루프 유형의 간단한 예를 살펴보겠습니다.

```java
for (int i = 1; i <= 50; i++) {
    methodToRepeat();
}

int whileCounter = 1;
while (whileCounter <= 50) {
    methodToRepeat();
    whileCounter++;
}
```

위의 두 코드 블록 모두 methodToRepeat 를 50번 호출합니다.

## 6. break
루프에서 일찍 종료 하려면 break 를 사용해야 합니다.   

간단한 예를 살펴보겠습니다.

```java
List<String> names = getNameList();
String name = "John Doe";
int index = 0;
for ( ; index < names.length; index++) {
    if (names[index].equals(name)) {
        break;
    }
}
```

여기에서 우리는 이름 목록에서 이름을 찾고 있으며 일단 찾으면 찾는 것을 멈추고 싶습니다.   
루프는 일반적으로 완료되지만  여기에서 break 를 사용 하여 이를 단락시키고 일찍 종료합니다.   

## 7. continue
간단히 말해서,  continue는 우리가 속한 루프의 나머지 부분을 건너뛰는 것을 의미합니다. 

```java
List<String> names = getNameList();
String name = "John Doe";
String list = "";
for (int i = 0; i < names.length; i++) { 
    if (names[i].equals(name)) {
        continue;
    }
    list += names[i];
} 
```
여기서는 중복된 이름을 목록에 추가하는 것을 건너뜁니다.

여기에서 보았듯이 break 및 continue 는 반복할 때 편리할 수 있지만 종종 return 문이나 다른 논리로 다시 작성할 수 있습니다.