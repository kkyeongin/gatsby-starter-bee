---
title: Type(1)
date: 2019-02-20 18:02:47
category: SCIP
---

# Type(1)

> 값들의 집함; 모든 value는 타입을 가지고 있고, procedure 또한 그렇다

* Types : compound data
* Scheme
  * Pair
  * List
  * Rat(유리수)

## 7을 설명하면?

* 7을 7명, 7개, 7mm, 7pt 등등 여러 의미를 다룰 수 있음
* 7 자체는 abstraction임을 뜻함
* 그래서 type을 정확히 얘기해 줘야함.

```Fs
;; 1,2,3 ... ,10, ACE = 11, TEEAN = 12, LOAD = 13
```

여기서 이렇게 정의 되어 있을때 처리값이 1이나, 15가 들어온다면
에러를 내는 주범이 될 것 이다.

## Function Type

### ex

* lambda (a b c) (if (> a 0)(+ b c)(- b c)))
  * number * number * number -> number

* lambda (p) (if (p) "hi" "bye"))
  * number -> string

### curried function

 > ex, int -> int -> int

```Fs
let add a b = a + b

let incr = add 1

; incr 2
; 3
; incr 3
; 4

```

프로시저를 한번 정의하고 재사용성을 높임

## User defined types

* Record
* Union
* Polymorphosm

## Record  

* AND Type

```Fs
type Person = {
    Name: string
    Aag: int
}

let p = ("kyeongin", 28) ; 두개를 동시에 줘야해서 And Type
; val p : string * int

let p2 = {
    Name = "Kyeongin"
    Age = 28
}
; val p2 : Person = {Name = "Kyeongin", Age = 28}

```

* Selector가 필요 없음.

## Union

* OR Type

```Fs
type Gender =
    | Male
    | Female

type Color
    | Red
    | Green
    | Blue
```

Red의 Type은 Color

## Polymorphosm

```Fs
let head ls = 
    match ls with
    | x :: xs -> x
    | _ -> fallwith "head error"
```

### 그래서 list의 type은?

> 'a list -> 'a

'a를 type variable 이라고 표현함.
'a의 type에 따라서 list의 type이 변하며, 무엇이 오던 간에 같은 동작을 함.

### 정리
