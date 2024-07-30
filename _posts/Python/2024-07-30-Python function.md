---
layout: single
title:  "Python 기초 function"
categories: Python
tag: [Python]
toc: true
author_profile: false
---

<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>


## Function이란?



**수학적인 의미의 함수와 개념은 비슷하지만 역할이 다르다.**





- input이 들어와서 output이 정해진 규칙에 따라 나온다는 개념은 같지만, 프로그램에서의 하나의 함수는 **하나의 기능**을 나타낸다.





- 정확하게 함수는 특정 기능을 구현한 **코드 묶음**이다.





- def 함수이름(param1, param2, ... ):

        <statement1>

        <statement2>

      return





- 함수를 쓰는 이유는 **재사용성** 때문이다.


> 함수를 사용하는 가장 중요한 이유는 재사용성 때문이다. Reusability라고 하며, ***똑같은 구조의 코드가 반복되는 것을 피하기 위해 사용***된다. 똑같은 구조의 코드는 보통 한 가지의 기능 단위로 묶이게 되며, 이 기능 단위를 코드로 묶어서 함수로 만든다.


## Python Function Definition



```python
# user-defined function
# default argument를 사용할 때는 non-default argument들을 모두 앞에 나열한 뒤에 사용해야 한다.
def foo(a : int, b : int) -> int:    # signature (added tyep hinting)
    '''
    a, b는 정수이고 foo 함수는 a, b를 입력받아서 더하는 기능을 제공합니다.
    더한 값을 return합니다.
    '''
    # docstring
    c = a + b
    return c

foo(15,16)

# built-in function
print()
input()
len()
type()
...
```

<pre>
31
</pre>
- 정확한 용어 구분은 중요하지만, 보통은 parameter라고 총칭한다. 크게 중요하진 않다.


#### 기억해야 할 것은, input --- (Function) ----> output 의 구조이며, 이 때 어떤 input parameter가 들어가서, 어떤 output parameter가 나오는지 주목해야한다.


#### 연습삼아, 사칙연산을 모두 함수로 만들어보자.



```python
def div(a:int, b:int) ->float:  ## function definition
    '''
    a, b는 정수이고 a를 b로 나눈 값을 return하는 함수
    '''
    if b == 0:
        print('0으로 나누는 연산은 지원하지 않습니다.')
    else:
        c = a / b
        return c
    
def div(a:int, b:int) ->float:  ## try-except 함수는 어떤 에러인지 지정해줘야함 남발하면 안된다.
    '''
    a, b는 정수이고 a를 b로 나눈 값을 return하는 함수
    '''
    try:          # 원래 하려고했던 코드 (에러가 발생할 것 코드)
        c = a / b
        return c
    except ZeroDivisionError: # 에러가 발생했을 때 실행할 코드 (에러가 발생하지 않고 에러 발생 대신에 except 내부의 코드를 실행한다.) 
        print('0으로 나누는 연산은 지원하지 않습니다.')

div(9, 3) ## function call
div(11, 2)
div(0, 8)
div(8, 0)
```

<pre>
0으로 나누는 연산은 지원하지 않습니다.
</pre>
### 함수 정의의 다양한 형태를 연습해보자!


#### 1. 가장 흔하게 사용되는 경우 -> 함수 parameter와 return이 모두 존재하는 경우.



```python
def x(a, b):
    return a + b
```

#### 2. 함수 parameter는 없고 return이 존재하는 경우.



```python
def x():
    return "Hi"
```

#### 3. 함수 parameter는 있는데 return이 없는 경우.



```python
def x(a):
    a = a + 1
```

#### 4. 함수 parameter도 없고 return도 없는 경우.



```python
def x():
    print("Hello World")
    
x()
```

<pre>
Hello World
</pre>
#### Q. 만약에 함수의 입력 parameter의 개수를 모를땐 어떻게 해야할까?



