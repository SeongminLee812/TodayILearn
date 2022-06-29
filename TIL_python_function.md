# function

- 반복적인 사용을 위해 함수 생성

- 코드를 분리시켜 놓고 싶을 때

- 함수를 정의할 때의 키워드는 def

- return 사용과 미사용의 차이점

  ## **함수 만들기**

  - 형식

    - def *함수명*(*매개변수1, 매개변수2*): *수식 or 기타 코드*

    - 함수 예시

      → 함수에 의해서 연산은 일어남

      return 없이 함수를 정의하면 함수를 호출한 측에 아무 값도 전달하지 않음

    ```python
    def add(a, b):
        a+b
    
    print(add(1, 2))
    #>>>None
    #위 함수는 print나 return이 없기에 연산만 일어나고 값을 출력하지 않는다.
    ```

    - return을 사용해야 호출한 측에 그 값을 전달해준다.

    ```python
    def add(a, b):
        print(a+b)
    
    add(1, 2)
    #>>>3
    print(add(1, 2))
    #>>>None
    #함수 안에서 print가 동작되긴하지만 호출한 쪽에 전달해주지는 않는다
    
    def add(a, b):
        return a+b
    ```

    - return 사용시 값만 전달
    - 전달받은 값을 보기 위해서 print()구문 사용해야함.

- ## **이중 함수**

  - a와 b를 더해주고 더해준 것을 절대값으로 변경해주는 함수
  - 정의한 함수를 다른 함수에서 사용 가능

  ```python
  def add(a, b):
      return a+b
  
  def addabs(a, b):
      c = add(a, b)
      return abs(c) #abs() -> 절대값으로 변경해주는 함수, 음수 -> 양수로
  print(addabs(-3, -7))
  #>>>10
  
  ```

- ## **return 문에 여러 값을 사용할 경우**

  - swap 함수는 a. b 매개 변수를 받는 함수
  - 값을 반환할 때 b, a 순서로 반환되도록 하는 함수

  ```python
  def swap(a, b):
      return b, a
  print(swap(10, 20))
  #>>>(20, 10)
  ```

  - return 문에 여러 값을 사용할 경우 → 튜플로 구성하여 전달

- ## **함수의 종류**

  - 더하기 계산을 출력해주는 add 함수 생성

  - 입력값이 있고 결과값이 있는 함수

  - 입력값이 있고 결과값이 없는 함수

    ```python
    def add(a, b):
        print(f'{a}와 {b}의 합은 {(a+b)}입니다.')
    add(3, 4)
    #>>>3와 4의 합은 7입니다.
    
    print(add(a, b))
    #>>>None
    ```

  - 입력값이 없고 결과값이 있는 함수

    - 매개변수를 받지 않는다 + return

    ```python
    #받는 매개변수는 없고 결과는 'hi'인 함수 say 생성
    def say():
        return 'hi'
    print(say())
    #>>>hi
    ```

  - 입력값이 없고 결과값이 없는 함수

    ```python
    def say():
    	print('hi')
    say()
    #>>>hi
    
    print(say())
    #hi
    #>>>None
    ```

- ## **함수의 인수(인자)**

  - 매개변수 : 함수를 만들 때 전달 받을 값

  - 인자 : 함수를 호출할 때 전달할 값

  - 인수의 기본값

    - 함수를 호출할때 인자를 넘겨주지 않아도 자동으로 기본 값을 취하도록 하는 기능

    ```python
    for i in range(1, 10): 
        print(i, end =' ')
    #>>>1 2 3 4 5 6 7 8 9
    ```

    ```python
    for i in range(10, 1, -1):
        print(i, end = ' ')
    #>>>10 9 8 7 6 5 4 3 2
    ```

    - 원래 range(1, 10, 1)으로 1이 생략된 것임(인수의 기본 값)

    ```python
    #매개변수 a, b=1을 갖고 a와 b를 더한 결과 값을 갖는 incrise 함수 생성
    def incrise(a, b=1):
        return a + b
    
    incrise(1)
    #>>>2
    incrise(1, 8)
    #>>>9
    
    #인자를 하나만 정해주면 기본으로 b = 1을 가짐
    #인자가 정해지면 기본값은 무시됨
    ```

    - 함수를 지정할 때 매개변수를 기본값으로 설정하기 위해서는 뒷부분부터 써줘야한다.

    ```python
    def add(a, b=1, c =2):
        print('a와 b와 c의 합은 %d입니다.' % (a + b + c))
    ```

