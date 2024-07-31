---
layout: single
title:  "Python 기초 I/O"
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


## I/O (Input / Output) 


### 1. STDIN / STDOUT (Standard IN, Standard OUT)


- 파이썬은 input()을 통해서 stdin을 사용자로부터 입력받을 수 있다.





- 파이썬은 print()를 통해서 stdout을 사용자에게 출력할 수 있다.



```python
# a에 키보드로 입력받은 값을 할당하고 출력해본다.
a = input('숫자를 하나 입력해주세요 : ')   # input 함수로 입력받은 모든 값은 전부 str로 저장된다.
a
```

<pre>
'10'
</pre>
- 파이썬에서는 stdin은 무조건 문자열 타입으로 들어온다. 이를 type casting을 통해서 다른 데이터 타입으로 바꾸어 사용해야 한다.



```python
# 입력받는 값을 숫자라고 가정한 경우.
while True:   # infinite loop
    try:
        number = int(input('숫자를 하나 입력해주세요 : '))
        break
    except ValueError:
        print('정수를 입력해주세요.')

print(f'정수 {number}를 입력받았습니다.')
```

<pre>
정수를 입력해주세요.
정수를 입력해주세요.
정수를 입력해주세요.
정수 10를 입력받았습니다.
</pre>

```python
# 입력받는 값을 숫자라고 가정했는데 문자열이 들어오면 에러가 난다. 이 경우는 type casting이 실패한 경우이다.
flag = True
while flag:   # infinite loop
    try:
        number = int(input('숫자를 하나 입력해주세요 : '))
        flag = False
    except ValueError:
        print('정수를 입력해주세요.')

print(f'정수 {number}를 입력받았습니다.')
```

<pre>
정수를 입력해주세요.
정수를 입력해주세요.
정수를 입력해주세요.
정수 10를 입력받았습니다.
</pre>
- 입력이 문자열이기 때문에 fancy하게 input을 처리할 수 있는 방법이 있다.


#### Q. 만약에 stdin으로 여러 개의 숫자가 들어오는 경우, 입력의 format을 알고 있다고 가정했을 때, 이를 효과적으로 처리할 수 있을까?



```python
# 이는 숫자를 2개로 가정한 경우
a, b = 30, 50 # unpacking
c =  30, 50   # packing
print(a,b,c)

a, b
a, b = input('숫자를 두 개를 입력하세요. e.g. 1,10').split(',')
print(a,b)
a = int(a)
b = int(b)
print(a + b)

# 숫자 여러개 받기
L = []
for i in input('숫자 여러개를 입력하세요. e.g. 1,10').split(','):
    L.append(int(i))

# Pythonic --> PEP8
[int(i) for i in input('숫자 여러개를 입력하세요. e.g. 1,10').split(',')]
```

<pre>
[1, 2, 3, 4, 5, 6, 7]
</pre>
### 2. File I/O


- file I/O란 프로그램에서 파일을 저장하고 불러오는 모든 것들을 의미합니다.



- file에는 txt, png, json, xlsx 등 여러가지 종류가 있습니다.



- 그 중에서 가장 간단하게 사용할 수 있는 데이터는 txt 파일입니다.


> 텍스트 파일을 여는 방법에는 read(), readline(), readlines(), for문을 이용한 방법이 있다. 코드를 통해 각 방법의 차이를 알아보자.



```python
#절대경로 :
#/Users/emphistaff/My Drive/Lecture Materials/...
'C:/Users/username/...'
'/Users/username/...'
#상대경로 :
#코드(프로그램)이 실행되고 있는 위치를 기준으로 한 파일의 경로.
'./'
```

#### 파일을 불러올 때 생기는 분노 포인트



1. 경로를 인식하지 못하는 경우



> 경로에 영어가 아닌 다른 글자(주로 한글)이 있는 경우에 인식하지 못하는 케이스가 있음.



> 경로가 진짜로 틀린 경우. e.g. '/' '\'



2. 파일 내부의 데이터가 **한글** 텍스트인 경우



> 기본적으로 텍스트를 'utf-8' 이라는 방식으로 인코딩함.



> 윈도우 + 한글 : cp949



```python
# f.read()를 통해 data 폴더안에 있는 test.txt를 read mode로 열어봅니다.
## read - r, (over)write - w, append - a
f = open('./test.txt', encoding='cp949')
print(f.read())   # txt 파일에 있는 모든 내용을 하나의 string으로 읽어옴
f.close()         # fileIOWrapper를 사용 후에 닫아주어야 함

with open('./test.txt', encoding='cp949') as f:
    print(f.read())
```

<pre>
아
휴
아이구
아이쿠
아이고
어
나
우리
저희
따라
의해
을
를
에
의
가
으로
로
에게
뿐이다
의거하여
근거하여
입각하여
기준으로
예하면
예를 들면
예를 들자면

