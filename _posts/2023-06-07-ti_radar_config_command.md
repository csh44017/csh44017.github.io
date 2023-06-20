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
last_modified_at: 2023-06-21
---

- MMWAVE SDK User Guide  
  https://nbviewer.org/github/csh44017/csh44017.github.io/blob/master/_posts/paper/mmwave_sdk_user_guide.pdf  
- 3D_people_counting_detection_layer_tuning_guide  
  https://nbviewer.org/github/csh44017/csh44017.github.io/blob/master/_posts/paper/3D_people_counting_detection_layer_tuning_guide.pdf  
- 참고자료  
  https://nbviewer.org/github/csh44017/csh44017.github.io/blob/master/_posts/paper/RADAR%20BASED%20OBJECT%20DETECTION%20AND%20TRACKING%20FOR%20CRITICAL%20INFRASTRUCTURE.pdf  

## TI radar Configuration Command 정리  
TI의 레이더 소스코드를 보면 사용자가 환경에 따라 변경할만한 파라메타들을 튜닝해서 사용할 수 있도록 cli를 통해 레이더의 동작에 관련된 설정들을 변경할 수 있도록 구현해 놓았다.  

<div align="center">
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/a6c3bdf0-a5a0-4b1c-8d59-dd0029622598">  
</div>
일부 파라메타 값을 변경하기 위해서는 위의 chirp diagram에 대한 이해가 필요하다.  
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

    | Value | Description |
    | ----- | ----------- |
    | 50    | 50us에 걸쳐 주파수 증가 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |

  - txOutPower  
    TX 안테나의 송신 출력 파워 차단 코드  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **mmW demo에서는 0만 테스트된 상태** |

  - txPhaseShifter  
    TX 안테나의 송신 위상 시프터  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **mmW demo에서는 0만 테스트된 상태** |

  - freqSlopeConst  
    Ramp 형태의 **chirp에 대한 MHz/us 단위의 주파수 기울기** (float로 작성 가능)  
    chirp 다이어그램의 관계에 따라 다른 profile 파라메타들과 상호의존적으로 설정해야 한다.  

    | Value | Description |
    | ----- | ----------- |
    | 55.27 | **chirp의 기울기를 55.27로 설정** (0보다 큰 값을 사용) |

  - txStartTime  
    주파수가 증가하기 시작하는 RampStart를 기준으로 얼만큼의 us 시간 이전에 TX를 시작할 것인지 설정 (float로 작성 가능)  
    chirp 다이어그램의 관계에 따라 다른 profile 파라메타들과 상호의존적으로 설정해야 한다.  

    | Value | Description |
    | ----- | ----------- |
    | 1     | 주파수를 증가하기 1us 이전에 TX 안테나에서 송신 시작 |

  - numAdcSamples  
    RampStart와 RampEnd 사이에 있는 StartADCSampling부터 EndADCSampling까지에 해당하는 **ADCSamplingTime 동안 수집할 ADC 샘플의 개수 설정**  
    **(numAdcSamples / digOutSampleRate) = ADCSamplingTime**  

    | Value  | Description |
    | ------ | ----------- |
    | **64** | **64개의 ADC 샘플을 수집** (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |
    | 256   | 256개의 ADC 샘플을 수집 (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |

  - digOutSampleRate  
    ksps(1초당 1000개의 샘플) 단위의 **ADC 샘플링 주파수 설정**  
    **(numAdcSamples / digOutSampleRate) = ADCSamplingTime**  

    | Value       | Description |
    | ----------- | ----------- |
    | **2000.00** | **2MHz의 샘플링 주파수 사용** (chirp 다이어그램의 관계에 따라 어떤 값이든 사용 가능) |

  - hpfCornerFreq1  
    레이더 신호에서 원치 않는 저주파나 클러터를 제거하기 위한 **HPF1의 코너 주파수 설정**  

    | Value | Description |
    | ----- | ----------- |
    | 0     | HPF1의 코너 주파수를 175kHz로 설정 |
    | 1     | HPF1의 코너 주파수를 235kHz로 설정 |
    | **2** | **HPF1의 코너 주파수를 350kHz로 설정** |
    | 3     | HPF1의 코너 주파수를 700kHz로 설정 |

  - hpfCornerFreq2  
    레이더 신호에서 원치 않는 저주파나 클러터를 제거하기 위한 **HPF2의 코너 주파수 설정**  

    | Value | Description |
    | ----- | ----------- |
    | 0     | HPF2의 코너 주파수를 350kHz로 설정 |
    | **1** | **HPF2의 코너 주파수를 700kHz로 설정** |
    | 2     | HPF2의 코너 주파수를 1.4MHz로 설정 |
    | 3     | HPF2의 코너 주파수를 2.8MHz로 설정 |

  - rxGain  
    수신된 레이더 신호를 증폭할 dB 단위의 RX 이득 값  

    | Value | Description |
    | ----- | ----------- |
    | 36    | RX 이득을 36dB로 설정 |    

- chirpCfg  
  chirp의 구성 설정  
  (필수로 입력해야 하는 command이며, sensorStop과 sensorStart 사이에서 값을 갱신 가능)  

  - chirpStartIdx  
    chirp의 시작 인덱스  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **TX0을 위한 설정** |
    | **1** | **TX1을 위한 설정** |
    | **2** | **TX2을 위한 설정** |

  - chirpEndIdx  
    chirp 끝 인덱스  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **TX0을 위한 설정** |
    | **1** | **TX1을 위한 설정** |
    | **2** | **TX2을 위한 설정** |

  - profiled  
    profile 식별자  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **항상 0으로 사용**(profileCfg.profileId에서 정의하여 하나의 프로파일만 사용) |

  - startFreqVar  
    Hz 단위의 시작 주파수  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **0으로 사용** (profileCfg에서 설정하는 것을 추천) |

  - freqSlopeVar  
    kHz/us 단위의 주파수 기울기  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **0으로 사용** (profileCfg에서 설정하는 것을 추천) |

  - idleTimeVar  
    us 단위의 유휴 시간  

    | Value | Description |
    | ----- | ----------- |
    | **0** | **0으로 사용** (profileCfg에서 설정하는 것을 추천) |

  - adcStartTimeVar  
    us 단위의 ADC 시작 시간

    | Value | Description |
    | ----- | ----------- |
    | **0** | **0으로 사용** (profileCfg에서 설정하는 것을 추천) |

  - txEnableMask  
    TX 안테나의 enable 마스크  
    개별적인 chirp들은 각각 오직 하나의 TX 안테나에 대해 활성화되어야 한다. (TDM-MIMO 모드)  

    | Value | Description |
    | ----- | ----------- |
    | **1** | **0b001로 마스킹되어 TX0을 활성화** |
    | **2** | **0b010으로 마스킹되어 TX1을 활성화** |
    | **4** | **0b100으로 마스킹되어 TX2를 활성화** |

- lowPower  
  






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
