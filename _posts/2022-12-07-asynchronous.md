---
title:  "Asynchronous"
excerpt: "FastAPI를 위한 비동기 이해하기"

categories:
  - ML
tags:
  - [Asynchronous]

toc: true
toc_sticky: true

published: true

date: 2022-12-07
last_modified_at: 2023-06-01
---

### **동시성** (Concurrency)  
여러 작업이 **실제로 동시에 수행되는 것은 아니지만**,  
**하나의 작업이 대기 상태일 때 다른 작업으로 전환하면서 조금씩 번갈아 수행**하는 방식으로 마치 여러 개가 동시에 처리되는 것처럼 보인다.  

제한된 리소스(CPU 코어)를 효율적으로 활용하고 대기 시간을 최소화할 수 있기 때문에 **시스템의 응답성을 높일 수 있다**.  
<br>  

- 멀티 프로세싱 (Multiprocessing)  
  여러 프로세스를 동시에 실행하여 작업을 병렬로 처리  

  각 프로세스들은 독립적인 메모리 공간을 가지기 때문에 하나의 프로세스에서 발생한 오류가 다른 프로세스에 영향을 미치지 않는다.  
  하지만, CPU와 메모리 리소스가 많이 필요하고 프로세스 간 통신이 복잡하다는 단점이 있다.  
  ```python
  import multiprocessing
  ```

- 멀티 스레딩 (Multithreading)  
  단일 프로세스 내에서 여러 스레드를 동시에 실행  

  각 스레드들은 부모 프로세스의 메모리를 공유해서 사용하기 때문에 

  ```python
  import threading
  ```

- 비동기 처리 (Asynchronous Programming)  
  단일 스레드에서 비동기로 여러 작업을 번갈아가면서 수행  

  I/O 바운드 작업에 유리  
  - I/O 작업의 예시  
    - 파일 read/write  
    - 네트워크 데이터 송수신  
    - 데이터베이스 작업  
    - UI 동작  
    - 주변 기기와 상호 작용, 센서 데이터 수신  
    - 프로세스 간 통신, 메모리 공유  
    - 오디오, 비디오 스트림  
  ```python
  import asyncio
  ```

- GIL (Global Interpreter Lock)  

<br>  

### 병렬성 (Parallelism)  
여러 처리 장치가 독립적으로 각각 다른 작업을 맡아 동시에 여러 작업을 수행하는 것  

대규모 작업의 처리 속도를 크게 단축할 수 있다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/f5222e22-8a6f-4f98-81ec-1f3d64375e7b" width="500" height="200">  
</div>
<br><br>  

- 코어 : 물리적 코어 (하드웨어)  
- 스레드 : 논리적 코어 (운영체제)  

  Hyper-Threading 기술을 이용하면 1개의 코어를 2개의 스레드로 나누어 두 가지 작업을 동시에 처리할 수 있다.  
<br>  



### 동기 (Synchronous)  

<br>  

### 비동기 (Asynchronous)  


<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/50fc0f33-d037-4edf-ae53-10c3f148ce2e" width="500" height="280">  
</div>
<br><br>  



### Blocking  
https://velog.io/@sb-jo/asyncio-%EC%8B%9C%EB%A6%AC%EC%A6%881-%EB%B9%84%EB%8F%99%EA%B8%B0#%EB%B8%94%EB%9D%BD---%EB%85%BC%EB%B8%94%EB%9D%BD  

<br>  

### Non-Blocking  

<br><br>  



### CPU-Bound  
https://ivdevlog.tistory.com/3  

<br>  

### I/O-Bound  

<br><br>  



### threading  

<br>  

### asyncio  
싱글 스레드 내에서 동시성을 구현하는 비동기 프로그래밍을 지원  
<br>  

#### 코루틴 (Coroutine)  
비동기 작업  
실행 중간에 중지했다가 나중에 해당 지점부터 다시 시작 가능  
이벤트 루프와 함께 사용되어 효율적인 비동기 프로그래밍 가능  
일반적인 서브루틴(함수, 메소드)과 다르게 작동  
<br>  

#### 이벤트 루프  
여러 코루틴의 실행을 관리하며, 어떤 코루틴이 실행될지 결정  
<br>  

#### 비동기 함수 실행 방법  
- await  
현재 실행 중인 비동기 함수 내에서 await 뒤의 함수를 호출하고,  
호출한 함수의 실행이 완료될 때까지 현재 함수의 실행을 일시 중지하고 다른 코드로의 이동을 차단한다.  
(코루틴 내에서 함수 실행의 흐름을 대기시키므로 동기적인 )

