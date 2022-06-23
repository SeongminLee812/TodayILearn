# Pandas



- Pandas 설치하기

  ```python
  !pip install pandas
  #anaconda 모듈에서는 자동으로 설치되어있음
  ```

- Pandas 자료구조

  - 서로 다른 형식을 갖는 여러 종류의 데이터를 컴퓨터가 이해할 수 있도록 동일한 형식을 갖는 구조로 통합할 필요가 있음
  - 판다스는 시리즈(Series)와 데이터프레임(DataFrame)이라는 데이터 형식을 제공
  - 

## 시리즈(Series)

### 시리즈(Series)

- 데이터가 순차적으로 나열된 1차원 배열
- 1차원 배열
  - 가로줄 - 행, 세로줄 - 열
  - 4 * 1 행렬은 시리즈
  - 4 * 2 ~ 행렬은 데이터 프레임
  - 1 * 2 행렬은 데이터 프레임
- 인덱스(index)와 데이터 값(value)이 일대일 대응
- 시리즈의 인덱스는 데이터 값의 위치를 나타내는 이름표 역할
- 딕셔너리로 만들 수 있는 자료구조
- index : value로 통하기 위한 목차 같은 개념
- value : 실제 값
- 해당하는 index에 맞춰서 value 들이 계속 들어감. → 그 커진 덩어리를 데이터 프레임이라고 부름
- 열이 2개 이상부터 → 데이터 프레임(2차원배열)
- ![image-20220623185438476](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220623185438476.png)

![image-20220623185510418](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220623185510418.png)

### 시리즈 생성

- 판다스 내장 함수인 Series() 사용

```python
import pandas as pd
dict_data = {'a' : 1, 'b': 2, 'c' : 3}
sr = pd.Series(dict_data)
print(sr)
print(type(sr))

#a    1
#b    2
#c    3
#dtype: int64  #자료형이 정수형이다
#<class 'pandas.core.series.Series'>
```

- 인덱스 → 자신과 짝을 이루는 데이터 값의 순서와 주소를 저장 위 코드의 ‘a’, ‘b’, ‘c’가 인덱스임

```python
import pandas as pd
list_data = ['2022-05-23', 3.14, '이성민', 100, True ]
sr = pd.Series(list_data)
print(sr)
print(type(sr))

#0    2022-05-23
#1          3.14
#2           이성민
#3           100
#4          True
#dtype: object #자료형이 문자열(혼합되어있는 경우 문자열로 표기)
#<class 'pandas.core.series.Series'>
```

### Index

- 리스트로 시리즈를 만들면 각각의 위치값이 데이터의 인덱스가 된다.
- 0~4 :  정수형 위치 인덱스
- object : 문자열
- 인덱스 종류
  1. 정수형 위치 인덱스
  2. 인덱스 이름

- 시리즈의 인덱스와 데이터를 따로 확인

  ```python
  #import pandas as pd
  
  list_data = ['2022-05-23', 3.14, '이성민', 100, True]
  sr = pd.Series(list_data)
  idx = sr.index
  val = sr.values
  
  print(idx)
  print(val)
  
  #RangeIndex(start=0, stop=5, step=1) 0으로 시작해서 5로 끝나고 1씩 증가
  #['2022-05-23' 3.14 '이성민' 100 True]
  print(type(val))
  #<class 'numpy.ndarray'>
  
  
  ```

- 인덱스 만들기

  - Series함수를 사용하면서 두번째 인자로 인덱스 이름을 지정해 줄 수 있다

  ```python
  import pandas as pd
  tup_data = ('이성민', '2022-05-23', 'Male', True) #튜플도 시리즈 만들수있다
  sr = pd.Series(tup_data, index = ['이름', '오늘날짜', '성별', '학생여부'])
  print(sr)
  print(type(sr))
  #이름             이성민
  #오늘날짜    2022-05-23
  #성별            Male
  #학생여부          True
  #dtype: object
  #<class 'pandas.core.series.Series'>
  
  tup_data = ('이성민', '2022-05-23', 'Male', True)
  sr = pd.Series(tup_data, ['이름', '오늘날짜', '성별', '학생여부'])
  print(sr)
  print(type(sr))
  #두번째 인자에 index 변수 지정 안해도 된다.
  ```

- 리스트 원소 선택

  인덱스 이름을 사용한 시리즈라고 하더라도, 정수형 위치 인덱스도 사용 가능하다.

  - 1번방법 →시리즈명[위치] →시리즈명[인덱스이름]

  ```python
  print(sr[0]) #정수형 위치 인덱스
  #이성민
  print(sr['이름']) #인덱스 이름
  #이성민
  ```

  - 2번방법(다중선택) → 다중으로 묶이는 경우 괄호를 두번 써줘야 한다. 컬럼과 구분해줘야함

  ```python
  print(sr[[1, 2]])
  #오늘날짜    2022-05-23
  #성별            Male
  #dtype: object
  
  print(sr[['오늘날짜', '성별']])
  #오늘날짜    2022-05-23
  #성별            Male
  #dtype: object
  ```

  - 3번방법(다중선택, 정수형 위치 인덱스) → 슬라이싱 이용

    - 시리즈명[처음위치:마지막위치] → 슬라이싱이기에 마지막 위치는 포함되지 않는다.

    ```python
    print(sr[1:3])
    #오늘날짜    2022-05-23
    #성별            Male
    #dtype: object
    ```

    - 시리즈명[’인덱스명’:’인덱스명’] → 인덱스 이름을 이용하면 범위의 끝을 포함한다

    ```python
    print(sr['오늘날짜':'성별'])
    #오늘날짜    2022-05-23
    #성별            Male
    #dtype: object
    ```



```python

```