---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# 서울시 구별 CCTV 현황 분석하기

```python

```

서울시 각 구별 CCTV 수를 분석하고, 인구대비 CCTV 비율 파악해서 순서를 비교해보자
인구대비 CCTV의 평균치를 확인하고 CCTV가 과하게 부족한 구를 분석해보자
Pandas와 시각화 방법을 적용해보자

## 1. 필요한 라이브러리 import

```python
import pandas as pd
import numpy as np
```

## 2. 데이터 불러오기

```
CCTV_Seoul = pd.read_csv('./data/01.CCTV_in_Seoul.csv', encoding='utf-8')

```

```python
# 불러온 데이터를 간단하게 확인하기 - 상위 5개의 Row만 확인
CCTV_Seoul.head()
```

```python
# 데이터 프레임 컬럼명을 확인하기
CCTV_Seoul.columns
```

```python
CCTV_Seoul.columns[0]

```

- encoding 방식 -> utf-8, euc-kr, cp949

  ![image-20220711191419203](0711_데이터분석실전1_이성민.assets/image-20220711191419203.png)

# 구별로 CCTV가 어떻게 설치되어 있는가? 또는 구별 CCTV 설치 현황은?

```python
CCTV_Seoul.rename(columns = {CCTV_Seoul.columns[0]:'구별'}, inplace=True)
```

- inplace = True => 데이터 프레임에 바로 반영한다.
- rename(columns = 딕셔너리)형태로 이름 변경

```python
CCTV_Seoul.head()
```

![image-20220711191453978](0711_데이터분석실전1_이성민.assets/image-20220711191453978.png)

- 강남구의 집중적으로 2013년 이전에 설치되어 있는 것으로 보임
    - 이 때 강남구의 어떤 일이 있었는 지 찾아볼 수 있다.
- 2013년 이전 강남구의 투자배경 분석 필요 또는 주요 선거 및 사건 유무 확인 필요



# 데이터 불러오기 - 엑셀파일 : 서울시 인구 현황


## 서울의 인구는 어떻게 구성되어 있을까?

```python
# 인구 데이터 불러오기
pop_seoul = pd.read_excel('./data/01. population_in_Seoul.xls', 
                          header=2,
                         usecols = 'B, D, G, J, N')
```

### 잘라오기
- usecol은 해당하는 칼럼만 불러온다.
- 엑셀처럼 A, B, C 순으로 진행된다.

```python
#인구 데이터가 잘 불러와졌는 지 확인하기
pop_seoul.head()
```

```python
# 열의 이름 변경하기 - 각 열의 시작은 무엇부터? 0부터!

pop_seoul.rename(columns={pop_seoul.columns[0]: '구별',
                         pop_seoul.columns[1]: '인구수',
                         pop_seoul.columns[2]: '한국인',
                         pop_seoul.columns[3]: '외국인',
                         pop_seoul.columns[4]: '고령자'}, inplace=True)

pop_seoul.head()
```



![image-20220711191523820](0711_데이터분석실전1_이성민.assets/image-20220711191523820.png)

# CCTV 데이터 파악하기

```python
# 서울시내 구별 CCTV는 몇대가 설치되어 있을까?
# 서울시의 CCTV 설치에 구별 편향성이 존재하는 가?
```

```python
# 심플하게 CCTV 현황 살펴보자

CCTV_Seoul.head()
```

```python
#소계를 기준으로 올림차순 정렬
CCTV_Seoul.sort_values(by='소계', ascending=True).head()
```

```python
# 소계 기준으로 내림차순으로 정렬하기
CCTV_Seoul.sort_values(by='소계', ascending=False).tail(8)
```

- .head() 위에서 부터 개수만큼 출력(기본값은 5개)
- .tail() 아래에서 부터 개수만큼 출력(기본값은 5개)

```python
# 증가율(비율) 계산하기
# \ = 중바꿈
CCTV_Seoul['최근 증가율'] = (CCTV_Seoul['2014년']+CCTV_Seoul['2015년']+\
                            CCTV_Seoul['2016년'])/CCTV_Seoul['2013년도 이전'] * 100
```

