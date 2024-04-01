---
title:  "Minicom으로 AT command 보내기"
excerpt: "ESP-12F 모듈을 이용해 서버와 소켓 통신"

categories:
  - Radar
tags:
  - [Wi-Fi, Minicom, AT-command]

toc: true
toc_sticky: true

date: 2022-08-12
last_modified_at: 2023-11-01
---


- 순서  
    1. 센서에서 signal processing을 통해 얻은 데이터들을 포함한 메시지를 출력  
    2. Serial port로 인터페이스 보드에서 패킷을 수신 후 메시지 파싱  

    ###
    1. 와이파이 보드의 10핀 커넥터에 레이더를 연결  
    2. cp2102로 노트북과 시리얼 통신으로 접속  

    3. 소켓 통신을 위해 노트북이랑 같은 공유기의 와이파이에 연결  
    4. UDP로 레이더 데이터를 노트북에 전송  

    5. 노트북에서 수신받은 데이터를 서버로 저장  

레이더 - (10핀 커넥터) - 인터페이스 보드 - (UART) - Wi-Fi 모듈 - (AT command) - 특정 공유기의 와이파이 연결 - Wi-Fi 모듈이 클라이언트가 되어 데이터 송신 - (Socket communication) - 노트북에 같은 공유기의 와이파이 연결 - 노트북에서 socket server를 열고 데이터 수신 - 

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/631ac666-fc5a-4631-b895-715c008ebb2a" width="500" height="120">  
</div>
<br><br>  


## 네트워크 없는 우분투 환경에서 Minicom 설치하기  
우분투 환경에서 네트워크를 연결할 수 없다면 필요한 패키지를 다운받거나 깃허브에서 이전에 작업한 코드를 가져오는 등 여러 작업이 제한되어 상당히 불편한데,  
센서에서 나온 데이터를 처리하고 Wi-Fi 모듈을 통해 MQTT 프로토콜로 메시지를 송수신하기 위해 제작한 인터페이스 보드에서 단가나 크기 때문인지 네트워크 연결이 불가능한 상태로 제작되었다.  
<br><br>  


### Minicom 패키지 선택  
구글에 'minicom deb'라고 검색하면 다양한 사이트에서 버전 별로 다운받을 수 있는 패키지 파일을 제공하고 있다.  

