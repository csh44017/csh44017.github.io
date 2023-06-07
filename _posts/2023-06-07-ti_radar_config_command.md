---
title:  "TI radar Configuration Command 정리"
excerpt: "TI 레이더의 cli command 사용법 정리"

categories:
  - Radar
tags:
  - [Radar, Configuration, Command]

toc: true
toc_sticky: true

date: 2023-06-07
last_modified_at: 2023-06-08
---

## TI radar Configuration Command 정리  

### Configuration Command  
TI의 레이더 소스코드를 보면 사용자가 환경에 따라 변경할만한 파라메타들을 튜닝해서 사용할 수 있도록 cli를 통해 레이더의 동작에 관련된 설정들을 변경할 수 있도록 구현해 놓았다.  

TeraTerm 같은 시리얼 통신 프로그램을 통해서 **통신 포트(Enhanced COM Port)**와 **통신 속도(921600)**를 지정하여 포트를 오픈한 후 직접 명령어를 입력할 수도 있지만, 
맞춰놓았던 설정 값들을 cfg파일에 필요한 command line을 작성해놓고 uart 통신으로 각각의 라인들을 차례대로 입력하여 사용하면 편리하게 사용할 수 있다.  
- UART 통신으로 command 입력 및 데이터 수신  
  ```python  
  import time
  import serial  # pyserial


  class Receiver:
      def __init__(self):
          self.enhanced_port = "COM6"
          self.standard_port = "COM5"
          self.cfg_path = "./people_counting.cfg"

      # 통신 포트 연결
      def is_ready(self):
          try:
              self.uart_com = serial.Serial(port=self.enhanced_port, baudrate=115200, timeout=0)
              self.data_com = serial.Serial(port=self.standard_port, baudrate=921600, timeout=0)
              return True
          except Exception as e:
              print(e, "\nPlease check serial port")
              return False

      def config(self):
          try:
              with open(self.cfg_path) as f:
                  lines = f.readlines()
                  for line in lines:
                      uart_com.write(line.encode())
                      ack = uart_com.readline().decode()
                      print(ack)
                      time.sleep(0.1)
                      ack = uart_com.readline().decode()
                      print(ack)
                      time.sleep(0.1)
              self.uart_com.reset_input_buffer()
              time.sleep(0.3)
          except:
              self.uart_com.close()
              self.data_com.close()
  

  if __name__ == "__main__":
      rx = Receiver()
      if rx.is_ready():
            rx.config()
  ```  
단순히 센서에 전원을 인가하는 것만으로는 RF가 동작하지 않고 cli를 통해 sensor를 start 시켜주어야 센서 데이터를 보내기 시작한다.  

