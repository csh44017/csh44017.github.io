---
title:  "Pythonic Code"
excerpt: "Pythonic 코드 작성 팁"

categories:
  - ML
tags:
  - [Python]

toc: true
toc_sticky: true

date: 2022-12-21
last_modified_at: 2023-10-13
---

## Pythonic 코드  
**파이썬스러운 코드**로, 
Python 언어의 규칙을 준수하면서 가독성이 좋고 클린한 코드를 작성하여 파이썬 생태계와 잘 통합될 수 있도록 한다.  

Pythonic code를 작성하기 위해 참고할 만한 것들과 헷갈릴 수 있는 부분을 정리해보았다.  
<br><br>  


### 데이터 타입  
동적 타이핑 언어인 파이썬에서는 변수의 데이터 타입이 실행 중에 자동으로 결정되기 때문에 변수를 선언할 때 데이터 타입을 지정해주지 않아도 된다.  
- 숫자 (Number)  
  - int  
  - float  
  - complex  
- 문자열 (String)  
  - str  
- 불리언 (Boolean)  
  - bool  
- 시퀀스 (Sequence)  
  - list  
  - tuple  
  - str  
  - range  
- 매핑 (Mapping)  
  - dict
- 집합 (Set)  
  - set  
  - frozenset  
- 바이트 시퀀스 (Byte Sequence)  
  - bytes  
  - bytearray  
- None  
  - NoneType  
<br><br>  


### 데이터 구조  
- 시퀀스 (Sequence)  
  **순서가 있는 데이터 구조**  
  각 요소가 인덱스에 따라 정렬되므로 **인덱스를 통해 개별 요소에 접근**할 수 있으며 슬라이싱 기능을 사용할 수 있다.  

  ex) 리스트(List), 튜플(Tuple), 문자열(String), 범위(range), 바이트(bytes), 바이트어레이(bytearray)  
  ```python
  # list
  numbers = [1, 2, 3, 4, 5]
  print(numbers[2])
  print(numbers[1:3])

  # tuple
  age = (11, 19, 37)
  print(age[1])

  # string
  text = 'bear'
  print(text[0])
  ```
  ```shell
  3
  [2, 3]

  19

  'b'
  ```
- 컨테이너 (Container)  
  **여러 요소나 객체를 저장하는 데이터 구조**  
  **순서에 상관없이 요소를 보관**하므로 **인덱스로 접근이 불가능**하지만, 반복 작업이나 검색, 추가, 삭제와 같은 기능을 수행할 수 있다.  

  ex) 리스트(List), 세트(Set), 딕셔너리(Dictionary), 큐(Queue)  
  (리스트는 시퀀스와 컨테이너 모두에 해당)  
  ```python
  # list
  numbers = [1, 2, 3, 4, 5]
  print(numbers*2)

  # set
  my_set = {1, 2, 3, 3, 4}
  print("중복 요소 제거 :", my_set)
  set1 = {1, 2, 3}
  set2 = {3, 4, 5}
  print('합집합 :', set1 | set2)  # Union
  print('교집합 :', set1 & set2)  # Intersection
  print('차집합 :', set1 - set2)  # Difference

  # dictionary
  animals = ['bear', 'dog', 'cat', 'bear', 'bear']
  key = list(set(animals))  # ['bear', 'dog', 'cat']

  animal_dict = {'bear': 1, 'dog': 2, 'cat':3}
  classes = [animal_dict[k] for k in key]
  print(classes)

  # queue
  import queue

  my_queue = queue.Queue()
  my_queue.put(5)
  my_queue.put(7)
  my_queue.put(9)

  while not my_queue.empty():
      item = my_queue.get()
      print(item)
  ```
  ```shell
  [1, 2, 3, 4, 5, 1, 2, 3, 4, 5]

  중복 요소 제거 : {1, 2, 3, 4}
  합집합 : {1, 2, 3, 4, 5}
  교집합 : {3}
  차집합 : {1, 2}

  [1, 2, 3]

  5
  7
  9
  ```
<br>  

