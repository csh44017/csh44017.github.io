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
TI의 레이더 소스코드를 보면 사용자가 환경에 따라 변경할만한 파라메타들을 튜닝해서 사용할 수 있도록 cli를 통해 레이더의 동작에 관련된 설정들을 변경할 수 있도록 구현해 놓았다.  

<div align="center">
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/a6c3bdf0-a5a0-4b1c-8d59-dd0029622598">  
</div>
몇몇 파라메타 값을 변경하기 위해서는 위의 chirp diagram에 대한 이해가 필요하다.  
<br>  

### 1. Configuration Command  
- dfeDataOutputMode  
  (필수로 입력해야 하는 command이며, sensorStop과 sensorStart 사이에서 값을 갱신하면 안되기 때문에 다른 설정 값을 사용하고 싶다면 보드를 재부팅한 후 command를 입력)  

  - modeType  

    | Value | Description |
    | ----- | ----------- |
    | **1** | **프레임에 기반한 chirp 사용 모드** |
    | 2     | 연속적으로 chirp 쏘기 (xwr68xx에서 지원X) |
    | 3     | 고급 프레임 설정 모드 |

- channelCfg  
  레이더 안테나 송수신 채널 설정  
  (필수로 입력해야 하는 command이며, sensorStop과 sensorStart 사이에서 값을 갱신하면 안되기 때문에 다른 설정 값을 사용하고 싶다면 보드를 재부팅한 후 command를 입력)  

  - rxChannelEn  
    사용할 수신 안테나를 지정하기 위한 비트 마스크 값  

    | Value  | Description |
    | ------ | ----------- |
    | **15** | **0b1111로 마스킹되어 4개 안테나를 모두 사용** (xwr68xx는 4개 안테나 지원) |

  - txChannelEn  
    사용할 송신 안테나를 지정하기 위한 비트 마스크 값  

    | Value | Description |
    | ----- | ----------- |
    | **7** | **0b111로 마스킹되어 3개 안테나를 모두 사용** (xwr68xx는 3개 안테나 지원) |

  - cascading  
    SoC cascading 값  
    
    | Value | Description |
    | ----- | ----------- |
    | **0** | **해당되지 않음** |

- adcCfg  
  ADC 데이터 포맷 설정  
  (필수로 입력해야 하는 command이며, sensorStop과 sensorStart 사이에서 값을 갱신하면 안되기 때문에 다른 설정 값을 사용하고 싶다면 보드를 재부팅한 후 command를 입력)  

  - numADCBits  

    | Value | Description |
    | ----- | ----------- |
    | 0     | 12-bits |
    | 1     | 14-bits |
    | **2** | **16-bits** (xwr68xx는 16bit만 지원) |

  - adcOutputFmt  

    | Value | Description |
    | ----- | ----------- |
    | 0     | 실수 (xwr68xx는 지원X) |
    | 1     | 허수 부분을 필터링한 복소수 |
    | **2** | **허수 부분까지 사용된 복소수** |

- adcbufCfg  
  ADC 데이터 버퍼의 하드웨어 설정  
  (필수로 입력해야 하는 command이며, sensorStop과 sensorStart 사이에서 값을 갱신 가능)  
  - subFrameIdx  

    | Value  | Description |
    | ------ | ----------- |
    | **-1** | **모든 서브 프레임들에 같은 설정 적용** (legacy 모드에서는 -1로 설정해야 함) |
    | either | dfeDataOutputMode 를 3(고급 프레임 설정 모드)으로 세팅한 경우 원하는 서브 프레임 번호를 value로 세팅 |
    
  - adcOutputFmt  
    ADC 버퍼에서 나갈 데이터의 포맷 설정  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **복소수** (xwr68xx는 복소수만 지원) |
    | 1     | 실수 |

  - SampleSwap  
    샘플링한 IQ 데이터의 I와 Q 순서 설정  

    | Value | Description |
    | ----- | ----------- |
    | 0     | I를 하위, Q를 상위로 저장 |
    | **1** | **Q를 하위, I를 상위로 저장** (xwr68xx는 1만 지원) |
    
  - ChanInterleave  
    각각의 안테나에서 수신한 ADC 버퍼 데이터를 안테나 채널을 전환하면서 저장할지 여부를 설정  

    | Value | Description |
    | ----- | ----------- |
    | 0     | 인터리빙 패턴에 따라 채널을 전환 (xwr14xx에서만 지원) |
    | **1** | **채널을 전환하지 않음** (xwr68xx는 1만 지원) |
    
  - ChirpThreshold  
    ADC 버퍼에서 ping/pong 버퍼 스위치를 트리거하기 위해 사용되는 threshold 구성  

    | Value | Description |
    | ----- | ----------- |
    | 0 ~ 8 | DSP를 사용하여 1D FFT를 수행하는 경우 |
    | **1** | DSP가 아닌 **HWA에서 1D FFT를 수행하거나 LVDS streaming 사용**하는 경우 (xwr68xx는 1만 지원) |

