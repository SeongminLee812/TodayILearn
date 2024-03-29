# 컨볼루션 신경망(Convolution Neural Networks, CNN)

-   완전 연결 네트워크의 문제점으로부터 시작
    -   매개변수의 폭발적인 증가(모든 노드가 연결되어 있기 때문)
    -   공간 추론의 부족: 픽셀 사이의 근접성 개념이 완전 연결 계층(Fully-Connected Layer)에서는 손실됨
-   동물의 시각피질(visual cortex, 視覺皮質)의 구조에서 영감을 받아 만들어진 딥러닝 신경망 모델
    
-   __시각피질의 신경세포__
    
    -   시야 내의 특정 영역에 대한 자극만 수용
    -   수용장(receptive field, 受容場)
    -   해당 영역의 특정 특징에 대해서만 반응
-   시각 자극이 1차 시각피질을 통해서 처리된 다음, 2차 시각피질을 경유하여, 3차 시각피질 등 여러 영역을 통과하여 **계층적인 정보**처리
    
-   정보가 계층적으로 처리되어 가면서 점차 추상적인 특징이 추출되어 시각 인식
    
-   동물의 계층적 특징 추출과 시각인식 체계를 참조하여 만들어진 모델
    
    -   전반부 : 컨볼루션 연산을 수행하여 특징 추출
    -   후반부 : 특징을 이용하여 분류