현재 사용중인 우분투 버전을 확인하기 위해 아래 명령어를 입력하면  
```shell
lsb_release -a
```
```shell
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.2 LTS
Release:        20.04
Codename:       focal
```
Description에서 'Ubuntu 20.04.2 LTS' 배포판이 설치되어 있음과 해당 버전의 Codename이 'focal'인 것을 확인할 수 있다.  
([Ubuntu 버전별 code name 참고](https://wiki.ubuntu.com/Releases#End_of_Life_.28EOL.29){:target="_blank"})  
<br><br>  


### Minicom 패키지 다운로드  
[우분투 minicom 패키지 아카이브](https://launchpad.net/ubuntu/+source/minicom){:target="_blank"}에서 동일한 코드네임인 'The Focal Fossa'에 있는 '2.7.1-1.1'을 보면 여러 package file들이 있다.  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/c5f12e61-6ad4-4cb5-bf34-7a81befd10e8" width="500" height="400">  
</div>

소스 코드가 압축되어 있는 '.tar.xz' 파일의 압축을 풀고 직접 컴파일 및 빌드하여 설치할 수도 있지만, '.deb' 파일을 사용해서 설치하는 것이 훨씬 간단하기 때문에 시스템 아키텍처에 맞는 'minicom_2.7.1-1.1_armhf.deb'를 다운받아서 진행한다.  

- dpkg 설치 확인  
  ```bash
  dpkg --version
  ```
  ```shell
  Debian 'dpkg' package management program version 1.19.7 (armhf).
  This is free software; see the GNU General Public License version 2 or
  later for copying conditions. There is NO warranty.
  ```
  .deb 파일로 설치하기 위해 dpkg가 사용가능해야 한다.  
- 리눅스에서 시스템 아키텍처 확인  
  ```bash
  uname -m
  ```
  ```bash
  arch
  ```
<br>  


### PC에서 보드로 패키지 파일 복사  
네트워크가 연결되어 ip가 잡힌다면 FileZilla나 scp 등을 이용해 로컬과 원격지 간에 파일을 복사가 가능하지만, 네트워크가 연결되지 않는 상황이므로 다른 방법으로 pc의 파일을 복사해야 한다.  
<br>  

#### 디스크 인식 오류  
인터페이스 보드의 sd카드를 pc에 삽입하고 sd 카드를 인식한 드라이브에 직접 복사하려고 했지만 'D: 드라이브의 디스크를 사용하기 전에 포맷해야 합니다.'라는 창이 뜨면서 D드라이브에 엑세스 되지 않았다.  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/66574e16-fc79-4552-ab77-8042dd8f97c2" width="300" height="140">  
</div>

sd카드가 초기화되므로 디스크 포맷은 할 수 없기에 취소를 선택한 뒤,  
'Win + R'를 누르고 아래의 디스크 관리자를 실행하는 명령어를 입력하거나
```cmd
diskmgmt.msc
```
'컴퓨터 관리 > 저장소 > 디스크 관리'에 들어가 현재 가지고 있는 볼륨에 대한 정보를 확인해보았다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/dde690af-d6c0-4070-9423-d9f9f8b23f19" width="500" height="330">  
</div>

sd카드를 삽입하여 생긴 '디스크1'에 인식된 파일 시스템이 없는 상태였다.  
(다른 사례들을 찾아보면 문제가 있을 경우 파일 시스템에 'RAW'라고 표기되어 있는데, 아무것도 표시되지 않고 드라이브의 문자도 보이지 않았다.)  

1. 체크디스크 명령어  
    명령 프롬프트를 관리자의 권한으로 실행해서  
    ```cmd
    chkdsk d: /f
    ```
    체크디스크 명령어로 D 드라이버에 있는 오류를 고치도록 입력해보았지만,  
    ```cmd
    파일 시스템 유형은 RAW입니다.
    RAW 드라이브에 대하여 CHKDSK을(를) 사용할 수 없습니다.
    ```
    라는 오류가 나왔다.  

2. USB 드라이브 (D:) 속성 > 도구 에서 오류 검사의 검사 진행  
    '디스크가 포맷되지 않았기 때문에 디스크 검사를 수행할 수 없습니다.'라는 창이 나왔다.  

3. 드라이브 문자 및 경로 변경  
  '디스크 관리 콘솔 보기가 최신이 아니므로 작업을 완료하지 못했습니다.'라는 창이 나왔다.  

4. 복구 프로그램 사용  
    'EaseUS Data Recovery Wizard'라는 프로그램을 다운받아서 파일을 

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/b1fdf6e3-314c-4db2-a2da-afbcfe81414d" width="500" height="120">  
</div>

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/091a7347-16ef-4ff3-8e56-954eb3675d82" width="500" height="330">  
</div>

복구 프로그램을 사용하다가 SD 카드의 파일 시스템이 EXT4인 것을 보고  
윈도우와 리눅스에서 주로 지원하는 파일 시스템이 다르다는 것이 떠올랐다.  

윈도우에서는 기본적으로 'NTFS(New Technology File System)'나 'FAT(File Allocation Table)' 파일 시스템을 지원하고, 리눅스에서는 기본적으로 'EXT4' 파일 시스템을 지원한다.  

결과적으로 우분투가 설치되어 EXT4로 포맷된 SD카드가 삽입되었을 때  
윈도우에서 지원하는 파일시스템이 아니기 때문에 인식을 하지 못하고 포맷을 권장하는 것이었다.  
<br>  

#### 해결 방법  
- 윈도우가 아닌 우분투가 깔려있는 PC를 통해 SD카드 리더기를 이용하여 접근한 뒤 파일을 복사  
- TeraTerm에서 Zmodem 프로토콜을 이용하여 파일을 전송  
  (우분투에 Zmodem 프로토콜을 지원하는 패키지인 lrzsz가 설치되어있어야 한다.)  
  - lrzsz 설치 (네트워크가 있을 경우)  
    ```bash
    sudo apt-get install lrzsz
    ```

중간에 패키지가 필요할 때마다 SD카드를 분리해서 다른 PC를 통해 복사하는 것은 번거롭다는 생각이 들어 SD 카드에 'lrzsz' 패키지만 설치해놓고 TeraTerm과 Zmodem을 이용하는 방법을 선택했다.  
(TeraTerm에서 단순히 '파일 보내기'를 하면 적절한 프로토콜이 사용되지 않아서인지 파일이 깨져서 수신되고, Zmodem 이외에 Xmodem, Ymodem도 있지만 Zmodem이 가장 효과적이고 많이 사용된다.)  
<br>  

#### 파일 수신  
CP2102와 같은 USB to TTL 컨버터로 PC의 USB 포트와 인터페이스 보드의 TTL 시리얼 포트를 연결한다.  

TeraTerm에서 해당 시리얼 포트로 인터페이스 보드에 접속하고, 아래 명령어를 입력하여 파일 수신 준비를 해준다.  
- Zmodem 프로토콜로 파일 수신 준비  
  ```bash
  rz
  ```
  ```bash
  rz waiting to receive.**B0100000023be50
  ```
<br>  

#### 파일 전송
'메뉴 > 전송 > ZMODEM > 보내기'에서  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/7c9f5296-5ac3-4fff-a1c1-e26825ad708f" width="400" height="350">  
</div>

전송할 파일로 'minicom_2.7.1-1.1_arm64.deb'을 선택해주면 바로 전송을 시작한다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/b51e2fad-8b84-407e-9c18-2bdd306cf8b9" width="300" height="190">  
</div>

전송이 완료되면 파일이 정상적으로 존재하는 것을 확인할 수 있다.  
- 파일 목록 조회
  ```bash
  ls
  ```
  ```bash
  bin  minicom_2.7.1-1.1_armhf.deb
  ```
<br><br>  


### 보드에 Minicom 설치  
- 패키지 설치  
  ```bash
  sudo dpkg -i minicom_2.7.1-1.1_armhf.deb
  ```
- Minicom 설치 확인  
  ```bash
  minicom --version
  ```
  ```bash
  minicom version 2.7.1 (compiled Dec 23 2019)
  Copyright (C) Miquel van Smoorenburg.

  This program is free software; you can redistribute it and/or
  modify it under the terms of the GNU General Public License
  as published by the Free Software Foundation; either version
  2 of the License, or (at your option) any later version.
  ```
<br><br>  



## Minicom으로 시리얼 통신하여 AT command 사용  
ESP-AT User Guide의 [Wi-Fi AT Commands](https://docs.espressif.com/projects/esp-at/en/release-v2.1.0.0_esp32s2/AT_Command_Set/Wi-Fi_AT_Commands.html){:target="_blank"}를 참고  

- 모든 USB 디바이스 목록 조회  
  ```bash
  ls /dev/tty*
  ```
- 우분투에서 사용중인 시리얼 포트 확인  
  ```bash
  dmesg | grep tty
  ```
  ```bash
  [    0.000000] Kernel command line: console=ttyS0,115200n8 root=/dev/mmcblk0p1 ro rootfstype=ext4 rootwait
  [    3.942566] printk: console [ttyS0] disabled
  [    3.942697] 44e09000.serial: ttyS0 at MMIO 0x44e09000 (irq = 20, base_baud = 3000000) is a 8250
  [    4.780923] printk: console [ttyS0] enabled
  [    4.787581] 48022000.serial: ttyS1 at MMIO 0x48022000 (irq = 27, base_baud = 3000000) is a 8250
  [    4.798770] 48024000.serial: ttyS2 at MMIO 0x48024000 (irq = 28, base_baud = 3000000) is a 8250
  [    4.810023] 481a8000.serial: ttyS4 at MMIO 0x481a8000 (irq = 39, base_baud = 3000000) is a 8250
  [    9.045112] systemd[1]: Created slice system-serial\x2dgetty.slice.
  ```
  'ttyS0'가 현재 CP2102에서 115200으로 사용중인 포트이고,  
  보드의 회로도를 보면 나머지 포트 중에 하나에서 ESP-12F 모듈과 UART로 연결되어 있다.  
  (해당 포트로 AT command를 전송할 예정)  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/ecaeeb63-d2b4-485e-8835-e1e504af88be" width="500" height="170">  
</div>

- Minicom 실행 후 configuration 모드로 진입  
  ```bash
  sudo minicom -s
  ```
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/bc9e057e-4796-4ffd-b8f2-5badf889bd0c" width="250" height="200">  
</div>

- 시리얼 포트 설정  
  1. 방향키를 통해 'Serial port setup'으로 이동 후 엔터를 눌러 진입  
      <div align="center">  
      <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/2150a542-8371-40fc-bec3-8b90767c6ea5" width="500" height="250">  
      </div><br>  
  2. 알파벳을 입력해 설정을 변경할 항목으로 커서 이동  
  3. '/dev/modem'을 통신할 포트인 '/dev/ttyS4'으로 수정하고 엔터  
  4. esc 키를 통해 빠져나온 뒤, 'Save setup as dfl'로 이동 후 엔터를 눌러 현재 설정을 디폴트로 저장  
  ('configuration saved' 메시지가 나옴)  
  5. esc 키를 한번 더 눌러주면 minicom이 실핼 중인 화면으로 넘어감  

- Minicom 실행  
  ```bash
  sudo minicom
  ```
  ```bash
  minicom -b 115200 -D /dev/ttyS4 crlf
  ```
- Minicom 종료  
  ```bash
  Ctrl + A, X
  ```
- Minicom 커맨드 목록 확인  
  ```bash
  Ctrl + A, Z
  ```
- Minicom 캐리지 리턴 CR-LF으로 변경  
  ```bash
  Ctrl + A, Z, U
  ```
- 지나간 화면 보기  
  ```bash
  Ctrl + A, B
  ```
  방향키로 움직여서 하단으로 이동하고, esc를 통해 빠져나오기  
<br><br>  


### AT 커맨드  
- 커맨드 입력 결과 확인  
  ```bash
  Enter, Ctrl + J
  ```
  ```bash
  AT
  ```
  ```bash
  OK
  ```
- 데이터 전송  
  ```bash
  AT+CIPSTART="TCP","192.168.0.40",12345
  ```
  ```bash
  AT+CIPSEND=10
  ```
  ```bash
  > HelloWorld
  ```

<br><br>  
https://mirror.librelabucm.org/trisquel/pool/main/v/vim/  
vim_8.1.2269-1ubuntu5_armhf.deb를 다운  

### MQTT 프로토콜 테스트  