#### 'list'와 'numpy.ndarray' 간에 주의힐 점  
- +연산  
  - list : 리스트 추가 or 요소와 숫자의 덧셈  
  - ndarray : 전체 요소에 각각 덧셈 연산 or 각 차원끼리 덧셈  
  ```python
  import numpy as np

  # list
  my_list = [[1, 2, 3], [4, 5, 6]]
  print(my_list + [[1, 2]])  # 연산할 리스트의 크기는 고려하지 않음

  # numpy.ndarray
  matrix = np.array([[1, 2, 3], [4, 5, 6]])
  print(matrix + 1)
  print(matrix + matrix)  # 연산할 행렬의 크기가 동일해야 함
  ```
  ```shell
  [[1, 2, 3], [4, 5, 6], [1, 2]]

  array([[2, 3, 4],
         [5, 6, 7]])
  array([[ 2,  4,  6],
         [ 8, 10, 12]])
  ```
- *연산  
  - list : 리스트 자체를 반복  
  - ndarray : 전체 요소에 각각 곱셈 연산  
  ```python
  import numpy as np

  # list
  my_list = [[1, 2, 3], [4, 5, 6]]
  print(my_list * 2)

  # numpy.ndarray
  matrix = np.array([[1, 2, 3], [4, 5, 6]])
  print(matrix * 2))
  print(matrix * matrix))  # 연산할 행렬의 크기가 동일해야 함
  ```
  ```shell
  [[1, 2, 3], [4, 5, 6], [1, 2, 3], [4, 5, 6]]

  array([[ 2,  4,  6],
         [ 8, 10, 12]])
  array([[ 1,  4,  9],
         [16, 25, 36]])
  ```
<br><br>  


### 데이터의 크기 조회  
- .shape  
  1차원 구조이거나 key-value 쌍으로 구성되는 데이터는 shape의 개념이 없음  
  --> (행, 열) tuple 반환  
- .size  
  전체 요소의 개수가 데이터의 길이와 같은 경우는 len으로 확인  
  --> '행x열'의 전체 요소 개수 int 반환  
- len()  
  데이터의 길이 확인 가능  
  --> 데이터의 길이 int 반환  

  |      |Tuple|List|ndarray|Set|Dictionary|Dataframe|  
  |------|-----|----|-------|---|----------|---------|  
  |.shape|X    |X   |O      |X  |X         |O        |
  |.size |X    |X   |O      |X  |X         |O        |
  |len() |O    |O   |O      |O  |O         |O        |
<br><br>  


### 데이터 처리  
- map()  
  주어진 함수를 iterable 객체의 모든 요소에 적용  
  - 사용법  
    ```python
    """ map(각 요소에 적용할 함수, 함수를 적용할 반복가능한 객체) """
    map(function, iterable)
    ```
  - 결과  
    ```shell
    <map object at 0x000001BF8666AB80>
    ```
- lambda  
  **함수의 이름 없이 정의**되는 익명 함수로, **한 줄로 간단한 함수를 정의**할 때 사용한다.  
  - 사용법  
    ```python
    """ lambda 함수에 전달할 매개변수: 함수에서 반환할 값 """
    lambda arguments: expression
    ```
  - 결과  
    ```shell
    <function <lambda> at 0x000001BF866FEE50>
    ```

  ```python
  numbers = [1, 2, 3, 4, 5]

  """
  @ lambda : 수행할 연산 정의
  @ map    : 반복 가능한 객체의 모든 요소에 함수 적용
  """
  squared_numbers = list(map(lambda x: x**2, numbers))  # 각 요소의 제곱
  print(squared_numbers)
  ```
  ```shell
  [1, 4, 9, 16, 25]
  ```

- enumerate()  
  ```python
  animals = ['dog', 'cat', 'bear', 'tiger']
  for i, animal in enumerate(animals):
      print(f'[{i}] {animal}')
  ```

- with  
파일을 열였을 때 할당된 리소스를 모두 해제해주어야 하는데,  
이 과정에서 예외나 오류가 발생하더라도 컨텍스트가 종료되므로 안전하게 실행할 수 있다.  
  ```python
  with open('log.txt', 'w') as log:
      log.write('success')
  ```
  ```python
  import json

  with open("./package.json") as f:
      cfg = json.load(f)
  ```
<br><br>  


### import  
- 스크립트 (Script)  
  파이썬 코드가 실행될 때 사용되는 파일  
- 모듈 (Module)  
  재사용하기 위한 함수, 클래스, 변수를 정의한 코드를 저장한 파일  
