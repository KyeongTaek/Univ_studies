# [프로그래밍언어론 9주차]
## LISP 개요
- list를 이용해 모든 것을 표현한다(안에 함수 기능도 있긴 하다)
- 인공지능 연구 목적으로 사용되었다

## 특징
- 프로그램과 데이터를 동일하게, 즉 프로그램도 데이터처럼 취급한다
- 재귀호출을 굉장히 많이 사용한다
- 자료구조로 연결리스트를 사용한다
- garbage collection(동적 할당된 메모리 중 사용할 수 없는 영역을 탐지해 자동 해제)을 지원한다


## Symbolic expression(s-expression)
### atomic symbol
최소 단위. 대문자나 숫자로 구성되며, 시작은 대문자이다
### s-expression
- 하나의 atomic symbol, 혹은 atomic symbol들의 나열이다
    - ex. `(A.(B.C))`

- s-expression은 list notation으로 표기할 수도 있다
    * ex. `(m1 m2 ... mn)` => `(m1.(m2.( ... (mn.NIL) ... )))`
- NIL은 비어 있는 list로, atom이면서 동시에 list인 element이다
    * false의 의미로도 사용된다
    * ex. `(A B)` => `(A.(B.NIL))`
        * 마치 연결리스트에서 head를 옮겨가듯이
        * NIL은 list nontation에서 괄호쌍의 수만큼 필요하다


## Elementary functions
### 1. car
- list의 가장 앞에 있는 element을 반환한다
- list가 아니거나, 비어있는 경우 NIL을 반환한다
- ex. `(car '(a b c))` => `a`
- +따옴표: list를 데이터로 보겠다는 의미

### 2. cdr
- list에서 가장 앞에 있는 element를 제외한 뒷부분을 반환한다

### 3. cons
- syntax: `(cons s-expr list)`
- list의 맨 앞에 s-expr을 삽입한다
- ex. `(cons 'a '(b c))` => `(a b c)`
- ex. `(cons 'a 'b)` => `(a.b)`
    - .이 있다면 list가 아니다!(그저 s-expression)

- ex. `(car '(cons a (b c)))` => `cons`
    - 따옴표 주의하기!!
- ex. `(car (cons 'a'(b c)))` => `a`
- ex. `(car  (cons a(b c)))` => `a의 값
	- a, b, c는 변수나 함수로 취급된다!

### 4. eq
- s-expr1과 s-expr2가 같은 **메모리주소**를 가진다면 T를, 다르다면 NIL을 반환한다
- ex. `(eq '(a b) '(a b))` => **`NIL`**
    - cf. 파이썬에서 a=[1,2]와 b=[1,2]일 때 a와 b는 메모리주소가 다르다
    - cf. 파이썬에서 a=3, b=3이라면 a와 b는 메모리주소가 같다(메모리 비효율 막기 위해)

### 5. atom
- s-expr이 atomic symbol인 경우 T를 반환한다
- ex. `(atom 'a)` => `T`
- ex. `(atom NIL)` => `T`

### 6. 산술연산, 논리연산
- prefix notation으로, 연산자가 먼저 나온다
- ex. `(+ 2 3 (*2 4) (/ 6 2))` => `(+2 3 8 3)` => `16`
- ex. `(and (atom 'a) (eq 'a 'a))` => `T`

### 7. 분기문
- syntax: `(cond (p e))`
- p는 참 또는 거짓을 의미할 수 있는 expression으로, 참이라면 e를 반환한다
- 마지막의 (t e)는 c언어에서의 else이다
    - 겹치는 게 하나도 없었을 때 e 반환

- ex. `(cond ((eq (cdr x) nil) nil)`  
		`((atom (car x)) (cons (car x) lis))`  
		`(t (cdr x)) )`

### 8. 함수
- 정의 시 syntax: `(defun func-name (arg) body)`
    - arg를 받아 body를 수행한다
- ex. `(defun second (lis) (car (cdr lis)))`
- 호출: `func-name arg`
- ex. `(second '(a b c))`

### 9. Recursive algorithm
- 이미 정의한 함수를 사용한 간단한 예를 이용해 알고리즘을 구상해야 한다
- 이때 Recursion의 종료 조건을 명확하게 해야 한다
 
 ### 9-1. last
 - list의 last element를 반환하는 함수
 - 종료 조건: list에서 더 이상 element를 꺼낼 수 없을 때 => `(null lis)`
 ```LISP
 (defun last (lis)
    (cond ((null lis) nil)              ; if initial argument is nil -> return nil
          ((null (cdr lis)) (car lis))  ; actual boundary condition
          (t (last(cdr lis))) ) )       ; recursion
```
 ### 9-2. removelast
 - last element만 제거한 list를 반환하는 함수
 - 종료 조건: `(null (cdr lis) nil)`
 ```LISP
 (defun removelast(lis)
    (cond ((null lis) nil)                              ; if initial argument is nil -> return nil
          ((null (cdr lis)) nil)                        ; actual boundary condition
          (t (cons (car lis) (removelast (cdr lis)))) )); recursion
```
### 9-3. append
- lis1의 element를 하나씩 꺼내 lis2와 결합하기
- ex. `(append '(a b) '(c d))` => `(a b c d)`
- cf. `(cons '(a b) '(c d))` => `((a b) c d)`
-종료 조건: `(null lis1)`
### 9-4. equal
- ex. `(equal '(a) '(a))` => `T`
- cf. `(eq '(a) '(a))` => `NIL`
### 9-5-1. reverse (1)
- 내부의 list 순서는 건드리지 않고, 전체 element의 순서만 뒤집은 list
- ex. `(reverse '((a b) c d))` => `(d c (a b))`
- `(cdr lis)`를 recursive하게 적용한 결과의 뒤에 `(car lis)`를 append해서 구현할 수 있다
- `cons` 호출 횟수가 `n(n-1)/2` => 효율적이지 않음
### 9-5-2. reverse (2)
- `9-5-1`의 `reverse`를 결과의 일부를 저장하는 `collection variable`을 사용하여 개선할 수 있다
- `cons` 호출 횟수가 `n`
### 9-6. revall
- 내부의 list 순서도 뒤집고, 전체 element의 순서도 뒤집은 list
- ex. `revall '((a b) c d))` => `(d c (b a))`
- `car lis1`이 atomic symbol이 아니라 내부 list인 경우, 이를 하나의 list로 보고 다시 `revall(car lis1)`을 호출해서 내부 list에 대해서도 순서를 뒤집는다.