```python
# 최근 증가율 기준으로 정렬

CCTV_Seoul.sort_values(by='최근 증가율', ascending=False).head(10)
```

![image-20220711191615102](0711_데이터분석실전1_이성민.assets/image-20220711191615102.png)

# 서울시 인구 데이터 파악해보기

```python
pop_seoul.head()
```

![image-20220711191648190](0711_데이터분석실전1_이성민.assets/image-20220711191648190.png)

-  불필요한 데이터를 제거 (합계가 리스트처럼 잡혀있을 필요가 없다)

```python
pop_seoul.drop([0, 1], axis=0, inplace=True)
```

```python
pop_seoul.head()
```

```python
pop_seoul['구별'].unique()
```

## 결측치 (nan, NaN)는 처리하자!
- isnull() 메서드는 결측치인지 아닌지 판단하여 불린형태로 반환

```python
# 결측치를 판단
pop_seoul[pop_seoul['구별'].isnull()]
```

![image-20220711191708009](0711_데이터분석실전1_이성민.assets/image-20220711191708009.png)

- 27번행에 다수의 결측치가 존재 => 결측치 처리 필요

```python
pop_seoul.drop([27], inplace=True)
```

```python
pop_seoul.tail()
```

```python
# 서울의 외국인의 비율이 어느정도인가?
# 서울의 고령자 비율은 어느정도인가?
```

```python
# 1. 외국인 비율 = 외국인/인구수 * 100
# 2. 고령자 비율 = 고령자/인구수 * 100

pop_seoul['외국인 비율'] = pop_seoul['외국인'] / pop_seoul['인구수'] * 100
pop_seoul['고령자 비율'] = pop_seoul['고령자'] / pop_seoul['인구수'] * 100
```

```python
pop_seoul.head()
```

```python
# 인구수, 외국인, 외국인 비율, 고령자, 고령자 비율 어디가 제일 Top?
# 기준 컬럼 by로 설정해서 정렬해보자
pop_seoul.sort_values(by='인구수', ascending=False).head(1)
```

```python
#외국인
pop_seoul.sort_values(by='외국인', ascending=False).head(1)
```

```python
#외국인 비율
pop_seoul.sort_values(by='외국인 비율', ascending=False).head(1)
```

```python
#고령자
pop_seoul.sort_values(by='고령자', ascending=False).head(1)
```

```python
#고령자비율
pop_seoul.sort_values(by='고령자 비율', ascending=False).head(1)
```

# Pandas 고급 기술 활용: dataFrame 병합하기


- concat, merge, join

```python
# merge 사용해서 두 데이터의 공통 컬럼을 찾아서 연결해보자!!!
# on = 공통컬럼명 (key) 사용

data_result = pd.merge(CCTV_Seoul, pop_seoul, on='구별')
```

```python
data_result.head()
```

![image-20220711191754247](0711_데이터분석실전1_이성민.assets/image-20220711191754247.png)

```python


# 의미없는 컬럼 버리기
# 이번에는 drop말고 del 코드 사용

del data_result['2013년도 이전']
```

```python
del data_result['2014년']
del data_result['2015년']
del data_result['2016년']
```

```python
data_result.head()
```

```python
data_result.set_index('구별', inplace=True)
```

```python
data_result
```

# 상관관계 분석기준
 1) 값이 0.1이면 거의 무시하자 <br>
 2) 값이 0.3이상이면 약한 상관관계가 존재한다.<br>
 3) 값이 0.7이상이면 뚜렷한 상관관계가 존재한다.<br>

```python
# 상관계수 (너와 나 = 얼마나 관련 있니?)
# AttributeError:'float' object has no attribute 'shape'에러가 발생했었으나
# dtype = float64 옵션을 넣어주니 해결되었다.
# dtype은 1.20 버전 이후 업데이트 된 것으로 아래 주석 참고
# https://numpy.org/doc/stable/reference/generated/numpy.corrcoef.html

np.corrcoef(data_result['고령자 비율'], data_result['소계'], dtype = 'float64')
```

