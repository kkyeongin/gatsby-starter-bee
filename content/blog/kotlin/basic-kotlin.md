---
title: basic kotlin 1
date: 2018-12-09 21:19:51
category: Kotlin
---

## Statement(문)과 Expression(식)의 구분
프로그래밍 언어론에서의 statement 와 expression은 다르다.
누군가 잘 신경쓰지 않을 수 있지만.. 이쪽 관련일을 했던 나로서는 꽤 신선했다.

중요한건 코틀린에선 조건문이아니라 조건식이라는 것이다.
그리고 다른 언어(Java, C++)에서 문이 었던 것들이 코틀린에서는 식으로 나타낸다. (loop문 제외)

## 변수 
코틀린은 자바스크립트랑 비슷하게 생기긴 했지만, 정적(static) 타입 언어다.
type을 쓸때도 있고 안쓸때도 있지만, 컴파일 타임에서 type inference가 일어난다. 
추론이 가능하지 않을 때는 컴파일 에러를 일으킨다.

예를 들어.

```js
var vr = "var is variable, mutable"
val vl = "val is value, immutable"

var compileError 

compileError = "need type (var compileError:String)"
```

## 클래스 & 프로퍼티

코틀린은 java를 100%지원하는 언어이다?
그래서 java <-> kotlin은 변환이 가능하다.

클래스에서 문법적으로 다른점이 있고 꾀나 재미있다.

몇가지를 소개 하자면.

1. 코틀린 클래스의 visibility modifier는 기본 public 이다.

2. 프로퍼티 
    * val - read only 
    * var - read , write ok
    * 클래스의 프로퍼티 이름을 사용하면 getter, setter가 자동으로 호출해 준다. 
    * 커스텀 접근자 

    ```js
        class CustomClass(val v1: Int, val v2: Int){
            val isEqual:Boolean
                get() = v1 == v2
                /*
                //or
                get(){
                    return v1 == v2
                }
                */       
        }
    ```
3. directory & package
    1. package, import java와 같이 사용가능하다. 
    2. 클래스 import 뿐만 아니라 함수 import도 가능하다. 
        - import {directory}.{packageName}.{functionName}