# Numpy

- Numpy 설치하기

  ```python
  !pip install numpy
  #anaconda 모듈에서는 자동으로 설치되어있음
  ```

- Numpy의 특징

  - Numerical Python의 약자
  - 파이썬 기반 수치 해석 라이브러리
  - 선형대수 연산에 필요한 다차원 배열과 배열 연산을 수행하는 다양한 함수를 제공
  - 산술 계산을 위한 패키지
    - 다차원 배열인 ndarray를 사용(빠른 배열 연산을 제공)
    - 반복문 작성할 필요 없이 전체 데이터 배열을 빠르게 계산 가능
  - 산술 처리에 필요한 여러 함수들을 제공
    - 연산 처리 후 통계나 분석 데이터를 처리하기 위해 Pandas를 사용

# 행과 열의 개념

# ![image-20220625115919990](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220625115919990.png)

- 배열(ndarray) 생성

  - numpy 모듈 불러오기 (as np)
  - arange(값) 함수 사용

  ```python
  import numpy as np
  my_arr = np.arange(10000)
  
  print(my_arr)
  #>>>[   0    1    2 ... 9997 9998 9999]
  ```

  - my_list의 요소값을 두배로 하는 코드

    ```python
    my_list2 = []
    for i in my_list:
        my_list2.append(i * 2)
    
    #그러나 이러한 반복문은 연산이 길어지는 단점이 있다.
    ```

  - ## my_arr의 요소값을 두배로 하는 코드

    ```python
    my_arr2 = my_arr * 2
    print(my_arr2)
    
    #>>>[    0     2     4 ... 19994 19996 19998]
    ```

- 리스트로 배열만들기

  - array() 함수 사용 → 인자로 list를 넣어주면됨

  ```python
  Array1 = np.array([1, 2, 3])
  print(Array1)
  
  #>>>[1 2 3]
  ```

  - 2차원 배열은 두개의 1차원 배열을 이용해 만들어 주는 것이고, 괄호를 한번 더 묶어줘야 한다. → 2차원 배열은 행렬구조임

  ```python
  Array2 = np.array([[1, 2, 3], 
                    [4, 5, 6]])
  print(Array2)
  print(type(Array2))
  
  [[1 2 3]
   [4 5 6]]
  <class 'numpy.ndarray'>
  ```

  <aside> 💡 arange함수 → 간단한 배열 만드는 경우 array함수 → 자료형을 변환(리스트 → 배열)

  </aside>

- 행, 열 개수 구하기

  - 행 개수 구하기

    - len()함수

    ```python
    Array2 = np.array([[1, 2, 3], 
                      [4, 5, 6]])
    
    print(len(Array2))
    #>>>2
    #행이 2개다
    ```

  - 열 개수 구하기

    - len()함수 사용
    - 인자 → 배열이름[행 정수형위치인덱스]

    ```python
    print(len(Array2[0]))
    #>>>3
    #정수형 위치 인덱스 0행에 위치한 열이 3개다
    ```

- 3차원 배열 만들기

  ```python
  Array3 = np.array([[[1, 2, 3, 4],
                     [5, 6, 7, 8],
                     [9, 10, 11, 12]],#여기까지 2차원 배열 한개
                     [[13, 14, 15, 16],
                     [17, 18, 19, 20],
                     [21, 22, 23, 24]]]) #2차원 배열 하나 더 만듬
  #괄호 3개로 묶어줌
  ```

  ![image-20220625120100053](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220625120100053.png)

  - 앞에서 보면 2차원 배열이다

    

- 3차원 배열의 깊이, 행, 열

  - 깊이구하기

    - len() 함수 사용 → 인자 : 배열 이름

    ```python
    print(len(Array3))
    #>>>2
    ```

  - 행 구하기

    - len() 함수 사용 → 인자 : 배열 이름[2차원 배열의 정수형 위치인덱스]

  ```python
  print(len(Array3[0]))
  #>>>3
  #깊이를 기준으로 앞에서부터 0번
  ```

  - 열 구하기

    - len() 함수 사용 → 인자 : 배열 이름[2차원 배열의 정수형 위치인덱스]\[행 정수형 위치 인덱스]

    ```python
    print(len(Array3[0][0]))
    #>>>4
    ```

- 배열의 차원, 배열의 크기

  - 배열의 차원구하기

    - 배열이름.ndim

  - 배열의 크기 구하기

    - 배열이름.shape

    ```python
    print(Array1.ndim)
    print(Array1.shape)
    
    print(Array2.ndim)
    print(Array2.shape)
    
    print(Array3.ndim)
    print(Array3.shape)
    
    #1
    #(3,) #행은 어차피 하나기에 열의 개수만
    #2
    #(2, 3)  #행이 추가가 되는 거니 앞에 붙는다
    #3
    #(2, 3, 4) #깊이가 앞으로 온다.
    
    #Array1
    #[1 2 3]
    
    #Array2
    #[[1 2 3]
    # [4 5 6]]
    
    #Array3
    #[[[ 1  2  3  4]
    #  [ 5  6  7  8]
    #  [ 9 10 11 12]]
    
    # [[13 14 15 16]
    #  [17 18 19 20]
    #  [21 22 23 24]]]
    ```

