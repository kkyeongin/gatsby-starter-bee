---
title: How to Summarize Procedure
date: 2019-02-19 18:02:89
category: SCIP
---

# 프로시저를 써서 요약하는 방법

* 목차
  1. [프로그램을 짤 때 바탕이 되는 것](#프로그램-짤-때-바탕이-되는-것)
  2. [Procedure vs Process](#Procedure-vs-Process)
  3. [higher-order procedure로 요약하는 방법](#higher-order-procedure로-요약하는-방법)

## 프로그램 짤 때 바탕이 되는 것

좋은 프로그래밍은 내 생각을 짜임새 깊게 나타내는 것이다.
그 중 가장 기본이 되는 것이 아래 3개 이다.

* primitive expression
* means of combination
* means of abstracion

좋은 프로그래밍 언어에는 primitive data & primitive procedure를 나타내고, 잘 조합하여, 추상화 시켜야 한다.

## Procedure vs Process

> 프로시저란, 한 컴퓨터 프로세스가 어떻게 나아가는지(local evolution of a computer process), 곧 지난일을 발판 삼아 다음으로 해야 할 일을 밝힌 것이다.

### How do we create expressions using these precedures?

```cpp
(+ 2 3)
||  | |
||  | |
||  | |___ close parenthesis
||  |___ other expressions
||____ an expression whose value is a procedure
|____ open parenthesis
  
```

* What about the semantics of a combination?

    sub-expression을 평가하고, 첫 번째 표현식의 값을 다른 표현식의 값에 적용합니다.

```cpp
(+(+ 2 3)4)
```

* 위와 같은 복합식(combination expression)은 어떻게 계산 해야 하나?

    위에서 (+ 2 3)식을 풀었고 같은 패턴의 복합식이므로 같은 방법으로 처리하면 된다.
    이걸 보통 recursive(재귀) 프로세스 라고 한다.

### abstraction

1. 식에 이름 부여

    ```cpp
    define score 23
    ```

2. special form

for, while 반목문 같은 경우 special form(특별한 형태)를 가진다.
위에서 (+ 2 3)식을 풀었고 같은 패턴의 복합식이므로 같은 방법으로 처리하면 된다.
이걸 보통 recursive(재귀) 프로세스 라고 한다.

프로시저란, 한 컴퓨터 프로세스가 어떻게 나아가는지(local evolution of a computer process), 곧 지난일을 발판 삼아 다음으로 해야 할 일을 밝힌 것이다. 

## higher-order procedure로 요약하는 방법

* 어떻게 추상화를 잘 하는 방법

1. 식에 이름을 정하는 것

```cpp
define score 23
```

2. special form

