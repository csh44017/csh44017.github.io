---
title:  "Bytes 파싱 참고 노트"
excerpt: "바이트 데이터 파싱을 위한 참고 노트"

categories:
  - Radar
tags:
  - [Parsing, Bytes]

toc: true
toc_sticky: true

published: false

date: 2022-06-18
last_modified_at: 2023-06-08
---

## Bytes Parsing  
파이썬으로 read한 센서 데이터의 바이트 값을 패킷에 따라 파싱하기 전에 데이터가 정상적으로 들어오는지 print해서 확인해보는 과정에서 출력 결과가 1byte 마다 '\x'로 구분되어 출력되는 것이 아니라 불규칙하게 특수문자들이 포함되어 있는 것을 보았다.  

어떤 규칙으로 표현되는 것인지를 보면 다음과 같다.  
<br>  

### 1. 바이트 리터럴  
파이썬에서 바이트 리터럴은 'b' 접두사를 사용하여 정의된 바이트 시퀀스이다.  
문자열과 비슷하게 보이지만 내부적으로는 **각 문자가 8bit 바이트로 표현되는 바이트 값**으로 인식된다.  
<br>  

### 2. 바이트 리터럴이 필요한 경우  
- 문자열 인코딩  
  **UTF-8과 같이 특정 인코딩을 사용하여 문자열을 바이트로 변환해야 할 때**, 바이트 리터럴을 사용  

- 이진 데이터를 다룰 때  
  파일 입출력, 네트워크 통신 등에서 이진 데이터를 다루는 경우에는 바이트 리터럴을 사용합니다. 이는 **데이터를 원시적인 바이트 형태로 다루어야 할 때** 사용  
<br>  

### 3. 바이트 리터럴 특수문자 해석 예시  
- b'\x04?'  
  2바이트로 이루어진 값으로, 첫 번째 바이트는 **0x04**이고 두 번째 바이트는 **0x3F(물음표)**이다.  

- b'\x00Ch'  
  - b'\x00' : 1바이트로 이루어진 값으로, 16진수로 표현하면 **0x00**이고 16진수의 0x00은 10진수로 0이 된다.  

  - b'C' : 1바이트로 이루어진 값으로, 16진수로 표현하면 **0x43**이고 16진수의 0x43은 10진수로 67이 된다. (ASCII 코드에서 'C'는 67)  

  - b'h' : 1바이트로 이루어진 값으로, 16진수로 표현하면 **0x68**이고 16진수의 0x68은 10진수로 104가 된다. (ASCII 코드에서 'h'는 104)  

  따라서 각각을 10진수로 변환하면 [0, 67, 104]가 된다.  

- b'\xff\x05a\x07\x12\x07\xe3\x05v\x05'  
  ```python  
  data = b'\xff\x05a\x07\x12\x07\xe3\x05v\x05'
  
  print(data[0], data[1], data[2], data[3], data[4], data[5], data[6], data[7], data[8], data[9])
  ```  
  ```shell  
  255 5 97 7 18 7 227 5 118 5
  ```  
<br>  

### 4. bytes와 string 간의 변환  
- bytes -> string  
  **hex()** 함수 사용  
  ```python  
  data = b'\xff\x05a\x07\x12\x07\xe3\x05v\x05'
  print(data.hex())  # string
  ```  
  ```shell  
  ff0561071207e3057605
  ```  
  bytes에서 string으로 변환하면 length는 2배가 된다.  

- string -> bytes  
  **bytes.fromhex()** 함수 사용  
  ```python  
  data = bytes([x for x in range(100)])
  data_str = data.hex()
  print(data_str)  # string

  print(bytes.fromhex(data_str))  # bytes
  ```  
  ```shell  
  000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f202122232425262728292a2b2c2d2e2f303132333435363738393a3b3c3d3e3f404142434445464748494a4b4c4d4e4f505152535455565758595a5b5c5d5e5f60616263
  ```  
  ```shell  
  b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\\]^_`abc'
  ```  
<br>  

### 기타  
for 문을 사용해 string을 bytes로 변환하는 함수를 만들어보았으나 내장함수를 사용하는 것이 압도적으로 빨랐다...  
(함수를 작성할 때에도 결국 다른 일부 기능의 내장함수들을 돌려 사용해 구현한 것 같은 느낌)  
<br>
fromhex() 함수를 타고 들어가보면 builtins.py에 다음과 같이 작성되어 있다.  
```python  
@classmethod # known case
def fromhex(cls, *args, **kwargs): # real signature unknown; NOTE: unreliably restored from __doc__ 
    """
    Create a bytes object from a string of hexadecimal numbers.
        
    Spaces between two numbers are accepted.
    Example: bytes.fromhex('B9 01EF') -> b'\\xb9\\x01\\xef'.
    """
    pass
```  
하위 수준의 언어로 구현되어 있어 해당 기능을 어떻게 구현했는지 바로는 확인해보지 못하는 것 같다.  
<br>
코드도 간결해지고 속도도 빨라지는 것을 보고 무조건 함수를 만들어 사용하는 것보다는 적절한 내장함수를 선택하여 사용하는 것이 효율적이라는 것을 깨달았다. 코드 리팩토링을 할 때 불필요한 코드들을 줄여나가야겠다.  