</pre>

```python
# f.readline()를 통해 data 폴더안에 있는 test.txt를 read mode로 열어봅니다.
with open('./test.txt', encoding='cp949') as f:
    print(f.readline())
```

<pre>
아

</pre>

```python
# f.readlines()를 통해 data 폴더안에 있는 test.txt를 read mode로 열어봅니다.
with open('./test.txt', encoding='cp949') as f:
    print(f.readlines())  # 텍스트 파일내에서 '\n'을 기준으로 자른 리스트를 반환합니다.
```

<pre>
['아\n', '휴\n', '아이구\n', '아이쿠\n', '아이고\n', '어\n', '나\n', '우리\n', '저희\n', '따라\n', '의해\n', '을\n', '를\n', '에\n', '의\n', '가\n', '으로\n', '로\n', '에게\n', '뿐이다\n', '의거하여\n', '근거하여\n', '입각하여\n', '기준으로\n', '예하면\n', '예를 들면\n', '예를 들자면\n']
</pre>

```python
# for문을 통해 data 폴더안에 있는 test.txt를 read mode로 열어서 출력해봅니다.
with open('./test.txt', encoding='cp949') as f:
    for i in f:
        print(i, end='')
```

<pre>
아
휴
아이구
아이쿠
아이고
어
나
우리
저희
따라
의해
을
를
에
의
가
으로
로
에게
뿐이다
의거하여
근거하여
입각하여
기준으로
예하면
예를 들면
예를 들자면
</pre>
#### Q. test.txt를 열어서 한글자짜리를 다 지우고 다시 저장하고 싶다. 어떻게 해야할까?



```python
# test.txt를 read mode로 열고 할 일이 끝나면 자동으로 닫는다.

# 1)
with open('./test.txt', 'r', encoding='cp949') as f:
    # output = [line for line in f]
    output = []
    for line in f:
        line = line.strip('\n')
        if len(line) >= 2:
            output.append(line)
        
output

# 2)
# with open('./test.txt', encoding='cp949') as f:
#     for i in f:
#         i = i.strip('\n')
#         if len(i) >= 2:
#             print(i)

# 두글자 이상인 텍스트만 output list에 저장한다.


# result.txt로 output list에 있는 내용을 저장하기 위해 write mode로 열었다.
# 기존에 저장된 파일을 완전히 덮어쓰는게 아니고 추가하고 싶은거라면 'a' (append mode)를 사용한다.
with open('result.txt', 'w') as f:
    for line in output:
        # f.write()    # f.write()는 주어진 string 자체를 그대로 저장함
        print(line, file = f)   # stdout을 fileout으로 변경
```


```python
# 제대로 데이터가 저장되어 있는지, 불러와서 확인한다.
with open('./result.txt', 'r') as f:
    output = [line for line in f]
output
```

<pre>
['아이구\n',
 '아이쿠\n',
 '아이고\n',
 '우리\n',
 '저희\n',
 '따라\n',
 '의해\n',
 '으로\n',
 '에게\n',
 '뿐이다\n',
 '의거하여\n',
 '근거하여\n',
 '입각하여\n',
 '기준으로\n',
 '예하면\n',
 '예를 들면\n',
 '예를 들자면\n']
</pre>
### (OPTIONAL) pickle 라이브러리를 통해서 파이썬 object 자체를 저장하기



```python
output
```

<pre>
['아이구\n',
 '아이쿠\n',
 '아이고\n',
 '우리\n',
 '저희\n',
 '따라\n',
 '의해\n',
 '으로\n',
 '에게\n',
 '뿐이다\n',
 '의거하여\n',
 '근거하여\n',
 '입각하여\n',
 '기준으로\n',
 '예하면\n',
 '예를 들면\n',
 '예를 들자면\n']
</pre>

```python
# serialization : 어떤 데이터를 binary image로 바꿔주는 방식
## 단, 대용량 데이터에 대해서는 적합하지 않음
## 대용량 데이터의 경우에는 .txt / .csv / .json 형식이 훨씬 효율적이다.
import pickle

with open("result.pk", 'wb') as f: # write as binary
    pickle.dump(output, f)
```


```python
with open("result.pk", 'rb') as f:
    output2 = pickle.load(f)
    
output2
```

<pre>
['아이구\n',
 '아이쿠\n',
 '아이고\n',
 '우리\n',
 '저희\n',
 '따라\n',
 '의해\n',
 '으로\n',
 '에게\n',
 '뿐이다\n',
 '의거하여\n',
 '근거하여\n',
 '입각하여\n',
 '기준으로\n',
 '예하면\n',
 '예를 들면\n',
 '예를 들자면\n']
</pre>