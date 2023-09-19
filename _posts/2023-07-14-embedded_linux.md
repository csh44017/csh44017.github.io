---
title:  "임베디드 리눅스 참고"
excerpt: "임베디드 리눅스에 관련된 배경지식 정리"

categories:
  - Linux
tags:
  - [Embedded, Linux]

toc: true
toc_sticky: true

date: 2023-07-14
last_modified_at: 2023-07-14
---

## Linux  
- BootLoader  
- Kernel  
한가지를 공통으로 사용하지만 버전이 중요  
- File System  
Ubuntu, CentOS, ...

## OS  
- Multi Process
- Memory  
- File System  

### 부트로더  
운영체제(OS)가 시동되기 이전에 미리 실행되면서 커널이 올바르게 시동되기 위해 필요한 모든 관련 작업을 마무리하고 최종적으로 운영체제를 시동시키기 위한 목적을 가진 프로그램  
메모리, 하드웨어(네트워크, 프로세서 속도, 인터럽트), 코드/데이터/스택 영역 설정 및 초기화, 커널 로더와 커널 이미지를 로딩, 커널 로더를 실행하여 커널 이미지가 실행되도록 함  
하드웨어 의존성이 강한 코드들로 되어 있고(대부분 어셈블리언어로 작성 됨) 프로그래머는 프로세서 구조(Clock, UART, Ethernet 등), 특징, 사용법에 대해 잘 알고 있어야 작업 가능  

### 부트로더 종류  
- LILO  
LInux LOader, 리눅스에서 사용되었으며, GRUB를 기본 부트로더로 사용하기 전까지 리눅스 배포판에서 사용하던 기본 부트로더
- GRUB  
GRand Unified Bootloader, GNU 프로젝트의 부트로더이며, 현재 대부분의 리눅스 배포판에서 사용, a.out와 ELF 포멧 지원, BIOS에서 인식되는 모든 장치에 액세스 가능  
- BLOB  
Boot Loader OBject, ARM용 부트로더로 임베디드 리눅스 상에서 LILO와 같이 선택 부팅이 가능하도록 기능 제공하며, 리눅스 커널 다운로드를 시리얼 케이블을 이용할 수 있도록 제공  
- ARMBOOT  
StrongARM을 위한 공개 소스 펌웨어, 다중형 플래시 메모리 지원, tftp, PCMCIA CF 부트등을 지원  
- RedBoot  
RedHat에서 개발한 임베디드 운영체제인 eCOS를 기반으로하여 만든 부트로더  
- U-BOOT  
Universal BOOTloader, 주로 PowerPC와 ARM 임베디드 시스템에서 사용되는 부트로더이며 오픈소스  
  - 깃허브 소스트리  
  https://github.com/u-boot/u-boot  

### U-Boot  
장치에서 U-Boot 모드로 들어가서 명령 프롬프트에 액세스하려면 전원을 켜고 부팅 프로세스를 'Ctrl+C' 등으로 중단하면 된다.  
명령 프롬프트를 통해 명령어를 입력하여 부트로더와 상호작용하고 설정을 구성하거나 이미지를 로드하는 등의 작업을 수행할 수 있다.  
