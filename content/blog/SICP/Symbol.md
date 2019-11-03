---
title: Symbol
date: 2019-02-20 18:02:47
category: SCIP
---

# Symbol

> integer, char, string 말고 다른거

## Symbolic differentiation

> 실제 값을 받는게 아니라 결과값을 심볼로 다루고 싶다. 아래 예제처럼

* x^2/dx = 2x
* xy/dx = y
* (x^2+x)/dx = 2x+1

### type

> deriv : < expr >  < var > => < expr >

* < expr >
  * "x" => Var x
  * "3" => Number 3
  * "x + 3" => Add ..
  * "5 * y" = > Mul ..

```fs
  type Expr =
    | Number of int
    | Var of string
    | Add of Expr * Expr
    | Mul of Expr * Expr
``` 