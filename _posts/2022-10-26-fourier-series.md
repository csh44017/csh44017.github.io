---
title:  "푸리에 급수(Fourier Series)"
excerpt: "푸리에 급수 정리"

categories:
  - SignalProcessing
tags:
  - [FourierSeries]
use_math: true

toc: true
toc_sticky: true
 
date: 2022-10-26
last_modified_at: 2022-10-26
---

## 주기 함수  
시간 주기(period, T) 마다 반복하는 함수  
<br>같은 형태를 반복하는 주기를 가진 파동은 아무리 복잡한 파형이라도 $sin$, $cos$ 함수의 결합으로 표현할 수 있다.  
<blockquote><p> $$\begin{aligned}
f(t)=a_0&+a_1\cdot cos(wt)+a_1\cdot cos(wt)+\cdots+a_n\cdot cos(nwt) \\
&+b_1\cdot sin(wt)+b_2\cdot sin(2wt)+\cdots+b_n\cdot sin(nwt)
\end{aligned}$$</p></blockquote>  

$a_0$는 0이 중심이 아닌 파동을 표현해주며 $cos(0)$는 1이고 $sin(0)$는 0이기 때문에 $b_0$는 생략한다.  
복잡한 파형이 주기를 가진 파동이기 때문에 이를 만족하려면 단순한 파동들은 모두 기본 주파수의 정수배여야 한다.  
<blockquote><p> $$f(t)=a_0+\displaystyle\sum_{n=0}^{\infty}{a_n\cdot cos(nwt)+b_n\cdot sin(nwt)}$$</p></blockquote>  

$a_n$과 $b_n$은 파동이 얼마나 들어있는가를 의미한다.  
각 성분이 얼마나 들어있는지 함수의 직교성을 통해 구할 수 있다.  
함수의 직교성은 내적을 이용  

<div align=center>
<blockquote><p> $(f, g) = \displaystyle\int_{a}^{b} f(x)\cdot g(x)dx = 0$ 일 경우,
<br>구간 $[a,b]$에서 $f(x)$와 $g(x)$는 orthogonal
</p></blockquote></div>  
