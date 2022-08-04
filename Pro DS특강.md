# Pro DS 특강

# 1.  데이터 가져오기

```python
import pandas as pd

# data1 = pd.read_csv('Dataset/Dataset_01.csv', dtype='object)
data1 = pd.read_csv('Dataset/Dataset_01.csv')
## parameter
# na_values => 값 지정해주면 다 결측치로 받아들임
# dtype => 데이터 형 지정, 간혹 제일 앞에 0이있는 경우 숫자가 사라지기 때문에 object로 읽으면 해결

data1.info()

type(data1)
data1.columns
# ['TV', 'Radio', 'Social_Media', 'Influencer', 'Sales']
data1.index
data1.values # 컬럼명, 인덱스 제외한 값만 가져옴 array구조로 -> reshape 함수를 사용가능하다
data1.values.reshape(-1, ) # 1차원 벡터 구조로 변환
```

- read_csv
    - parameter
    - na_values => 값 지정해주면 다 결측치로 받아들임
    - dtype => 데이터 형 지정, 간혹 제일 앞에 0이있는 경우 숫자가 사라지기 때문에 object로 읽으면 해결
    

# 2. 결측값 처리

- **nan** vs **null**
    - nan과 null은 셀 단위에서 같은 것으로 표현할 수 있다
    - nan은 벡터 입장에서 셀 안에서 [1, 3, nan] 한 값이 비었다는 표현으로 사용 가능하고,
    - null은 셀 안에서 []로 사용된 경우 -> 전체 다 비었다.

> 결측치는 반드시 체크해야 하는데, 파이썬의 구조 때문이다. sklearn등 패키지는 벡터구조로 다루기 때문
> 
> 
> 시계열 데이터 -> 주변의 데이터를 확인해서 대표값으로 결측치 처리하는 경우가 많다
> 
> 시계열데이터가 아닌 경우 -> 데이터의 특징이 반영되어야함
> 

```python
data1.isna()
# boolean 형태로 데이터 프레임으로 반환됨 (True:1, False:0)
data1.isna().sum() # Series 형태
data1.isna().sum().sum() # 숫자의 합계도 구함.

# 결측치가 포함된 행 리턴
    # 불린 인덱싱 array인 경우는 True인 위치를 다 가져오지만, 
mask = data1.isna().any(axis=1) # any 매서드는 적어도 한개가 True이면 그 열을 하나로 만들어서 열단위로 뽑아줌
data1[mask]

# 결측치 제거 .dropna(how=, subset=[]), 
    # 분산이 0인 값 -> 평균과 같다 -> y값에 영향을 미치지 않으므로 불필요한 피쳐
    # droplevel -> 지정한 인덱스 제거

# 결측치 보정 .fillna(), 결측치 위치를 이용해서 결측치가 포함된 데이터 위치 찾아서 변경
    # .groupby(), .apply(), lambda x:

# groupby로 묶어서 결측치 처리 가능
na_data = data1.groupby('Influencer')['TV', 'Radio', 'Social_Media'].apply(lambda x: x.fillna(x.mean()))
# fillna 안에 {'TV':5, 'Radio':7} 식으로 딕셔너리 형태로 넣어주면 각 컬럼에 따라 결측치에 대체값을 넣어준다.

na_data[data1.TV.isna()]

# sklearn.impute -> 머신러닝 결측치 보정 알고리즘
```

- groupby 함수를 이용, 그룹별 대표값으로 결측치 지정 가능하다.
- 결측치 제거 .dropna(how=, subset=[])
    - 분산이 0인 값 -> 평균과 같다 -> y값에 영향을 미치지 않으므로 불필요한 피쳐
    droplevel -> 지정한 인덱스 제거
- 결측치 보정 .fillna(),
    - 결측치 위치를 이용해서 결측치가 포함된 데이터 위치 찾아서 변경
        # .groupby(), .apply(), lambda x:
- sklearn.impute -> 머신러닝 결측치 보정 알고리즘

### 이상치

- 이상치는 중심축을 변화시키기 때문에 보정해줘야한다.
- 이상치를 찾는 방법
- IQR사용 IQR = Q3-Q1
- # Q1-1.5*IQR < < Q3+1.5*IQR 범위를 벗어나는 값들을 이상치처리
# 데이터 안에 그룹의 특징이 존재하는 경우에는 boxplot만 보면 안되고 그룹의 특징을 고려해야함.

# 3. 상관관계