- 배열 인덱싱

  - 리스트 인덱싱과 비슷한 방법

  ```python
  a = np.array([0, 1, 2, 3, 4])
  
  print(a[1])
  #>>>1
  print(a[2])
  #>>>2
  ```

  - 2차원 배열
    - 배열이름[행의 정수형위치인덱스,열의 정수형위치인덱스]

  ```python
  a = np.array([[0, 1, 2],
                [3, 4, 5]])
  
  print(a[0, 0])
  #>>>0
  print(a[1, 1]) #마찬가지로 0부터 시작한다
  #>>>4
  print(a[-1, -1])
  #>>>5
  #a[0][0]의 형태로도 가능하다!
  ```

  - 3차원 배열

    - 배열이름[깊이, 행의 정수형위치인덱스,열의 정수형위치인덱스]

    ```python
    print(Array3)
    print('='*20)
    print(Array3[1, 1, 2]) 
    #1번 깊이, 1번행, 2번열(인덱스는 0부터 시작)
    
    #[[[ 1  2  3  4]
    #  [ 5  6  7  8]
    #  [ 9 10 11 12]]
    
    # [[13 14 15 16]
    #  [17 18 19 20]
    #  [21 22 23 24]]]
    #====================
    #19
    ```

- 배열 슬라이싱

  - a 배열 생성

  ```python
  a = np.array([[0, 1, 2, 3],
              list(range(4, 8))]) #list(), range()함수 한번써봄
  
  print(a)
  #>>>[[0 1 2 3]
  # [4 5 6 7]]
  ```

  - 2차원 배열 슬라이싱(1차원 배열은 리스트와같다)
    - 배열이름[행 범위지정, 열 범위지정]

  ```python
  print(a[0, :]) #첫번째 행의 전체 열
  # >>>[0 1 2 3]
  
  print(a[:, 1]) #두번째 열 전체(행 전체중에 두번째열 다 뽑기)
  #>>>[1 5]
  
  print(a[:, :2])
  #>>>[[0 1]
  # [4 5]]
  ```

- 팬시 인덱싱

  - 인덱스 배열
  - 위치 정보를 가지고 다른 배열을 만들 수 있음
  - 해당 위치 배열을 인덱싱하여 배열의 값을 가져오는 것
  - 여러개의 값을 한번에 볼 수 있다.
  - 배열이름[[정수형 위치 인덱스, 정수형위치인덱스, …]]

  ```python
  a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
  
  print(a[[2, 4, 7]])
  #>>>[3 5 8]
  ```

  - 불 자료형을 이용해서 팬시 인덱싱으로 원하는 열만 뽑아내기!

  ```python
  b = np.array([True, False, True, False, True, False, True, False, True])
  
  print(a[[b]])
  #>>>[1 3 5 7 9]
  ```

  - 팬시인덱싱 응용

    ```python
    #a배열 생성
    a = np.array([[1, 2, 3, 4],
                  [5, 6, 7, 8],
                  [9, 10, 11, 12]])
    
    print(a[:2, [1, 3]])
    #>>[[2 4]
    # [6 8]]
    ```

    a[[행], [열]]

  - 팬시 인덱싱을 사용하여 배열바꾸기

    ```python
    b = a[[2, 0, 1], :]
    print(b)
    
    #>>[[ 9 10 11 12]
    # [ 1  2  3  4]
    # [ 5  6  7  8]]
    ```

- 불린 인덱싱 (bool자료형)

  - 필터링

  - 조건 필요

  - 배열이름[조건]

    - array1[array1<10]

    ```python
    #a 배열에서 5보다 큰 값을 출력
    a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
    
    print(a[a>5])
    #  [6 7 8 9]
    
    # 2차원배열
    b = np.array([[1, 2, 3, 4],
                  [5, 6, 7, 8],
                  [9, 10, 11, 12]])
    
    print(b[b>5])
    # [ 6  7  8  9 10 11 12]
    #1차원 배열로 출력된다. 
    #2차원 배열은 깨지기 때문
    ```

