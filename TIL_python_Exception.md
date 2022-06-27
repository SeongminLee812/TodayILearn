# Exception

- 오류의 종류

  - IndentationError →  들여쓰기 에러, 들여쓰기 할 때는 공백 4칸 이나 tap을 혼용해서 쓰면 안되고 한가지만 써야함

  - **`IndexError**: list index out of range` → 리스트 범위 밖의 인덱싱을 했기 때문

  - SyntaxError

    - EOL : 따옴표, 괄호의 짝이 맞지 않을 때
    - invalid syntax : in뒤에는 연산자가 아니라 리스트 같은 변수나 묶음자료형이 와야함
    - can’t assign to literal : 변수 이름은 숫자로 시작 불가

  - NameError : 없는 변수 호출 혹은 변수 이름 틀림

  - RecursionError : 함수안에서 자기 자신을 재 호출하는 재귀함수의 종료 조건을 설정하지 않아, 계속 자기 자신을 호출하다 종료

    ```python
    def hello():
        print("Hello")
        hello()
    ```

  - AttributeError(속성) : 리스트에 없는 속성을 호출

  - ValueError : 함수 안에 실행인자를 잘못 입력한 경우 → int(’hello’)

  - TypeError : 데이터 타입이 잘못된 경우 → abs() 함수 안에 문자열 자료형이 들어간 경우

  - KeyError : 딕셔너리에 없는 키를 호출하려 했을 때

  - ImportError : 모듈이름을 잘못 입력했거나 해당 모듈이 존재하지 않는 경우

- 파이썬은 오류가 발생하면 프로그램을 중단하고 오류 메세지를 보여줌

- 오류를 무시하고 프로그램을 정상적으로 실행 → 예외처리

- **try, except문**

  - try 블록 수행 중 오류가 발생하면 except 블록 수행

  - 형식

    ```python
    try:
    	정상실행할 문장
    except:
    	에러 발생시 실행할 문장
    ```

    ```python
    a = 5
    b = 0
    try :
        c = a / b
        print(c)
    except:
        print('error')
    #>>>error
    #오류 문장 이름 생략 시 전체 에러를 잡아냄
    #except뒤에 오류 문장 지정 시 특정 에러만 잡아냄
    a = 5
    b = 0
    try:
        c = a / b
        print(c)
    except ZeroDivisionError:
        print('error')
    ```

  - 여러에러 잡아내기 가능 → except 추가 사용

  ```python
  a = 5
  b = 0
  try:
      c = a / b
      print(c)
  except ZeroDivisionError:
      print('error')
  except NameError:
      print('error2')
  ```

  - 두가지 예외를 한번에 묶어서 처리하기 → 괄호와 콤마로 에러문장 묶기

  ```python
  a = 5
  b = 0
  try:
      c = a / b
      print(c)
  except (ZeroDivisionError, NameError):
      print('error')
  ```

  - 에러의 상세 내용 출력하기

  ```python
  try:
      spam()
  except NameError as x:
      print(x)
  #>>>name 'spam' is not defined
  #x는 except구문에서만 사용가능한 임의의 변수
  ```

- **try ~ except ~ finally구문**

  - 무조건 실행하고 싶은 문장이 있는 경우 try 구문에도 문장을 추가해줘야 하고, except구문에도 문장을 추가해줘야함 → 문장이 길어짐

  ```python
  try:
      spam()
      print('hello')
  except NameError as x:
      print('hello')
      print(x)
  ```

  - finally구문은 무조건 실행할 문장을 넣어주면 됨

  ```python
  try:
      spam()
  except NameError as x:
      print(x)
  finally:
      print('hello')
  #>>>name 'spam' is not defined
  #hello
  
  #finally 구문은 마지막에 실행된다.
  ```

- **예외 직접만들기**

  ```python
  class MyError(Exception):
      pass
  
  def say_nick(nick):
      if nick == '바보':
          raise MyError()
      print(nick)
  say_nick('천사')
  say_nick('바보')
  
  #천사
  #-----------------------
  #MyError
  ```

  ```python
  class MyError(Exception):
      def __str__(self):
          return "허용되지 않는 별명입니다"
  
  def say_nick(nick):
      if nick == '바보':
          raise MyError()
      print(nick)
  #>>>MyError: 허용되지 않는 별명입니다.
  #Exception -> 내장되어있는 클라스
  ```

  ```python
  
  ```