```python
import seaborn as sns

sns.pairplot(data1)

data1.corr()
# 데이터프레임 구조로 리턴되기 때문에 데이터프레임 메서드들을 이용하면 된다.
corr_df = data1.corr()['Sales'].drop('Sales')
corr_df.abs().max() # 절대값 -> 최대값

corr_df[corr_df.abs() >= 0.6] # 특정기준 목록 추출 가능(0.6이상이면 상관관계를 강하다 판단)
corr_df[corr_df.abs() >= 0.6].index # 선형관계가 높은 변수 목록

corr_df.abs().idxmax() # -> 최대값 인덱스명 리턴
corr_df.abs().argmax() # -> 정수형 위치인덱스로 리턴
corr_df.abs().nlargest(2) # -> 가장 큰 2가지 반환 .index 붙이면 인덱스 명만 추출

print('정답 :', round(corr_df.abs().max(), 4))
# round 상수(스칼라)만 가능, 벡터인 경우 numpy의 round함수를 쓰거나 반복적용해줘야함

# 참고
import numpy as np
np.ceil(1.23456 * 1000) / 1000 # 올림, 셋째자리에서 반올림하기 위해서는 곱셈과 나눗셈을 해줘야함
np.floor(1.76543 * 1000) / 1000 # 내림
```

- 독립변수가 많지 않은 경우 pairplot으로 시각화해서 확인하기

![Untitled](Pro%20DS%20%E1%84%90%E1%85%B3%E1%86%A8%E1%84%80%E1%85%A1%E1%86%BC%208fee1f0469c9485faf7cfd4c4b58d2b4/Untitled.png)

```python
data1.corr()

                    TV     Radio  Social_Media     Sales
TV            1.000000  0.869460      0.528168  0.999497
Radio         0.869460  1.000000      0.607452  0.869105
Social_Media  0.528168  0.607452      1.000000  0.528906
Sales         0.999497  0.869105      0.528906  1.000000
```

```python
corr_df[corr_df.abs() >= 0.6] # 특정기준 목록 추출 가능(0.6이상이면 상관관계를 강하다 판단)

TV       0.999497
Radio    0.869105
Name: Sales, dtype: float64
```

# 4. 회귀분석

```python
from sklearn.linear_model import LinearRegression, SGDRegressor

q3 = data1.dropna()

var_list = list(data1.columns.drop(['Sales', 'Influencer']))
lm = LinearRegression().fit(q3[var_list], q3['Sales']) # 결측치 존재 오류 -> dropna()로 해결
# fit_intercept : 절편을 표현할 것 인지 (상수값을 어떻게 할 것인지) ->True(default)인 경우 절편포함
lm2 = LinearRegression().fit(q3['TV'], q3['Sales']) # 차원 오류

q3['TV'].ndim
q3['TV'].shape # => 1차원 이므로 reshape을 활용해 2차원으로 만들어줌
# .values로 array로 가져와야함
lm3 = LinearRegression().fit(q3['TV'].values.reshape(-1, 1), q3['Sales'])

# 혹은 괄호를 한번 더씌워서 각 행들을 벡터로 리턴시켜주는 방법도 있음
lm2 = LinearRegression().fit(q3[['TV']], q3['Sales']) 

# object 구조 확인 함수
dir(lm)

lm.coef_ # 가중치, 회귀계수?
lm.intercept_ # 절편 값 확인

q3_coef = pd.Series(lm.coef_, index=var_list)
q3_coef.sort_values(ascending=False)
np.trunc(q3_coef.sort_values(ascending=False)*1000) / 1000 # 반올림

from statsmodels.formula.api import ols
# ols(식, 데이터셋).fit()
# lm4=ols()
# lm5=lm4.fit()

# lm5=ols().fit()
# 식 : 문자열로 표현 'Sales ~ TV + Radio + C(범주형) - 1' => y ~ x변수들을 더하기로 나열.
# 범주형은 자동으로 인코딩, - 변수는 제외함

lm4=ols('Sales~TV+Radio+Social_Media', data1).fit()

form1='Sales~' + '+'.join(var_list)
form1

lm5=ols(form1, data1).fit()

dir(lm5)

lm5.summary()
```

- 차원개수 오류 : reshape함수로 해결
- ols : ‘(y) ~ (X1)+(X2)’ 의 방법으로 다중회귀방정식 표현

## 분포

- 데이터에 대한 특징을 본다
    - 데이터의 중심으로부터 얼마나 흩어져있는 지
- 목적, 대상