```python
# *(asterisk)를 앞에 붙이는 것으로 여러개의 parameter를 받아서 tuple로 변환하여 준다.   
def add_many(*args): # **kwargs : 딕셔너리 형태
    # 파라미터로 입력받은 모든 숫자를 더해서 return 해주는 함수.
    ## TO-DO
    total = sum(args)
    return total

def add_many(*args):
    total = 0  # initialization
    for i in args:
        total += i
    return total

add_many(1, 2, 3, 4, 5)
add_many(1, 2, 3)
add_many(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

<pre>
55
</pre>
#### Q. 코드를 작성할 때, 언제 이 부분은 함수로 구현해야겠다 라고 판단할 수 있을까?



***A. 똑같은 코드가 2번 이상 반복될 때.***


### 파라미터에 대해 조금 더 알아봅시다!



- 함수에서 사용되는 변수들에게는 효력 범위와 수명이 있습니다.


Q. 만약에 함수의 파라미터 변수 이름과, 함수를 호출하는 argument의 이름이 같은 경우에 어떻게 될까?





- 파이썬에서는 함수의 파라미터로, int/float/str/tuple(immutable data type)이 넘어가는 경우에는 내부에서 해당 변수를 수정해도 바뀌지 않습니다.



- 파이썬에서는 함수의 파라미터로, list/dict/set(mutable data type)이 넘어가는 경우에는 내부에서 해당 변수가 바뀝니다.



```python
# global variable
# ipynb 파일들에서는 global variable이 한번 실행되면 해당 프로그램이 종료될때까지 lifetime을 가진다.
# global variable의 scope는 해당 변수를 정의한 ipynb 파일 전체입니다.

name = "Kim"
print(f"1. {name}")

def change_name(name):
    # 함수 내부(code block)에서 정의된 변수를 local variable이라고 합니다.
    # local variable의 lifetime은 함수가 종료될 때(return을 실행할 때)까지입니다.
    # local variable의 scope는 해당 함수 내부입니다.
    print(f"2. {name}")
    name = 'Lee'
    print(f"3. {name}")
    return name
    print(f"4. {name}")

print(f"5. {name}")
change_name(name)
print(f"6. {name}")
change_name(name)
print(f"7. {name}")
```

<pre>
1. Kim
5. Kim
2. Kim
3. Lee
6. Kim
2. Kim
3. Lee
7. Kim
</pre>

```python
# parameter passing할 때 mutable object가 넘어가면 해당 object 자체가 넘어갑니다.

def change_list(L):
    # local variable
    print(f"2. {L}")
    L[0] = 4
    L.pop()
    print(f"3. {L}")

# global variable
L = [1, 2, 3]
print(f"1. {L}")
change_list(L)
print(f"4. {L}")
```

<pre>
1. [1, 2, 3]
2. [1, 2, 3]
3. [4, 2]
4. [4, 2]
</pre>
### Lambda 함수를 사용해보자!

> Lambda Expression





- 굉장히 간단한 함수가 있는 경우, 한 줄짜리 함수로 간편하게 사용할 수 있다.



- 이런 함수를 Lambda 함수라고 하며, lambda 함수와 반복문을 통해 함수의 정의없이 다양한 프로그래밍이 가능하다.



```python
def add(a, b):
    return a+b

# lambda function, lambda expression, inline function --> function을 parameter passing해야하는 경우에 많이 사용함
# lambda 함수로 바꾸면?
f = lambda a,b : a+b

print(add(3, 5))
print(f(3, 5))
```

<pre>
8
8
</pre>

```python
# e.g.
L = [3, 4, 5]
L.sort()

L2 = ['a', 'z', 'c']
L2.sort()

L3 = ['kim', 'jessica', 'young', 'park']
L3.sort(key = lambda s:len(s)) # [3, 7, 5, 4]를 sorting함 ---> 글자 길이순으로 정렬이 가능함
L3
```

<pre>
['kim', 'park', 'young', 'jessica']
</pre>