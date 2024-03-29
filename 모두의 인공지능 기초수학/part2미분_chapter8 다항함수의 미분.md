# 평균변화율
__평균변화율__ : x가 변하는 양에 대해 y가 얼마나 변하는지에 대한 변화율
$\frac{y의증가량}{x의증가량} = \frac{\triangle y}{\triangle x}$ 
![[Pasted image 20221016154557.png]]
$f(x) = x^2$일 때, x가 1에서 5로 변할때 평균 변화율
$\frac{f(5)-f(1)}{5-1} = \frac{25-1}{4}=6$ 

평균변화율의 또다른 표현 : $\triangle$x를 h로 치환
![[Pasted image 20221016154850.png]]

b=a+h (a + 증가량)

### 평균변화율의 기하학적 의미
![[Pasted image 20221016155158.png]]

# 미분계수(=순간 변화율)
미분계수는 x의 증가량이 0으로 갈 떄의 평균변화율이다.

![[Pasted image 20221016155413.png]]

__미분계수__ : 평균변화율에서 x증가량 -> 0일 때의 극한 값을 x=a에서의 미분계수 또는 순간변화율이라고 한다.
![[Pasted image 20221016155700.png]]

__미분계수의 기하학적 의미__
![[Pasted image 20221016155911.png]]
- 접선 : 원 끝에 닿는 선


__연습문제__
![[Pasted image 20221016161453.png]]
평균변화율 : $\frac{f(5)-f(1)}{5-1} = \frac{40-4}{5-1} = \frac{36}{4} = 9$
순간변화율 : 
1. 순간변화율 공식
2. a에 k를 대입
$$
\begin{align*} 
f`(a)=\lim_{h\to 0}\frac{f(a+h)-f(a)}{h}\\
\lim_{h\to0}\frac{\{(k+h)^{2} +3(k+h)-(k^{2}+3k)\}}{h}
\end{align*}$$
![[Pasted image 20221016162229.png]]

# 도함수
__도함수__ : 함수 f(x)를 미분하여 얻은 함수 f`(x)
	즉, 순간변화율 or f(x) 미분의 결과

$f(x)=x^2+x$에서 $f(1), f(2), f(3)$ 을 구해보자
- 식을 세번 써야해서 번거로움 -> 도함수 사용![[Pasted image 20221016162714.png]]

![[Pasted image 20221016162646.png]]
- 매번 변하는 수 1, 2 등을 x로 치환

__연습문제__
![[Pasted image 20221016162811.png]]
 도함수 : 4x
 x=6일때 미분계수 : 24

# 함수의 연속과 미분가능성
### 함수의 미분 가능성
__함수의 미분가능성__ : 어떤 함수가 x=a에서 미분이 가능하다는 의미는 x=a에서 미분계수가 존재한다는 의미

![[Pasted image 20221016163445.png]]
미분 가능성과 연속성의 관계
미분가능성$\subset$함수의 연속성
1. 미분 가능하면 연속
2. 연속이라고 해서 항상 미분 가능한 것은 아니다

![[Pasted image 20221016163607.png]]

__연속이지만 미분 불가능한 경우__
![[Pasted image 20221016163750.png]]
![[Pasted image 20221016163920.png]]

__미분가능성 연습문제__
함수 $f(x)=x^2$이 x=2에서 미분가능한가?
1. 미분계수 존재 확인
	- 미분계수 공식 : $\displaystyle\lim_{h \to0}\frac{f(a+h)-f(a)}{h}$에서 함수 대입 및 x값 대입
![[Pasted image 20221016164456.png]]
x=2일때 미분계수 : 4 =>미분가능!


### 다항함수의 미분법
![[Pasted image 20221016164530.png]]
(1) f(x)=c이면 f'(x)=0이다. (단c는 상수)
상수 c를 미분하면 0이다.

(2) $f(x)=x^n이면 f`(x)=nx^{n-1}$이다
	- 예를 들어 $y=x^2$을 미분하면 $y`=2\times x^{2-1}=2x$가 된다

(3) ${cf(x)}`=cf`(x)$이다 (단, c는 상수)
	- 상수 (c * 함수)의 미분은 c * (함수의 미분)과 같음
	- 예를 들어 $y=5x^{3}$을 미분하면 $y`=5\times 3x^{3-1}$(2번공식이용)이 되어 $y`=15x^2$가 된다.

(4) $\{f(x)\pm g(x)\}` = f`(x)\pm g`(x)$ 
	 예를 들어, $y=5x^{2}+2x$를 미분해보자
	 그대로 미분 = 각자 미분
	 - $5x^2$미분
		 - $5(2\times x^{2-1}) = 5(2x) = 10x$
	 - $2x$미분
		 - $2x=> 2x^{1-1}=2$
		결론 : $10x +2$

 (5) $\{f(x)g(x)\}`=f`(x)g(x)+f(x)g`(x)$이다. 
	 예를 들어, $y=(4x^{2}+2)(2x-3)$ 을 미분해보자
		 - $f(x)= (4x^{2}+2),\, g(x)=(2x-3)$이다.
	 $y`=(8x)(2x-3) + (4x^{2}+2)(2)$

(6) 분수에 대한 미분공식
	![[Pasted image 20221016170818.png]]
