---
title: Kotlin 공부하기
tags: kotlin
---

# 문법

## 패키지 선언하기

*Java* 처럼 소스파일에 상단에 명시해야 함.

```kotlin
package my.demo

import java.util.*
```

- 디렉토리와 패키지명을 일치시킬 필요 없음.

## 기본 타입

### 숫자

```kotlin
var double1: Double = 123.5
var double2: Double = 123.5e10
var float: Float = 123.5f // 대문자 `F`도 가능.
var long: Long = 123L
var int: Int = 123
var hex: Int = 0x0F
var binary: Int = 0b00001011
var short: Short =
var byte: Byte =
```


`_` 와 함께 표현 가능.

```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

`null` 을 포함할 수 있는 참조(`?`로 끝나는..)는 자동 박싱됨.

```kotlin
val a: Int = 10000
print(a === a) // Prints 'true'
val boxedA: Int? = a
val anotherBoxedA: Int? = a
print(boxedA === anotherBoxedA) // !!!Prints 'false'!!!
```


// 박스 내용물 비교도 가능
```kotlin
val a: Int = 10000
print(a == a) // Prints 'true'
val boxedA: Int? = a
val anotherBoxedA: Int? = a
print(boxedA == anotherBoxedA) // Prints 'true'
```

## 함수 선언하기

기본 형태.

```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```


표현식(_an expression body_) 형태

```kotlin
fun sum(a: Int, b: Int) = a + b
```


_Java_ 의 `void` 와 같이 리턴값이 없을 경우, `Unit` 을 사용하며 생략 가능.
_String Interpolation_(= kotlin 용어는, _string templates_ 인듯) 도 사용가능.

```kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
```

## 변수 선언하기

한번만 할당 가능한 읽기 전용 로컬 변수는 `val` 을 사용.

```kotlin
val a: Int = 1
val b = 2 // 초기 값이 있을 경우 타입을 추론 함.
val c: Int // 초기 값이 없을 경우 탕입 필수!
c = 3
```


변경 가능한 변수는 `var` 을 사용.

```kotlin
var x = 5
x += 1
```


최 상위 변수(Top-level variables)
```kotlin
val PI = 3.14
var x = 0

fun incrementX() { 
    x += 1 
}
```

## 주석

_Java_ 나 _JavaScript_ 와 유사함.
블럭 형태는 _Java_ 와 달리 중첩해서 사용 가능.

```kotlin
// This is an end-of-line comment

/* This is a block comment
   on multiple lines. */
```

## 문자열 템플릿


문자열 보간(String Interpolation.)

```kotlin
var a = 1
// simple name in template:
val s1 = "a is $a" 

a = 2
// arbitrary expression in template:
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

## 조건문

```kotlin
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
```


표현식(_expressions_) 에서도 사용 가능
```kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```


## null


`null` 을 리턴할 수 있는 함수는 `?` 키워드를 사용
```kotlin
fun parseInt(str: String): Int? {
    // ...
}
```


**nullable** 한 값을 리턴하는 방법.

```kotlin
// 아무것도 반환하지 않거나,
fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    if (x != null && y != null) {
        println(x * y)
    }
    else {
        // 바로 이 부분 처럼.
        println("either '$arg1' or '$arg2' is not a number")
    }    
}

// 명시적으로 아무것도 반환하지 않으면 됨.
fun printProduct(arg1: String, arg2: String) {
    // .. 생략 ..
    if (x == null) {
        println("Wrong number format in arg1: '$arg1'")
        return
    }
    if (y == null) {
        println("Wrong number format in arg2: '$arg2'")
        return
    }
    println(x * y)
}
```

## 타입 검사 및 자동 캐스팅.

`is` 로 표현식의 타입을 검사할 수 있음.
불변의 값이나 속성(property) 을 특정 타입으로 검사 후에는 명시적은 캐스팅이 필요하지 않음.

```kotlin
// 예1.
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // 요 영역(branch) 에서는, `obj` 는 `String` 으로 간주함. 
        return obj.length
    }

    // 밖은 여전히 `obj` 는 `Any` 타입
    return null
}

// 예2.
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // 요 영역(branch) 에서는, `obj` 는 `String` 으로 간주함. 
    return obj.length
}

// 예3
fun getStringLength(obj: Any): Int? {
    // `&&` 연산의 오른쪽 영역에서는 `obj` 를 `String` 으로 간주함.
    if (obj is String && obj.length > 0) {
        return obj.length
    }

    return null
}
```
## 반복문

### `for`

`.indices` 는 뭘까?

```kotlin
// 예1.
val items = listOf("apple", "banana", "kiwifruit")
for (item in items) {
    println(item)
}

// 예2.
val items = listOf("apple", "banana", "kiwifruit")
for (index in items.indices) {
    println("item at $index is ${items[index]}")
}
```

### `while`

```kotlin
val items = listOf("apple", "banana", "kiwifruit")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}
```


## `when`

인상적인 부분.

```kotlin
fun describe(obj: Any): String =
when (obj) {
    1          -> "One"
    "Hello"    -> "Greeting"
    is Long    -> "Long"
    !is String -> "Not a string"
    else       -> "Unknown"
}
```

## 범위(ranges)

```kotlin
// 예1. '범위' 구문을 이용해서 조건문을 작성할 수 있음.
val x = 10
val y = 9
if (x in 1..y+1) {
    println("fits in range")
}


// 예2. '!'과 함께 '포함하지 않음' 을 표현 가능.
val list = listOf("a", "b", "c")

if (-1 !in 0..list.lastIndex) {
    println("-1 is out of range")
}
// 아마도 `0..` 은 생략 가능한 듯.
if (list.size !in list.indices) {
    println("list size is out of valid list indices range too")
}


// 예3. 범위를 이용한 반복.
for (x in 1..5) {
    print(x)
}


// 예4. Step, 건너뛰기.
for (x in 1..10 step 2) {
    print(x)
}

// 예5. 역순.
for (x in 9 downTo 0 step 3) {
    print(x)
}
```

## 컬렉션 사용

```kotlin
// 예1. 컬렉션 순회(Iterating)
for (item in items) {
    println(item)
}


// 예2. `in` 연산자를 이용한 컬렉션 내 데이터 포함여부 확인.
when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}

// 예3. 람다 표현식을 이용한 _fitler_ 와 _map_
fruits
  .filter { it.startsWith("a") }
  .sortedBy { it }
  .map { it.toUpperCase() }
  .forEach { println(it) }
```

## 객체 만들기

```kotlin
val rectangle = Rectangle(5.0, 2.0) // Java 와 같은 'new' 키워드가 필요 없다.
val triangle = Triangle(3.0, 4.0, 5.0)
```


# 참조
- [Kotlin Reference](https://kotlinlang.org/docs/reference/)