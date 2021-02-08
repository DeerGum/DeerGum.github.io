---
title:  "[Kotlin] 스코프 함수(Scope Functions)"
search: true
categories: 
  - kotlin
tags:
  - Kotlin
  - Kotlin Standard Library
  - Scope Functions
last_modified_at: 2021-02-06T08:06:00-05:00
---
<br>
작년에 코틀린으로 안드로이드 앱 제작을 해보면서 코틀린에 대한 이해도가 많이 부족하다는 것을 깨달았다. <br>
그래서 이번에 부스트코스의 코틀린 강의를 들으면서 공부를 하고 있는데 안드로이드 개발을 할 때 많이 헷갈렸던 스코프 함수에 대해 정리해보았다.

<br>
<br>

## 스코프 함수(Scope Functions)란?
- 코틀린 표준 라이브러리에 포함된 객체 `Context`내의 코드 블록을 실행시켜주는 함수

코틀린에 존재하는 스코프 함수는 다음과 같다.

| Function | Object reference | Retrun value   | Is extension function                        |
|----------|------------------|----------------|----------------------------------------------|
| **let**  | it               | Lambda result  | Yes                                          |
| **run**  | this             | Lambda result  | Yes                                          |
| **run**  | -                | Lambda result  | No: called without the context object        |
| **with** | this             | Lambda result  | No: takes the context object as an argument. |
| **apply**| this             | Context object | Yes                                          |
| **also** | it               | Context object | Yes                                          |

출처 - [공식문서](https://kotlinlang.org/docs/reference/scope-functions.html)
<br>
<br>

## 함수 정의
### let
- 확장함수 형태로 객체( `it` )를 코드블록으로 넘기고 블록의 결과값을 반환

```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R { ... return block(this) }
```

### also
- 확장함수 형태로 객체( `it` )를 코드블록으로 넘기고 객체를 반환

```kotlin
public inline fun <T> T.also(block: (T) -> Unit): T { block(this); return this}
```

### apply
- 확장함수 형태로 객체( `this` )를 코드블록으로 넘기고 객체를 반환

```kotlin
public inline fun <T> T.apply(block: T.() -> Unit): T { block(); return this }
```

### run
- 인자가 없는 익명 함수 형태로 블록의 결과값을 반환
- 확장함수 형태로 객체( `this` )를 코드블록으로 넘기고 블록의 결과값을 반환

```kotlin
public inline fun <R> run(block: () -> R): R = return block()
public inline fun <T, R> T.run(block: T.() -> R): R = return block()
```

### with
- 인자가 있는 익명 함수 형태로 인자로 받는 객체( `this` )를 코드블록으로 전달하고 결과값을 반환

```kotlin
public inline fun <T, R> with(receiver: T, block: T.() -> R): R = receiver.block()
```

<br>
<br>

## 함수의 특징
### let과 also

`let`과 `also`함수는 상당히 비슷하다. 무슨차이가 있는 걸까?

다음 예제코드를 통해 알 수 있다.

```kotlin
var n = 1
n = n.let { it + 3 }
println(n) // 4

var m = 1
m = m.also { it + 3 }
println(m) // 1
```

`let`함수는 코드블록 ( `{...}` )의 결과값( `it + 3` )을 반환하고 `also`함수는 코드블록 실행 후 자기자신( `m` )을 반환 받는다.

### run과 with
익명 함수 형태의 `run`과 `with`는 인수의 유무만 다르고 동작이 같다. 

`with`는 단독 함수로 사용되어 세이프 콜(`?.`)을 지원하지 않기 떄문에 `let`과 같이 사용해서 세이프 콜을 해줄 수 있다. (확장함수 형태의 스코프 함수들은 세이프 콜 지원)

```kotlin
supportActionBar?.let {
    with(it) {
        setDisplayHomeAsUpEnabled(true)
        setHomeAsUpIndicator(R.drawable.ic_clear_white)    
    }
}
```

`let`과 `with`를 같이 쓰면 인수가 있는 `run` 효과를 낼 수 있다.

### 사용예제
```kotlin
fun main() {
    data class Person(var name: String, var skills: String)
    var person = Person("Kildong", "Kotlin")

    val returnObj = person.apply {
        this.name = "Sean"
        skills = "Java" //this 생략가능
        "success"   //사용되지 않음
    }   //returnObj : Person

    var returnObj2 = person.run {
        this.name = "Dooly"
        skills = "C#"   //this 생략가능
        "success"   //반환
    }   //returnObj2 : String

    val returnObj3 = with(person) {
        this.name = "Dooly"
        skills = "C#"   //this 생략가능
        "success"   //반환
    }   //returnObj3 : String

    val returnObj4 = person.let {
        it.name = "Jarry" //it 생략 불가능
        it.skills = "Kotlin"
        "success"   //반환
    }   //returnObj4 : String

    val returnObj5 = person.also {
        it.name = "Kildong" //it 생략 불가능
        it.skills = "Java"
        "success"   //사용되지 않음
    }   //returnObj5: Person
}
```