- asyncio.create_task()  
현재 실행중인 이벤트 루프에서 호출한 함수를 별도의 새로운 비동기 태스크로 추가하여 스케줄링한다.  
호출된 함수는 백그라운드에서 동시성을 가지고 실행되기 때문에 완료 여부와 상관없이 즉시 다음 코드로 넘어간다.  
(다음 코드는 동기 코드일 수도 있고 비동기 코드일 수도 있음)  

- asyncio.run()  
새로운 이벤트 루프에서 함수를 비동기적으로 실행하고 완료될 때까지 기다린 후 종료한다.  
이미 실행중인 이벤트 루프에서는 사용할 수 없기 때문에 주로 스크립트의 main point에서 비동기 함수를 호출할 때 사용  

```python
async def main():
    asyncio.create_task(async_function())  # 비동기 함수를 백그라운드에서 실행
    print("이 코드는 즉시 실행됩니다.")

asyncio.run(main())
```


<br>  

#### 몇 가지 예시 코드  
```python
import asyncio
from datetime import datetime


async def time_delay(time):
    await asyncio.sleep(time)
    print(f'{time}\t\t{datetime.now()}')


async def main():
    asyncio.create_task(time_delay(1))
    asyncio.create_task(time_delay(2))
    print(f'\n[End]\t{datetime.now()}')


print('[Start]', datetime.now(), '\n')
asyncio.run(main())
```
```text
[Start] 2023-12-04 13:15:18.258738 


[End]	2023-12-04 13:15:18.259738
```
이렇게 작성하면 새로 생성한 비동기 태스크의 생명주기와 독립적으로 메인 코루틴이 종료되면서 백그라운드 태스크들이 완료되기 전에 프로그램이 종료되어 아무것도 출력되지 않는다.  
<br>  

```python
import asyncio
from datetime import datetime


async def time_delay(time):
    await asyncio.sleep(time)
    print(f'{time}\t\t{datetime.now()}')


async def main():
    asyncio.create_task(time_delay(1))
    asyncio.create_task(time_delay(2))
    await asyncio.sleep(5)
    print(f'\n[End]\t{datetime.now()}')


print('[Start]', datetime.now(), '\n')
asyncio.run(main())
```
```text
[Start] 2023-12-04 13:16:52.101797 

1		2023-12-04 13:16:53.116033
2		2023-12-04 13:16:54.121275

[End]	2023-12-04 13:16:57.119279
```
따라서 백그라운드 태스크들이 완료될 수 있도록 충분한 시간 동안 실행 상태를 유지하면 출력 결과를 볼 수 있다.  
첫번째 태스크를 생성하고 곧바로 두번째 태스크를 생성하였으므로 실행 결과에서는 약 1초 만큼의 차이가 존재하고 총 2초의 실행 시간이 걸린다.  
<br>  

```python
import asyncio
from datetime import datetime


async def time_delay(time):
    await asyncio.sleep(time)
    print(f'{time}\t\t{datetime.now()}')


async def main():
    await time_delay(1)
    await time_delay(2)
    print(f'\n[End]\t{datetime.now()}')


print('[Start]', datetime.now(), '\n')
asyncio.run(main())

```
```text
[Start] 2023-12-04 13:19:52.768755 

1		2023-12-04 13:19:53.772756
2		2023-12-04 13:19:55.785480

[End]	2023-12-04 13:19:55.785480
```
아니면 await로 해당 호출 함수가 완료될 때까지 다음으로 넘어가지 못하게 해도 출력 결과를 받아볼 수 있다.  
하지만 다음 코드로 넘어가지 못해 동기적인 동작을 하므로 1초와 2초씩 차이가 존재하고 총 3초의 실행 시간이 걸린다.  
<br>  

```python
import asyncio
import time
from datetime import datetime


async def time_delay(time):
    await asyncio.sleep(time)
    print(f'{time}\t\t{datetime.now()}')


async def main():
    asyncio.create_task(time_delay(1))
    asyncio.create_task(time_delay(2))
    time.sleep(5)
    print(f'\n[End]\t{datetime.now()}')


print('[Start]', datetime.now(), '\n')
asyncio.run(main())

```
```text
[Start] 2023-12-04 13:21:51.115174 


[End]	2023-12-04 13:21:56.127794
```
만약 aysncio.sleep()을 사용하여 비동기적으로 대기하지 않고 time.sleep()을 사용하여 동기적으로 대기하면, 현재 스레드가 차단되어 이벤트 루프의 실행이 중단된다.  
결과적으로 시작한 비동기 태스크가 완료될 때까지 대기하는 것이 아니라 asyncio의 이벤트 루프와 비동기 작업의 실행까지도 막았다가 5초 후에 바로 프로그램이 종료되어 버린다.  
(asyncio 모듈을 동기 함수와 섞어서 사용하는 것은 좋지 않다.)  