- profileCfg  
  
  - profileId  
    **프로파일 식별자**  
    (아무 값이나 사용 가능하지만, 특별한 것이 없으면 보통 0으로 사용)  

    | Value        | Description |
    | ------------ | ----------- |
    | **anything** | dfeOutputMode를 1(프레임에 기반한 chirp 사용 모드)로 세팅한 경우, **아무 값이나 사용 가능 (config 당 1개의 유효한 프로파일 지원)** |
    | anything | dfeOutputMode를 3(고급 프레임 설정 모드)로 세팅한 경우, 아무 값이나 사용 가능 (서브 프레임당 1개의 프로파일이 지원되며, 다른 서브 프레임들은 다른 프로파일들을 가져야 함) |

  - startFreq  
    **주파수 변조를 시작할 GHz 단위 주파수** (float로 작성 가능)  
    RampStart에서의 주파수도 해당 주파수가 된다.  

    | Value | Description |
    | ----- | ----------- |
    | 61.2  | 61.2GHz에서 시작 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |
    | 77    | 77GHz에서 시작 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |

  - idleTime  
    **RampEnd와 RampStart 사이에 해당하는 us 단위의 유휴 상태 시간** (float로 작성 가능)  
    Ramp가 끝났기 때문에 다음 Ramp를 준비하기 위해 idleTime 동안 직전 Ramp 에서 증가시켰던 주파수를 다시 startFreq로 내리고 (내려간 주파수가 startFreq에서 흔들리지 않고 안정된 상태가 되어야 함), RampEnd와 함께 off 되었던 TX를 다시 ON 시킨다.  

    | Value | Description |
    | ----- | ----------- |
    | 7.34  | 7.34us의 유휴 시간 설정 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |
    | 60.00 | 60.00us의 유휴 시간 설정 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |

  - adcStartTime  
    **RampStart를 기준으로 설정한 ADC 대기 시간** (float로 작성 가능)  
    RampStart에서 주파수가 높아지기 시작한 후 FrequencySlope가 안정된 구간에서 유효한 ADC 샘플링을 하기 위해 필요하다.  

    | Value | Description |
    | ----- | ----------- |
    | 17.00 | 17.00 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |

  - rampEndTime  
    **RampStart를 0초 시작으로 하여 Ramp가 끝나는 us 단위의 시간 설정**  
    rampEndTime 동안 주파수가 높아진다.  








단순히 센서에 전원을 인가하는 것만으로는 RF가 동작하지 않고 cli를 통해 sensor를 start 시켜주어야 센서 데이터를 보내기 시작한다.  
<br>  

### 2. cfg 파일 예시  
```cfg  
sensorStop
dfeDataOutputMode 1
channelCfg 15 7 0
adcCfg 2 1
adcbufCfg -1 0 1 1 1
lowPower 0 0

profileCfg 0 61.2 60.00 17.00 50 394758 0 55.27 1 64 2000.00 2 1 36
chirpCfg 0 0 0 0 0 0 0 1
chirpCfg 1 1 0 0 0 0 0 2
chirpCfg 2 2 0 0 0 0 0 4
frameCfg 0 2 224 400 120.00 1 0


dynamic2DAngleCfg -1 5 1 1 1.00 15.00 2
dynamicRACfarCfg -1 10 1 1 1 8 8 6 4 4.00 6.00 0.50 1 1
staticRACfarCfg -1 4 4 2 2 8 16 4 6 6.01 13.00 0.50 0 0
dynamicRangeAngleCfg -1 7.000 0.0010 2 0
staticRangeAngleCfg -1 0 1 1

antGeometry0 -1 -1 0 0 -3 -3 -2 -2 -1 -1 0 0
antGeometry1 -1 0 -1 0 -3 -2 -3 -2 -3 -2 -3 -2
antPhaseRot -1 1 -1 1 -1 1 -1 1 -1 1 -1 1
fovCfg -1 64.0 64.0
compRangeBiasAndRxChanPhase 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0


staticBoundaryBox -3 3 -3 3 -0.5 3
boundaryBox -5 5 -5 5 -0.5 3.5
sensorPosition 3.5 0 90
gatingParam 2 2 2 2 4
stateParam 3 3 6 300 5 1000
allocationParam 2 100 0.01 5 1.5 20
maxAcceleration 0.2 0.01 0.2
trackingCfg 1 4 800 20 37 33 120
presenceBoundaryBox -6 6 -6 6 0.5 4.0
sensorStart
```  
<br>