```python
np.corrcoef(data_result['외국인 비율'], data_result['소계'], dtype = 'float64')
```

```python
np.corrcoef(data_result['인구수'], data_result['소계'], dtype = 'float64')
```

- 고령자 비율과 CCTV 개수의 상관관계는 음의 상관관계가 있는 경향이 있으나, <br>
외국인 비율과는 상관관계가 없고, <br>
인구수와는 약간의 상관관계가 있다.

```python
# 소계를 기준으로 정렬(내림차순)

data_result.sort_values(by='소계', ascending=False).head()
```

- CCTV가 많이 설치된 구와 인구수가 많은 구를 시각적으로 분석해 보는 것이 필요
<br>
(내가 필요할 것 같은 내용을 적어놓자)

```python
data_result.sort_values(by='인구수', ascending=False).head()
```

## CCTV와 인구현황 - 그래프 - 시각화 분석 적용하기


```python
# 시각화 분석을 위해서 해주던 셋팅

import matplotlib.pyplot as plt

```

```python
data_result.head()
```

### 한글폰트 적용법
1) Windows OS
1) import matplotlib.pyplot as plt 정의했을 경우

① 나눔고딕 폰트가 설치되어 있다면, <br>

plt.rcParams['font.family'] = 'NanumGothic'<br>
② 맑은고딕 폰트가 설치되어 있다면,<br>

plt.rcParams['font.family'] = 'Malgun Gothic'<br>
2) mpl로 정의했을 경우<br>

import matplotlib as mpl # 시각화모듈 시 사용<br>
import matplotlib.pyplot as plt #시각화 모듈 시 사용<br>
① 한줄로 정의<br>
<br>
mpl.rc('font',family= 'NanumGothic')<br>
② 경로 설정해서 두줄로 정의<br>
<br>
font_name =<br>
mpl.font_manager.FontProperties(fname='C:\Windows\Fonts\malgun.ttf').get_name()
mpl.rc('font',family=font_name)<br>

2) Mac OS<br>
① Apple Gothic 이 설치되어 있다면,<br>
<br>
plt.rcParams['font.family'] = 'AppleGothic'<br>



```python
plt.figure()

data_result['소계'].plot(kind='barh', grid=True, figsize=(6, 8))
plt.show()
```

```python
plt.rcParams['font.family'] = 'Malgun Gothic'
# 맑은 고딕은 기본으로 설치되어 있다.
```

### 꿀팁
- 함수의 괄호 안에서 shift+tab 누르면 파라미터 설명 볼 수 있다.

```python
# 크기가 큰 구간부터 나올 수 있도록 정렬 => sort_values() 사용
data_result['소계'].sort_values().plot(kind='barh', grid=True, figsize=(6, 8));
```

![image-20220711192143660](0711_데이터분석실전1_이성민.assets/image-20220711192143660.png)

- CCTV 개수는 강남구가 타 지역구보다 월등히 많이 설치되어 있음

- 상대적으로 양천구, 서초구, 은평구도 많은 CCTV가 설치되어 있음

    => 인구대비 CCTV 비율이 추가적으로 분석이 필요함(지리의 면적과의 관계도 분석 필요)



 jupyter notebook 내에서 바로 그래프 보기<br>
 `%matplotlib inline`<br><br>
 jupyter notebook 팝업으로 그래프보기<br>
 `%matplotlib qt`<br>

```python
#인구당 CCTV의 비율
data_result['CCTV 비율'] = data_result['소계'] / data_result['인구수'] * 100
```

```python
data_result['CCTV 비율'].sort_values().plot(kind='barh', grid=True, figsize=(8, 8));
# ;콜론을 문장 제일 뒤에 넣어주면 불필요하게 나오는 <AxesSubplot:ylabel='구별'>등을 지울 수 있다.
```

```python

```

![image-20220711192208770](0711_데이터분석실전1_이성민.assets/image-20220711192208770.png)
