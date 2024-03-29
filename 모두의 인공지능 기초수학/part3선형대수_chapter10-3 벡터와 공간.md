# 벡터의 내적과 외적
__벡터의 덧셈__ : 각 성분을 더하는 것
![[Pasted image 20221020160119.png]]

__평행사변형 법칙__
- a와 b 두 벡터의 시작점이 일치할 때, a와 b의 평행사변형의 대각선은 두 벡터의 합을 의미한다.
![[Pasted image 20221020160213.png]]
$\overrightarrow a=(5, -2) \, , \overrightarrow b=(-3, 3)$
$\overrightarrow a+\overrightarrow b=(2, 1)$

__삼각형 법칙__
a의 끝점과 b의 시작점을 연결할 때 그 시작점과 끝점을 연결한 벡터는 두 벡터의 합을 의미한다.
![[Pasted image 20221020160516.png]]
### 벡터 덧셈에 대해 성질
1. 교환법칙 : A + B = B + A
2. 결합법칙 : (A+B) + C = A + (B + C)
3. 벡터 덧셈의 항등원이 존재함 : A + 0 = A
4. 벡터 덧셈의 역원이 존재함 : A + (-A) = 0
	- 역원 연산 결과로 항등원을 만드는 원소

__벡터의 덧셈 연습문제__
![[Pasted image 20221020160807.png]]
같은 원소끼리 더해주면 됨 => (-6, 13)

### 벡터의 뺄셈
- 각 성분을 빼는 것
![[Pasted image 20221020160838.png]]

1. 두 벡터의 시작점을 같게 함
2. 빼는 벡터의 끝점을 시작점으로하고, 빼지는 벡터의 끝점을 끝점으로 하는 벡터를 그리면 a-b의 벡터가 된다.

혹은 A-B = A+(-1)B로 계산할 수도 있다.

__벡터의 뺄셈 연습문제__
![[Pasted image 20221020160957.png]]
=> (2, 11)

### 벡터의 곱셈
- 벡터의 곱은 크게 내적과 외적으로 구분
- 스칼라 곱셈 : 방향은 그대로 이면서 크기를 키우는 것
	- ![[Pasted image 20221020161112.png]]
	- ![[Pasted image 20221020161149.png]]
	- 벡터의 크기를 키우거나 줄이는 역할
- __스칼라 곱셈의 성질
	- 결합벅칙
	- 분배법칙
	- 항등원이 존재

__벡터의 내적(inner porduct)__ : 벡터공간에서 정의된 이중 선형함수의 일종을 inner product 또는 dot product라고 부른다.
![[Pasted image 20221020161325.png]]
예시
$$\begin{bmatrix}6&6\end{bmatrix}\times \begin{bmatrix}12\\0\end{bmatrix} = 72 $$

__벡터 내적의 조건__
- x의 차원과 y의 차원이 같아야함
- 앞의 벡터가 행벡터이고, 뒤의 벡터가 열벡터여야함
- 두 벡터 x, y의 내적 결과는 스칼라이다.
![[Pasted image 20221020161517.png]]
__벡터 내적의 성질__
k = 스칼라
![[Pasted image 20221020161650.png]]
__벡터 내적의 연습문제__
$$\begin{bmatrix}-3&4&7\end{bmatrix}\begin{bmatrix}-4\\-9\\5\end{bmatrix} = 11$$
### 벡터의 외적
3차원 공간에서 벡터들 간의 연산 중 하나이다. 벡터들 간 연산의 결과이기 때문에 벡터곱(vector product)이라고 한다. 

__벡터의 외적 구하기__ 
![[Pasted image 20221020161951.png]]
![[Pasted image 20221020162028.png]]
크로스로 곱하고 빼주면 됨
![[Pasted image 20221020162210.png]]

__벡터 외적의 성질__
![[Pasted image 20221020162226.png]]
- 교환법칙 성립 X

__벡터의 외적 연습문제__
![[Pasted image 20221020162354.png]]
= [94, 54, 29]

__직교벡터__ : 두 벡터 사이의 각도가 90도를 이루는 것
![[Pasted image 20221020162433.png]]

__벡터의 크기__ : 벡터의 시작점과 끝점의 거리
![[Pasted image 20221020162530.png]]
__벡터의 거리/유사도__ : 두 벡터간의 거리
1. 유클리드 거리
	- 두 벡터 사이의 직선 거리(가장 빠른 거리)
	- $d(x, y)=\sqrt{(x_{1}+y_{1})^{2}+(x_{2}+y_{2})^{2}}$
	- 
2. 맨하튼 거리
	- 사각형 격자로 이뤄진 지도에서 출발점에서 도착점 까지를 가로 지르지 않고 갈 수 있는 최단거리를 구하는 공식
	- $d(x,y) = |x_{1}-x_{2}| + |y_{1}-y_{2}|$
	-![[Pasted image 20221020163036.png]] 
3. 코사인 거리
	- 코사인 유사도 : 두 벡터의 방향이 비슷할 수록 벡터가 비슷하다고 간주하여 두 벡터 사이의 각인 코사인 값을 코사인 유사도라고 한다
	- ![[Pasted image 20221020163242.png]]
	- ![[Pasted image 20221020163248.png]]
	- 