-   영상분류, 문자 인식 등 인식문제에 높은 성능
    
    ![](https://upload.wikimedia.org/wikipedia/commons/6/63/Typical_cnn.png)



# 컨볼루션 연산 (Convolution Operation)

-   필터(filter) 연산
    
    -   입력 데이터에 필터를 통한 어떠한 연산을 진행
    -   필터에 대응하는 원소끼리 곱하고, 그 합을 구함
    -   연산이 완료된 결과 데이터를 **특징 맵(feature map)**이라 부름
    
-   필터(filter)
    -   커널(kernel)이라고도 함
    -   이미지 처리에서 사용하는 '이미지 필터'와 비슷한 개념
    -   필터의 사이즈는 거의 항상 홀수
        -   짝수이면 패딩이 비대칭이 되어버림
        -   왼쪽, 오른쪽을 다르게 주어야함
        -   중심위치가 존재, 즉 구별된 하나의 픽셀(중심 픽셀)이 존재
    -   필터의 학습 파라미터 개수는 입력 데이터의 크기와 상관없이 일정
    -   과적합을 방지할 수 있음
    
    ![](https://theano-pymc.readthedocs.io/en/latest/_images/numerical_padding_strides.gif)
-   연산 시각화
    
    ![](https://www.researchgate.net/profile/Ihab_S_Mohamed/publication/324165524/figure/fig3/AS:611103423860736@1522709818959/An-example-of-convolution-operation-in-2D-2.png)
-   일반적으로, 합성곱 연산을 한 후의 데이터 사이즈
    
    (𝑛−𝑓+1)×(𝑛−𝑓+1)
    
    𝑛: 입력 데이터의 크기  
    𝑓: 필터(커널)의 크기
    
    ![](https://miro.medium.com/max/1400/1*Fw-ehcNBR9byHtho-Rxbtw.gif)
-   위 예에서 입력 데이터 크기(𝑛)는 5, 필터의 크기(𝑘)는 3이므로 출력 데이터의 크기는 (5−3+1)=3



# 패딩(Padding)과 스트라이드(Stride)

-   필터(커널) 사이즈과 함께 입력 이미지와 출력 이미지의 사이즈를 결정하기 위해 사용
-   사용자가 결정할 수 있음

---

## 패딩(Padding)

-   입력 데이터의 주변을 특정 값으로 채우는 기법
    
    -   주로 0으로 많이 채움
    
    ![](https://miro.medium.com/max/395/1*1okwhewf5KCtIPaFib4XaA.gif)
-   출력 데이터의 크기
    
    (𝑛+2𝑝−𝑓+1)×(𝑛+2𝑝−𝑓+1)
    
    -   위 그림에서, 입력 데이터의 크기(𝑛)는 5, 필터의 크기(𝑓)는 3, 패딩값(𝑝)은 1이므로 출력 데이터의 크기는 (5+2×1−3+1)=5
### filters의 parameter
-   `valid`
    -   패딩을 주지 않음
    -   `padding=0`은 0으로 채워진 테두리가 아니라 패딩을 주지 않는다는 의미
-   `same`
    -   패딩을 주어 입력 이미지의 크기와 연산 후의 이미지 크기를 같도록 유지
    -   만약, 필터(커널)의 크기가 𝑘 이면, 패딩의 크기는 𝑝=(𝑘−1) / 2 (단, stride=1)

---

## 스트라이드(Stride)

-   필터를 적용하는 간격을 의미
    
-   아래 예제 그림은 간격이 2
    
    ![](https://miro.medium.com/max/294/1*BMngs93_rm2_BpJFH2mS0Q.gif)
    

---

## 출력 데이터의 크기

![[Pasted image 20220827211851.png]]

-   입력 크기 : (𝐻,𝑊)
-   필터 크기 : (𝐹𝐻,𝐹𝑊)
-   출력 크기 : (𝑂𝐻,𝑂𝑊)
-   패딩, 스트라이드 : 𝑃,𝑆

-   위 식의 값에서 𝐻+2𝑃−𝐹𝐻𝑆 또는 𝑊+2𝑃−𝐹𝑊𝑆가 정수로 나누어 떨어지는 값이어야 함
-   정수로 나누어 떨어지지 않으면, 패딩, 스트라이드 값을 조정하여 정수로 나누어 떨어지게 해야함


# 풀링(Pooling)

-   필터(커널) 사이즈 내에서 특정 값을 추출하는 과정

## 맥스 풀링(Max Pooling)

-   가장 많이 사용되는 방법
-   출력 데이터의 사이즈 계산은 컨볼루션 연산과 동일
![[Pasted image 20220827212048.png]]
-   일반적으로 stride=2, kernel_size=2 를 통해 특징맵의 크기를 절반으로 줄이는 역할
-   모델이 물체의 주요한 특징을 학습할 수 있도록 해주며, 컨볼루션 신경망이 이동 불변성 특성을 가지게 해줌
-   예를 들어, 아래의 그림에서 초록색 사각형 안에 있는 2와 8의 위치를 바꾼다해도 맥스 풀링 연산은 8을 추출
-   모델의 파라미터 개수를 줄여주고, 연산 속도를 빠르게 함
    
    ![](https://cs231n.github.io/assets/cnn/maxpool.jpeg)

## 평균 풀링(Avg Pooling)

-   필터 내의 있는 픽셀값의 평균을 구하는 과정
-   과거에 많이 사용, 요즘은 잘 사용되지 않음
-   맥스풀링과 마찬가지로 stride=2, kernel_size=2 를 통해 특징 맵의 사이즈를 줄이는 역할
    
    ![](https://www.researchgate.net/profile/Juan_Pedro_Dominguez-Morales/publication/329885401/figure/fig21/AS:707709083062277@1545742402308/Average-pooling-example.png)


## 전역 평균 풀링(Global Avg Pooling)

-   특징 맵 각각의 평균값을 출력하는 것이므로, 특성맵에 있는 대부분의 정보를 잃음
-   출력층에는 유용할 수 있음


# 완전 연결 계층(Fully-Connected Layer)

-   입력으로 받은 텐서를 1차원으로 평면화(flatten) 함
-   밀집 계층(Dense Layer)라고도 함
-   일반적으로 분류기로서 네트워크의 마지막 계층에서 사용


# 유효 수용 영역(ERF, Effective Receptive Field)

-   입력 이미지에서 거리가 먼 요소를 상호 참조하여 결합하여 네트워크 능력에 영향을 줌
    
-   입력 이미지의 영역을 정의해 주어진 계층을 위한 뉴런의 활성화에 영향을 미침
    
-   한 계층의 필터 크기나 윈도우 크기로 불리기 때문에 RF(receptive field, 수용 영역)이라는 용어를 흔히 볼 수 있음
    
    ![](https://wiki.math.uwaterloo.ca/statwiki/images/8/8c/understanding_ERF_fig0.png)
-   RF의 중앙에 위치한 픽셀은 주변에 있는 픽셀보다 더 높은 가중치를 가짐
    
    -   중앙부에 위치한 픽셀은 여러 개의 계층을 전파한 값
    -   중앙부에 있는 픽셀은 주변에 위치한 픽셀보다 더 많은 정보를 가짐
-   가우시안 분포를 따름
    
    ![](https://www.researchgate.net/publication/316950618/figure/fig4/AS:495826810007552@1495225731123/The-receptive-field-of-each-convolution-layer-with-a-3-3-kernel-The-green-area-marks.png)



## Fashion MNIST

![](https://www.tensorflow.org/tutorials/keras/classification_files/output_oZTImqg_CaW1_0.png?hl=ko)

```
import datetime
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as pyplot
plt.style.use('seaborn')
  

from tensorflow.keras import Model
from tensorflow.keras.models import Sequential
from tensorflow.keras.utils import to_categorical
from tensorflow.keras.datasets.fashion_mnist import load_data
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow.keras.layers import Dense, Flatten, Conv2D, MaxPool2D, Dropout, Input
```