### 3. Command 입력  
TeraTerm 같은 시리얼 통신 프로그램을 통해서 **통신 포트(Enhanced COM Port)**와 **통신 속도(921600)**를 지정하여 포트를 오픈한 후 직접 명령어를 입력할 수도 있지만, 
맞춰놓았던 설정 값들을 cfg파일에 필요한 command line을 작성해놓고 uart 통신으로 각각의 라인들을 차례대로 입력하면 편리하게 사용할 수 있다.  
- UART 통신으로 command 입력  
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

      # command - acknowledge
      def loop(self):
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
      while True:
          time.sleep(0.5)
          if rx.is_ready():
              rx.loop()
              break
  ```  
  여기에서 **ack = uart_com.readline().decode()** 를 연속으로 두 번 호출하는 이유는 명령-응답의 상호 작용을 위한 통신 타이밍을 보장하기 위한 것으로 보여진다. 이와 더불어 line 간에 sleep()도 적절한 통신을 위해 반드시 필요하다.  
  (두 번의 호출 중에 하나의 ack에서만 응답이 들어있음)  
  
  - readline()을 한번만 사용하고 time.sleep()을 0.2초로 설정한 경우  
    command가 끝까지 출력되지 않고 중간에 끊어지는 문제가 발생하였다.  

  <br>
  이외에도 readline()을 연속으로 두 번 호출하면 실제 응답이 아닌 이전 응답이나 불완전한 데이터를 첫 번째 read에서 가져가 버퍼에 존재할 수 있는 원치 않는 데이터를 버리고, 두 번째 호출에서 실제 응답을 읽어들여 '오류 처리'나 '동기화'에 관련된 방법으로 사용될 수 있다.  














### CLI 코드  
- CLI Init  
  ```c  
  void MmwDemo_CLIInit (uint8_t taskPriority)
  {
      CLI_Cfg     cliCfg;
      char        demoBanner[256];
      uint32_t    cnt;

      /* Create Demo Banner to be printed out by CLI */
      sprintf(&demoBanner[0], 
                         "******************************************\n" \
                         "xWR64xx MMW Demo %02d.%02d.%02d.%02d\n"  \
                         "******************************************\n", 
                          MMWAVE_SDK_VERSION_MAJOR,
                          MMWAVE_SDK_VERSION_MINOR,
                          MMWAVE_SDK_VERSION_BUGFIX,
                          MMWAVE_SDK_VERSION_BUILD
              );


       /* Initialize the CLI configuration: */
      memset ((void *)&cliCfg, 0, sizeof(CLI_Cfg));

      /* Populate the CLI configuration: */
      cliCfg.cliPrompt                    = "mmwDemo:/>";
      cliCfg.cliBanner                    = demoBanner;
      cliCfg.cliUartHandle                = gMmwMCB.commandUartHandle;
      cliCfg.taskPriority                 = taskPriority;
      cliCfg.socHandle                    = gMmwMCB.socHandle;
      cliCfg.mmWaveHandle                 = gMmwMCB.ctrlHandle;
      cliCfg.enableMMWaveExtension        = 1U;
      cliCfg.usePolledMode                = true;
      cliCfg.overridePlatform             = true;
  #if defined(USE_2D_AOA_DPU)
      cliCfg.overridePlatformString       = "xWR68xx_AOP";
  #else
      cliCfg.overridePlatformString       = "xWR64xx";
  #endif
    
      cnt=0;
      cliCfg.tableEntry[cnt].cmd            = "sensorStart";
      cliCfg.tableEntry[cnt].helpString     = "[doReconfig(optional, default:enabled)]";
      cliCfg.tableEntry[cnt].cmdHandlerFxn  = MmwDemo_CLISensorStart;
      cnt++;
  
      cliCfg.tableEntry[cnt].cmd            = "sensorStop";
      cliCfg.tableEntry[cnt].helpString     = "No arguments";
      cliCfg.tableEntry[cnt].cmdHandlerFxn  = MmwDemo_CLISensorStop;
      cnt++;

      /* Open the CLI: */
      if (CLI_open (&cliCfg) < 0)
      {
          System_printf ("Error: Unable to open the CLI\n");
          return;
      }
      System_printf ("Debug: CLI is operational\n");
      return;
  }
  ```  

- CLI task  
  ```c  
  static void CLI_task(UArg arg0, UArg arg1)
  {
      uint8_t                 cmdString[256];  // 입력 받은 커맨드
      char*                   tokenizedArgs[CLI_MAX_ARGS];
      char*                   ptrCLICommand;
      char                    delimitter[] = " \r\n";
      uint32_t                argIndex;
      CLI_CmdTableEntry*      ptrCLICommandEntry;
      int32_t                 cliStatus;
      uint32_t                index;
  
      // banner 존재 시 출력
      if (gCLI.cfg.cliBanner != NULL)
      {
          CLI_write (gCLI.cfg.cliBanner);
      }
  
      /* Loop around forever: */
      while (1)
      {
          CLI_write (gCLI.cfg.cliPrompt);  // "mmwDemo:/>" 프롬프트 출력
  
          /* Reset the command string: */
          memset ((void *)&cmdString[0], 0, sizeof(cmdString));
  
          /* Read the command message from the UART: */
          UART_read (gCLI.cfg.cliUartHandle, &cmdString[0], (sizeof(cmdString) - 1));
  
          /* Reset all the tokenized arguments: */
          memset ((void *)&tokenizedArgs, 0, sizeof(tokenizedArgs));
          argIndex      = 0;
          ptrCLICommand = (char*)&cmdString[0];
  
          /* comment lines found - ignore the whole line*/
          if (cmdString[0]=='%') {
              CLI_write ("Skipped\n");
              continue;
          }
  
          /* Set the CLI status: */
          cliStatus = -1;
  
          /* The command has been entered we now tokenize the command message */
          while (1)
          {
              /* Tokenize the arguments: */
              tokenizedArgs[argIndex] = strtok(ptrCLICommand, delimitter);
              if (tokenizedArgs[argIndex] == NULL)
                  break;
  
              /* Increment the argument index: */
              argIndex++;
              if (argIndex >= CLI_MAX_ARGS)
                  break;
  
              /* Reset the command string */
              ptrCLICommand = NULL;
          }
  
          /* Were we able to tokenize the CLI command? */
          if (argIndex == 0)
              continue;
  
          /* Cycle through all the registered CLI commands: */
          for (index = 0; index < gCLI.numCLICommands; index++)
          {
              ptrCLICommandEntry = &gCLI.cfg.tableEntry[index];
  
              /* Do we have a match? */
              if (strcmp(ptrCLICommandEntry->cmd, tokenizedArgs[0]) == 0)
              {
                  /* YES: Pass this to the CLI registered function */
                  cliStatus = ptrCLICommandEntry->cmdHandlerFxn (argIndex, tokenizedArgs);
                  if (cliStatus == 0)
                  {
                      CLI_write ("Done\n");
                  }
                  else
                  {
                      CLI_write ("Error %d\n", cliStatus);
                  }
                  break;
              }
          }
  
          /* Did we get a matching CLI command? */
          if (index == gCLI.numCLICommands)
          {
              /* NO matching command found. Is the mmWave extension enabled? */
              if (gCLI.cfg.enableMMWaveExtension == 1U)
              {
                  /* Yes: Pass this to the mmWave extension handler */
                  cliStatus = CLI_MMWaveExtensionHandler (argIndex, tokenizedArgs);
              }
  
              /* Was the CLI command found? */
              if (cliStatus == -1)
              {
                  /* No: The command was still not found */
                  CLI_write ("'%s' is not recognized as a CLI command\n", tokenizedArgs[0]);
              }
          }
      }
  }
  ```  