- 배열정렬

  - sort() 함수 사용
    - numpy.sort(배열이름)
    - 배열이름.sort()
    - 옵션
      - axis = 0 →행정렬, axis =1 → 열 정렬
  - numpy.sort()는  출력용

  ```python
  a = np.array([[4, 3, 5, 7],
                [1, 12, 11, 9],
                [2, 15, 1, 14]])
  
  print(np.sort(a, axis=0))
  #>>[[ 1  3  1  7]
  #   [ 2 12  5  9]
  #   [ 4 15 11 14]]
  #모든 행 값(열 내에서)이 오름차순으로 됨
  #작은거 1행으로 올리고 [밑 행으로 갈 수록 커진다]
  #세로줄에 맞춰서 정렬됨
  #위치가 다 바뀐다
  
  print(np.sort(a, axis = 1))
  # [[ 3  4  5  7]
  #  [ 1  9 11 12]
  #  [ 1  2 14 15]]
  #모든 열 값(해당 행 내에서)이 오름 차순으로 됨
  #가로줄에 맞춰서 정렬됨
  #뒷 열로 갈수록 커진다
  ```

  - 기존 값을 바꾸고 싶은 경우에는 배열이름.sort(옵션)

  ```python
  a.sort(axis = 1)
  
  print(a)
  
  #[[ 3  4  5  7]
  # [ 1  9 11 12]
  # [ 1  2 14 15]]
  ```

  - argsort() 함수는 순서만 알고 싶을 경우 사용

    ```python
    a = np.array([42, 38, 12, 26])
    print(a)
    print(np.argsort(a))
    
    #>>>[42 38 12 26]
    #   [2 3 1 0]
    # 2 3 1 0 이라고 나오는 이유는
    #위 a배열의 인덱스 순서에 해당하는 값을 
    #작은 숫자부터 배열한 것임
    # 2 = 12, 3 = 26, 1 = 38, = 24
    
    b = np.argsort(a)
    print(a[b]) #팬시 인덱싱 = a[[2, 3, 1, 0]]
    #>>>[12 26 38 42] 
    ```

- 배열 크기에 맞춰 배열 바꾸기

  - reshape(행개수, 열개수) 함수 사용

  ![image-20220625120449739](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220625120449739.png)

  ```python
  a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12])
  print(a)
  #[ 1  2  3  4  5  6  7  8  9 10 11 12]
  
  b = a.reshape(3, 4)
  print(b)
  #[[ 1  2  3  4]
  # [ 5  6  7  8]
  # [ 9 10 11 12]]
  ```

- 배열 연산

  - 행렬의 연산과 같다

  ```python
  a = np.array([[1, 2, 3],
               [4, 5, 6]])
  print(a)
  #[[1 2 3]
  # [4 5 6]]
  print(a + a)
  #[[ 2  4  6]
  # [ 8 10 12]]
  print(a * a)
  #[[ 1  4  9]
  # [16 25 36]]
  print(1 / a)
  #[[1.         0.5        0.33333333]
  # [0.25       0.2        0.16666667]]
  print(a >= a)
  #[[ True  True  True]
  # [ True  True  True]]
  
  #정수를 넣으면 각 값에 전부 연산된다
  ```

- 다차원 배열 객체 연산

  - 브로드 캐스팅 되어 연산됨

  ![image-20220625120517634](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220625120517634.png)

  ```python
  #1*3배열에서 5더하기
  a = np.arange(3)
  #[1 2 3]
  print(a+3)
  #>>>[3 4 5]
  ```

  - 3*3배열 + 1*3배열

  ```python
  print(np.ones((3, 3))) #1로 다 채운 3*3행렬
  #[[1. 1. 1.]
  # [1. 1. 1.]
  # [1. 1. 1.]]
  
  print(np.ones((3, 3)) + np.arange(3)) #[0 1 2]
  #[[1. 2. 3.]
  # [1. 2. 3.]
  # [1. 2. 3.]]
  ```

  ```python
  np.arange(3).reshape(3, 1) #reshape함수로 1*3을 3*1로 바꾸기
  #[1]
  #[2]
  #[3]
  
  print(np.arange(3).reshape(3, 1) +  np.arange(3))#reshape함수로 1*3을 3*1로 바꾸
  #[[0 1 2]
  # [1 2 3]
  # [2 3 4]]
  ```

- 전치행렬

  - 행과 열을 바꿈
    - numpy.transpose(배열이름)
    - 배열이름.T
    - 배열이름.transpose
  - 전치행렬로 a배열 행과 열 바꾸기

  ```python
  a = np.array([[1, 2, 3],
              [4, 5, 6]])
  print(a)
  #[[1 2 3]
  # [4 5 6]]
  
  b = a.transpose()
  print(b)
  #[[1 4]
  # [2 5]
  # [3 6]]
  
  #1차원 배열은 전치행렬이 안된다
  #기준점을 가지고 종이접기하듯이 접히는 것인데
  #1차원 배열은 안된다.
  ```

- 행열 곱

  - 조건
    - A 열의 개수 = B 행의 개수

  ![image-20220625120606164](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220625120606164.png)

  - 연산결과 = A 행의 개수 * B 열의 개수 행렬이 나옴
  - np.dot(A, B)

  ```python
  A = np.array([[1, 2, 3],
               [4, 5, 6]])
  B = np.array([[7, 8],
               [9, 10],
               [11, 12]])
  
  print(np.dot(A, B))
  
  #[[ 58  64]
  #[139 154]]
  ```