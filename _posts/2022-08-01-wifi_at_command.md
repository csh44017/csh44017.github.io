---
title:  "Wi-Fi 기반 소켓 통신"
excerpt: "ESP-12F 모듈을 이용해 AT command로 서버와 소켓 통신"

categories:
  - Radar
tags:
  - [Wi-Fi, Socket, AT-command]

toc: true
toc_sticky: true

date: 2022-08-01
last_modified_at: 2023-11-01
---


## Wi-Fi 기반 소켓 통신  
- 참고  
  [ESP-12F 개발환경 설정하기](https://m.blog.naver.com/just4u78/221069655001){:target="_blank"}  
  [ESP-12F 연결 테스트](https://www.instructables.com/ESP-12F-ESP8266-Module-Connection-Test/){:target="_blank"}  
  [서버/클라이언트 TCP 소켓 통신](https://uknowblog.tistory.com/256){:target="_blank"}  
<br>  

### Arduino IDE로 시리얼 통신하여 ESP-12F 모듈에 코드 업로드  
사용할 와이파이 네트워크의 연결을 위해 SSID와 PW를 지정하는 코드를 업로드해야 한다.  

1. Arduino IDE 설치하기  
  [아두이노 공식 사이트](https://www.arduino.cc/en/software){:target="_blank"}를 이용  
<br>  

2. ESP8266 보드 지원 설치  
  File > Preferences 에서 'Additional boards manager URLs'에 url을 추가  
    ```shell
    http://arduino.esp8266.com/stable/package_esp8266com_index.json
    ```
<br>  

3. ESP8266 보드 패키지 설치  
  Tools > Boards > Boards Manager 에서 'ESP8266'을 검색 후 설치  
<br>  

4. 보드 선택  
  Tools > Boards > esp8266 > NodeMCU 0.9 (ESP-12 Module) 선택  
<br>  

5. 연결할 Wi-Fi 네트워크의 자격 증명 설정  
  연결할 와이파이의 이름(ssid)과 비밀번호(password)를 지정해주면 된다.  
  (노트북은 상관없지만 ESP-12F는 5G 와이파이를 찾지 못하기 때문에 2.4G 와이파이를 지정)  
  <br>
  serverIP는 같은 공유기의 와이파이를 사용하여 로컬 환경에서만 통신할 것이므로 노트북의 내부 IP를 사용  
  serverPort는 노트북에서 수신받기 위해 지정한 포트를 동일하게 지정  
    - 코드 작성  
      ```c++
      #include <ESP8266WiFi.h>


      /* Wi-Fi config */
      const char* ssid = "iptime";
      const char* password = "admin";

      /* host server config */
      const char* serverIP = "192.168.0.40";
      const int serverPort = 12345;

      int num = 0;


      void setup() {
        /* serial communication init */
        Serial.begin(115200);  // baudrate
        delay(10);

        /* Wi-Fi network scan */
        int n = WiFi.scanNetworks();
        Serial.print(" Wi-Fi scan result :  ");
        if (n == 0) {
          Serial.println("network not found\n");
        } else {
          Serial.print(n);
          Serial.println(" networks found\n");
        }

        for (int i = 0; i < n; i++) {
          Serial.print("[Wi-Fi ");
          Serial.print(i+1);  // network index
          Serial.print("]  ");
          Serial.print(WiFi.SSID(i));  // Service Set Identifier
          Serial.print("\t\t(");
          Serial.print(WiFi.RSSI(i)); // Received Signal Strength Indicator
          Serial.print(" dbm)");
          Serial.println((WiFi.encryptionType(i) == ENC_TYPE_NONE)?" ":"   * encryption *");  // encryption
          delay(10);
        }
        Serial.println();

        /* Wi-Fi network connect */
        WiFi.begin(ssid, password);

        while (WiFi.status() != WL_CONNECTED) {
          delay(500);
          Serial.print("Connecting to '");
          Serial.print(ssid);
          Serial.print("' [status : ");
          Serial.print(WiFi.status());  // connect status
          Serial.println("]");
        }
        Serial.println("\nWi-Fi connected.\n");
      }


      void loop() {
        /* TCP client tries to connect */
        WiFiClient client;

        if (!client.connect(serverIP, serverPort)) {  // connect to server
          Serial.println("Failed to connect to server.");
          delay(500);
        }

        /* send socket */
        client.println(num++);  // value
        delay(1000);
      }
      ```
<br>  

6. ESP-12F와 CP2102로 핀 연결  
  프로그래밍 모드로 핀을 연결 후 컴퓨터의 USB포트로 시리얼 통신  
    - 핀 연결  
      |ESP-12F    |CP2102                                               |  
      |-----------|---------------------------------------------------- |  
      |VCC        |3.3V<br>(TX, RX 통신 시 5V를 인가하면 고장날 수 있음) |  
      |GND        |GND                                                  |  
      |EN (CH_PD) |3.3V                                                 |  
      |GPIO0      |3.3V (동작 모드)<br>**GND (프로그래밍 모드)**         |  
      |GPIO2      |3.3V                                                 |  
      |GPIO15     |GND                                                  |  
      |TX         |RX                                                   |  
      |RX         |TX                                                   |  

    <div align="center">  
    <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/1e25f5bc-5b2d-47d2-a807-8296744d9266" width="500" height="115">  
    </div>  
    <div align="center">  
    <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/8e7be8fc-957c-46c3-9287-e66040f37f59" width="500" height="300">  
    </div><br>  

7. 포트 설정  
  Tools > Port 에서 CP2102가 잡힌 COM 포트 선택  
<br>  

8. 스케치 업로드  
  GPIO0를 low로 놓고, Arduino IDE로 작성한 프로그램 코드를 ESP-12F 모듈에 업로드  
    <div align="center">  
    <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/a6f802c2-dd3d-4e25-889e-ae030e98566d" width="500" height="350">  
    </div><br>  

9. 연결 확인  
  ESP-12F의 'GPIO0'를 GND에서 3.3V로 바꾼 후,  
  Arduino IDE의 우측 상단에 돋보기 아이콘을 클릭해 'Serial Monitor'를 열고  
  'Both NL & CR'과 115200의 baudrate으로 세팅하여 시리얼 통신을 하면  
  업로드한 코드의 동작을 확인할 수 있다.  
  (업로드한 코드에서 설정한 115200의 baudrate으로 통신)  

    <div align="center">  
    <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/78722a74-6ab4-4ae5-a3dd-fa45a460454c" width="500" height="350">  
    </div><br>  

    ```shell
     Wi-Fi scan result :  1 networks found

    [Wi-Fi 1]  iptime		(-88 dbm)   * encryption *

    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 7]
    Connecting to 'iptime' [status : 3]

    Wi-Fi connected.
    ```
    - WiFi.status()  
      |WiFi.status()     |Value |의미                    |  
      |------------------|------|------------------------|  
      |WL_NO_SSID_AVAIL  |1     |지정된 SSID를 찾지 못함  |  
      |WL_SCAN_COMPLETED |4     |Wi-Fi 네트워크 스캔 완료 |  
      |WL_DISCONNECTED   |7     |Wi-Fi 연결이 끊어짐      |  
      |WL_CONNECTED      |3     |Wi-Fi 연결 성공          |  

    노트북에서 소켓을 생성하여 서버가 수신을 대기하고 있어야 클라이언트와 서버가 연결된다.  
<br>  

10. 노트북 host 서버에서 수신  
  노트북에서 서버 IP는 자신이므로 "0.0.0.0"(localhost)을 사용  
  서버 port는 사용하지 않고 있는 특정 포트를 지정  

    ```python
    import socket


    server_ip = "0.0.0.0"  # localhost
    server_port = 12345

    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((server_ip, server_port))
    server_socket.listen(1)
    print(f"Listening '{server_ip}:{server_port}'\n")

    while True:
        client_socket, client_address = server_socket.accept()
        print(f"Accepted connection from {client_address}")

        data = client_socket.recv(1024)  # Receive up to 1024 bytes of data
        print(f"Received data : {data.decode('utf-8')}")

        client_socket.close()
    ```
    ```shell
    Listening '0.0.0.0:12345'

    Accepted connection from ('192.168.0.138', 50027)
    Received data : 581
    Accepted connection from ('192.168.0.138', 64175)
    Received data : 582
    Accepted connection from ('192.168.0.138', 61180)
    Received data : 583
    ```
    <br>
    client의 IP는 공유기에서 할당받은 ip가 그대로 사용되고 있지만 port는 계속 바뀌는데  

    ```python
    client_socket.close()
    ```
    이 코드로 데이터를 한번 수신하고 나면 소켓을 종료시켜줌으로써  
    다음 데이터를 받을 때 클라이언트가 다시 연결을 시도하고, 동적으로 새로운 포트 번호를 선택하기 때문이다.  

    여기서 서버의 port 번호까지 변경되는 것은 아니고 코드에서 지정한 대상의 포트가 고정되어 계속 사용된다.  

    - 참고  
    TCP 포트 번호는 16비트 숫자로 표현되어 0~65535까지 가능하다.  
      |포트 번호   |일반적인 용도                 |  
      |------------|-----------------------------|  
      |0~1023      |HTTP 웹 서버 80포트, HTTPS 443포트, 등<br>널리 알려진 특정 서비스 포트 |  
      |1024~49151  |애플리케이션이나 서비스를 등록하기 위해 할당   |  
      |49152~65535 |클라이언트 애플리케이션이 일시적으로 사용 가능 |  
    <br>
    또한, 하나의 클라이언트하고만 계속 통신을 하는 경우라면 괜찮을 수 있지만,  
    여러 클라이언트로 부터 데이터를 수신 받는 경우, 어떤 클라이언트로 부터 데이터를 제대로 수신받지 못해 대기 상태로 남아있게 되면  
    다른 클라이언트들과의 연결을 처리하지 못하고 데드락 상태에 빠질 수 있기 때문에 소켓을 종료시켜주는 것이 좋다.  
<br><br>  


### TCP vs UDP  
- TCP(Transmission Control Protocol)  
클라이언트와 서버 간의 연결 지향 프로토콜로,  
데이터가 손실된 경우, 데이터를 재전송하여 오류를 복구하고 데이터를 전송한 순서대로 수신하도록 순서를 보장해주기 때문에 안정적인 전송이 가능하여 신뢰성이 좋다.  
대신 데이터의 신뢰성을 위해 복잡한 기능을 제공하므로 헤더의 크기가 크다.  
웹 브라우징, 이메일, 파일 전송과 같은 신뢰성이 필요한 통신에 주로 사용한다.  

- UDP(User Datagram Protocol)  
클라이언트와 서버 간의 비연결성 프로토콜로,  
데이터 손실이나 중복 전송에 대한 처리를 하지 않고 그대로 전송하기 때문에 신뢰성이 떨어지는 대신 전송 속도가 빠르다는 장점이 있다.  
<br>
  |프로토콜 |연결성 |신뢰성     |헤더      |전송 순서 |전송 속도      |  
  |---------|-------|----------|----------|---------|---------------|  
  |**TCP**  |관리O  |손실 복구O |크고 복잡 |보장O    |상대적으로 느림 |  
  |**UDP**  |관리X  |손실 복구X |작고 간단 |보장X    |빠름            |  
<br><br>  


## Wi-Fi 인터페이스 보드  
### 시리얼 접속  
|       |          |   |   |    |  
|-------|----------|---|---|----|  
|인터페이스 보드 |5V (3.3V)<br>(C타입 포트로 전원을 넣어주면 3.3V는 연결 안해도 됨)|TX |RX |GND |  
|CP2102 |5V (3.3V) |RX |TX |GND |  
<br><br>  




ESP-12F에 코드를 업로드하고 나면 AT command가 작동하지 않기 때문에 AT 명령어를 사용하고 싶다면 초기상태로 다시 업로드 해주어야 한다.  
- ESP8266 DOWNLOAD TOOL  
https://www.espressif.com/en/support/download/other-tools?keys=&&&order=field_release_date&sort=desc  

- 펌웨어 다운로드 참고  
  https://crystalcube.co.kr/215  
  https://chocoball.tistory.com/entry/Hardware-ESP12-using  

- AT 커맨드 참고  
  https://bcho.tistory.com/1283  
  명령어는 순서대로 다 해야함  

### AT command  
- AT 응답 확인  
  ```shell
  AT
  ```
- Firmware 버전 확인  
  ```shell
  AT+GMR
  ```
- Wi-Fi 네트워크 모드 확인  
  ```shell
  AT+CWMODE?
  ```
- Wi-Fi 네트워크 모드 설정  
  ```shell
  AT+CWMODE=1
  ```
  |Mode Number |Wi-Fi Mode    |  
  |------------|--------------|  
  |1 |Station Mode (디바이스) |  
  |2 |AP Mode (Access Point)  |  
  |3 |AP + Station Mode       |  
- 연결 가능한 Wi-Fi 목록 확인  
  ```shell
  AT+CWLAP
  ```
  Wi-Fi 네트워크 모드를 설정해야 목록을 확인할 수 있으며,  
  모드에 따라 출력되는 목록의 결과가 달라짐  
- Wi-Fi 네트워크 연결  
  ```shell
  AT+CWJAP="SSID","Password"
  ```
  한번 연결하면 재부팅 후에도 같은 네트워크에 자동으로 연결한다.  
<br>  

- Wi-Fi 연결 후 IP 할당 실패 시  
  ```shell
  AT+CWJAP="myssid","mypassword"
  WIFI CONNECTED
  +CWJAP:1

  FAIL
  WIFI DISCONNECT
  ```
  |+CWJAP |Message    |  
  |-------|-----------|  
  |0 |Wi-Fi 연결 성공 |  
  |1 |Wi-Fi 연결 실패 (PW나 연결 문제) |  
  |2 |Wi-Fi 인증 실패 |  
  |3 |DHCP 오류       |  
  |4 |연결을 중단함   |  
  |5 |연결이 끊어짐   |  

  SSID와 PW는 올바르게 입력하였지만 와이파이 연결이 되고 곧바로 실패하는 경우,  
  ESP8266이 default로 DHCP가 disable 되어있기 때문에 이러한 오류가 발생할 수 있다.  
  <br>  
  ```shell
  AT+CWDHCP=1,1
  ```
  AT+CWDHCP={mode},{en}  
  DHCP를 enable로 바꿔준 후 다시 연결을 시도하면 정상적으로 IP를 할당받아 연결이 완료된다.  


- 연결된 IP 주소 및 MAC 주소 확인  
  ```shell
  AT+CIFSR
  ```
- PING 날리기  
  ```shell
  AT+PING="www.google.com"
  ```
- 네트워크 연결 끊기  
  ```shell
  AT+CWQAP
  ```