- 패키지 (Package)  
  모듈을 그룹으로 묶어서 구조화하는 디렉토리  
  패키지에서는 여러 가지 하위 모듈을 포함할 수 있고, 모듈과 모듈 안의 함수를__init__.py 를 이용해 한번에 가지고 올 수 있다.  
  - [패키지 import 응용](https://dojang.io/mod/page/view.php?id=2450){:target="_blank"}  
  - [Numpy의 \_\_init__.py 코드](https://github.com/numpy/numpy/blob/main/numpy/__init__.py){:target="_blank"}  
- 라이브러리 (Library)  
  여러 모듈과 패키지의 집합  
<br>  

#### 하위 폴더 import
- 폴더 구조  
  ```shell
  project/
  ├── main.py
  └── dir1/
      └── dir2/
          └── module.py
  ```
  (main.py 에서 실행)  

  ```python
  import dir1.dir2.module
  ```
<br>  

#### 상위 폴더 import  
- 폴더 구조  
  ```shell
  /home/
   └── ubuntu/
       └── my_project/
           ├── dir1/
           │   ├── math.py
           │   └── dir2/
           │       └── dir3/
           │           └── main.py
           └── module.py
  ```
  (main.py 에서 실행)  

  - sys.path 에 절대경로 추가  
    ```python
    import sys
    import os
    current_dir = os.path.dirname(__file__)
    project_dir = os.path.abspath(os.path.join(current_dir, '../../../'))
    sys.path.append(project_dir)

    from module import my_function
    from dir1.math import add_function
    ```
    모듈이나 패키지를 검색할 경로를 직접 추가해준다.  

  - 상대 경로  
    ```python
    from ...module import my_function
    from ..math import add_function
    ```
    ```python
    from ...module import *
    from ...dir1.math import add_function
    ```
    상대 경로는 모듈 내부에 대해서만 사용이 가능하고  
    relative import를 할 때 현재 실행시키는 스크립트의 위치를 알지 못하여  
    상위 디렉토리에 있는 모듈이나 패키지는 검색 경로가 파악되지 않기 때문에  
    ```shell
    ImportError: attempted relative import with no known parent package
    ```
    이라는 오류가 발생할 수 있다.  
    
    이를 해결하기 위해 모듈을 스크립트처럼 실행하도록 하는 -m 옵션을 사용하면 정상적으로 동작한다.  
    ```bash
    python3 -m my_project.dir1.dir2.dir3.main
    ```
    이 때 주의할 점은 최상위 디렉토리에서 실행해야 상대 경로로 import할 모듈들의 폴더의 구조를 모두 파악 가능하므로,  
    경로를 구조 파악에 필요한 최상위 폴더부터 작성하여 입력해야한다.  
<br>  







<br><br>  
<br><br>  
<br><br>  

### Asterisk (*)  
파이썬에서 코드를 타고 들어가다 보면 *args, **kwargs 와 같은 인자를 볼 수 있다.  
이는 C언어의 포인터( * )와 헷갈릴 수 있지만 다른 개념이다.  

애스터리스크는 여러 요소를 묶어서 하나의 시퀀스로 만들거나,  
리스트, 튜플, 문자열 등의 시퀀스를 unpacking해서 여러 개의 요소로 분해할 수 있다.  

### *  
- 연산  
  - 곱셈  
  - 거듭제곱  
  - ndarray 행렬 곱셈  
- 리스트 반복 확장  
- 가변인자 사용  
- 컨테이너 타입 Unpacking  

https://mingrammer.com/understanding-the-asterisk-of-python/

```python
# 언패킹
numbers = [1, 2, 3, 4, 5]
a, b, *rest = numbers
print(a)  # 1
print(b)  # 2
print(rest)  # [3, 4, 5]

a, b, c, d, *e = numbers
print(e)  # [5]

# 묶기
numbers = [1, 2, 3]
new_list = [*numbers, 4, 5]
print(new_list)  # [1, 2, 3, 4, 5]
```

### **  

```python
# 딕셔너리 언패킹
params = {'param1': 1, 'param2': 2}
my_function(**params)  # my_function(param1=1, param2=2)

# 키워드 인자 전달
def greet(name, age):
    print(f"Hello, {name}! You are {age} years old.")

person_info = {'name': 'Alice', 'age': 30}
greet(**person_info)  # Hello, Alice! You are 30 years old.
```
<br><br>  


## 상속  
```python
super().__init__()
```
<br><br>  


## 데코레이터  
함수를 파라미터로 받아서 함수를 반환하는 함수  


<br><br>  

## 디스크립터  


<br><br>  

## 제너레이터  


<br><br>  

## 단위 테스트  
unittest  