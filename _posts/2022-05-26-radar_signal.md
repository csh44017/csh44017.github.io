---
title:  "레이더 신호 이해하기"
excerpt: "신호 관련 개념 정리"

categories:
  - Radar
tags:
  - [Radar, Signal]

toc: true
toc_sticky: true

math: true

published: true

date: 2022-05-26
last_modified_at: 2023-09-24
---

- 참고 자료  
[(복소 신호)IQ signal](https://blog.naver.com/rlaghlfh/221112341508){:target="_blank"}  

## 레이더 시그널  
레이더 안테나에서 수신한 신호가 ADC를 거치면 우리는 90도의 위상차를 가진 I데이터와 Q데이터를 얻을 수 있다.  
<div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/329fb359-7bb1-4277-b26e-00df3e5b9045" width="400" height="200">  
</div>  
여기서 말하는 IQ 데이터가 무엇인지 이해하기 위해 참고자료를 바탕으로 필요한 배경지식을 정리해보려고 한다.  
<br><br>  


### 복소 신호(I/Q Signal, Complex Signal, Quadrature Signal)  
**특정 시간에서의 값을 하나의 복소수로 표현할 수 있는 2차원 신호**로 I와 Q 2개로 이루어져 있다.  
- I (In-phase)  
  실수부, Real part  
- Q (Ouadrature-phase)  
  복소부, Imaginary part  

복소신호는 전기장과 자기장이 서로 수직하게 전파되는 전자기파와 같은 2차원 신호를 표현할 수 있다.  
(전기장과 자기장의 2가지 정보를 하나의 수로 동시에 표현 가능, $ cos(wt) + jsin(wt) $)  
<div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/731f9343-1a4f-4451-83ca-54b2fd061e61" width="300" height="200">  
</div>  
위의 그림처럼 시간의 흐름에 따라 2개 축에서 동시에 함수가 각각 뻗어 나간다.  
<br><br>  


## 복소수  
$ j^2 = -1 $ 로 정의되며,  
j가 4번 곱해질 때마다 원래의 값으로 돌아오는 주기성을 가지고 있다.  
(주파수를 지닌 파동이나 신호처럼 주기가 있는 것을 표현할 때 j를 사용 가능)  
<div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/131ef847-310e-49cd-9119-409d89c2c1c9" width="500" height="200">  
</div>  
복소수는 1차원 직선에 존재하는 것이 아니라 임의의 허수축을 추가하여 2차원으로 확장한 복소평면 내에 존재한다.  
<br><br>  


### 오일러 공식(Euler's formula)  
복소수 지수를 정의하는 데에서 출발해서 삼각함수와 지수함수에 대한 관계를 나타내는 공식이다.  
<div align=center>
<blockquote><p>

$$ e^{jϕ} = cos(ϕ) + jsin(ϕ) $$  
</p></blockquote></div>  

위 식을 그림으로 나타내면 다음과 같이 이해할 수 있다.
<div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/9fbae511-9e84-4796-8692-ad23bc9e1d74" width="350" height="350">  
</div>  

(시간에 따라 원으로 회전하는 복소수의 허수 축 크기를 그리면 $sin$함수가 그려지고, 실수 축 크기를 그리면 $cos$함수가 그려진다.)  
j의 곱은 phasor form에서 보았을 때 $ e^{jπ/2} $ 를 곱하는 것이므로 반시계방향으로 $ ϕ $에 해당하는 90도씩 회전시킨다.  
<br><br>  

### 복소수의 성질  
- **Time domain**에서 관점  
  2차원의 복소평면에 그린 특정 시점의 복소수를  
  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/ec074b98-dc23-4ea8-9a01-6ca743fced90" width="400" height="200">  
  </div>  
  시간축을 추가해 시간의 흐름에 따라 그려보면 다음 그림과 같이 이해할 수 있다.  
  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/ce8ebce9-d1d5-4cbc-b6b9-d1658c9123f8" width="400" height="200">  
  </div>  

  복소수가 $f_0$의 주파수로 시간 t의 흐름에 따라 반시계 방향으로 회전하는 형태로 그려진다.  
  <br>
  여기에서 특정 시점 $t$에  
  반대방향으로 동일하게 회전한 두 phasor를 더하면 cos의 실수부만 남고  
  <div align=center>
  <blockquote><p>

  $ e^{j2πf_0t} + e^{-j2πf_0t} = 2cos(2πf_0t) $  
  </p></blockquote></div>  
  반대방향으로 동일하게 회전한 두 phasor를 뺴면 sin의 허수부만 남는다.  
  <div align=center>
  <blockquote><p>

  $ e^{j2πf_0t} - e^{-j2πf_0t} = 2jsin(2πf_0t) $  
  </p></blockquote></div>  
<br>  

- **Frequency domain**에서 관점  
  우리가 평소에 보는 $cos(2πf_0t)$와 $sin(2πf_0t)$을 시간 축에 그린다면  
  허수 $j$를 고려하지 않기 때문에 시간에 따라 회전하는 형태의 변화가 실수 성분만 관측되어 그려지지만,  
  주파수 축에서 생각해보면 각각 양과 음의 $f_0$주파수에 해당하는 절반 크기의 임펄스(impulse) 2개로 나타난다.  
  (이로 인해 frequency domain에서 다양한 관점의 해석이 가능해진다.)  
  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/ce3f1726-b600-488f-9625-eb734ba0d7a4" width="400" height="200">  
  </div>  

  $cos(2πf_0t)$은 실수 축에 2개의 임펄스가 좌우대칭으로, $sin(2πf_0t)$은 허수 축에 2개의 임펄스가 원점대칭으로 보인다.  
  
  이를 바탕으로 오일러 공식에서 보았던 $cos(2πf_0t) + jsin(2πf_0t)$를 만들어주기 위해 ($ϕ$대신 $2πf_0t$ 사용) $sin$에 $j$를 곱하고 $cos$과 더하면,  
  $sin$의 임펄스가 허수축에서 실수축으로 90도 회전하기 때문에 음의 주파수 쪽은 상쇄되고 양의 주파수 쪽은 1/2로 작아졌던 임펄스 2개가 더해진다.  
  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/aeadc560-5d33-4978-a30a-5a7c14b77538" width="400" height="200">  
  </div>  

  결과적으로 원래 크기의 실수 임펄스 1개로 합쳐져 주파수 영역에서 $e^{j2πf_0t}$를 나타내는 것을 볼 수 있다.  

## 복소 신호 샘플링  

$j$의 곱처럼 90도의 변화가 아닌 다른 크기의 위상(phase)의 변화 $ϕ$가 있다면  
$cos(2πf_0t+ϕ)$는 $\frac{1}{2}\cdot(e^{j2πf_0t+ϕ} + e^{-j(2πf_0t+ϕ)})$