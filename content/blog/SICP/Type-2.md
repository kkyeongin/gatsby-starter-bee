---
title: Type(2)
date: 2019-02-20 18:02:47
category: SCIP
---

# Type2

## type of procedure

* input
  * type
  * number
  * order
* type of result (->)

```fs
let sum (a+b) = a+b
; int * int -> int
```

## Advanced in Types

* AND type ; product type(곱셈)
  * Tuple
  * Record
* OR type ; add type adition
  * List
  * Option
  * Union

## enviroment table

> 에 넣는 걸을 variable binding 으로 표현 한다. 우리가 흔히 아는 stack이랑 같은 공간이라 생각하면 된다.

## curring

```fs
let add x y = x + y
; curring
; add = lambda x (lambda y ( x + y ))
let incr = add 1
```

## Recursive type

> type(or | and) 안에 type(or | and) 이 올 수 있다.

```fs
type Natural =
| Zero
| Succ of Natural
```

## Tree

```fs
module Tree

; OR Type * AND Type
type 'a Tree =
    | Leaf
    | Node of 'a Tree * 'a * 'a Tree 

let a = Leaf
let b = Node(Node(Leaf, 2, Leaf), 1, Leaf)
let c = Node(Leaf, 1, Node(Leaf, 2, Leaf))

let rec sum tree =
    match tree with
    | Leaf -> 0
    | Node (x, i, y) -> sum x + i + sum y
```

## Operator

> Operator 와 Procedure는 거의 같다. (prefix 말고)

```fs
module Operator

let (++) a b =  a+b
; (++) a b
; a ++ b
let foo = (++) 3 ; curring
; (++) type: int -> int -> int
; foo type: int -> int

```