- ## **키워드 인수**

  - 인수 이름으로 값을 전달하는 방식

  ```python
  def area(weight, width):
      return weight * width
  
  print(area(width = 20, weight = 10))
  #>>>200                                                                         
  #인자에 직접 인수이름으로 지정가능
  #키워드 인수를 사용하면 위치를 교차해서 사용할 수 있다.
  ```

  - 키워드 인수 또한 뒷 부분에 지정해줘야 한다(keyword argument 에러)

  ```python
  print(area(20, width = 5))
  #>>>100
  
  print(area(width = 5, 20))
  #>>>SyntaxError: positional argument follows keyword argument
  ```

- ## **가변 길이 인수 리스트(\*매개변수)**

  - 언패킹과 비슷한 개념
  - 고정되지 않은 수의 인수를 함수에 전달하는 방식

  ```python
  def varilen(a, *b):
      print(a, b)
  
  varilen(1) #*b는 나머지라는 개념으로 나머지가 없으니 빈 튜플로 print 됨
  #>>>1 ()
  varilen(1, 2)
  #>>>1 (2,)
  varilen(1, 2, 3)
  #>>>1 (2, 3)
  varilen(1, 2, 3, 4)
  #>>>1 (2, 3, 4)
  varilen(1, 2, 3, 4, 5)
  #>>>1 (2, 3, 4, 5)
  ```

- ## **\*\*매개변수**

  - 사전 자료형과 관련이 있음
  - 함수 호출에 사용하는 인수들이 사정에 있으면 **을 이용하여 함수를 호출할 수 있음

  ```python
  dicargs = {'a':1,'b':2, 'c':3, 'f':4}
  def h(a, b, c, f):
      print(a, b, c, f)
  
  h(**dicargs)
  #>>>1 2 3 4
  ```

- ## **변수의 종류(유효 범위)**

  - 변수가 함수 내에서 정의되면 지역변수
  - 변수가 함수 밖에서 정의되면 전역변수
  - 호출 순서(우선순위)
    - 지역변수 → 전역변수

  ```python
  str = 'global variable'
  
  def func_1():
      str = 'local variable'
      print(str)
  
  func_1()
  #>>>local variable
  
  #def func_1()함수 내에 str변수가 없다면 global이 호출됬을 거임
  ```

  > 지역변수 개념은 함수 내부에서만 통용되는 개념이고, 반복문 안에서 선언한 변수는 전부 전역변수가 됨

- ## **함수 안에서 함수 밖의 변수를 변경하는 방법**

  - ### 첫번째, return 사용

  ```python
  a = 1
  def vartest(a):
      a = a + 1
      return a
  
  a = vartest(a)
  print(a)
  #>>>a
  
  #a의 값이 2로 바뀜
  ```

  - ### 두번째, global 사용

  ```python
  a = 1
  def vartest():
      global a #전역변수를 데려옴
      a = a + 1 
  
  vartest()
  print(a)
  #>>>2
  
  #a라는 전역변수
  #global a는 전역변수 a를 함수 안으로 데려온 것임
  #글로벌 스타 초청 후 키워서 내보냄
  ```

  - global은 간단하게 쓸 수 있지만, 자칫하면 원본 전역변수가 훼손될 수 있다. 함수가 많아질 수록 부작용이 있음.

- ## **lambda함수**

  - 간단한 함수를 쉽게 선언하는 방법
  - *함수 이름* = lambda <매개변수>:<리턴값>

  ```python
  def add(a, b):
      return a + b
  
  add_2 = lambda a, b : a + b
  
  print(add(2, 3))
  print(add_2(2, 3))
  #>>>5
  #>>>5
  ```

  - lambda를 사용하면 결과값을 반환하는 함수로 정의됨 (=return 사용과 같음)
  - 람다를 사용하면 리스트 안에 함수를 정의할 수 있다

  ```python
  myList = [lambda a, b : a + b, lambda a, b : a * b]
  
  print(myList[0](3, 4))
  #>>>7
  
  print(myList[1](3, 4))
  #>>>12
  
  #리스트 요소로써 함수가 저장된다
  print(type(myList[0]))
  #>>><class 'function'>
  ```

  - lambda를 이용해 리스트에 자주 쓰는 연산 등 함수를 넣어서 필요할 때마다 꺼내쓸 수 있다.