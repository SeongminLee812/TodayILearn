# Class

- 클래스는 객체를 표현하기 위한 문법
- 파이썬에서는 객체라는 단위를 메모리 위에서 정보를 관리(메모리에 올린 것은 다 객체) → 함수도 객체임

- 클래스 안의 함수 => 메서드
- 과자틀 → 클래스(class) 과자틀에 의해 만든 과자 → 객체(object)



## **클래스를 왜 사용할까?**

```python
result = 0
def add(num):
    global result
    result = result + num
    return result

print(add(3))
#>>>3
print(add(4))
#>>>7

result2 = 0
def add2(num):
    global result2
    result2 = result2 + num
    return result2
```

- 위와 같이 함수를 만들어서 사용한 후 다른 값을 num에 대입시키고 싶을 때 매번 함수를 바꿔주기 번거로움.

```python
class Calculator:
    def __init__(self): # 객체를 생성하는 순간 동작하는 메서드
        self.result = 0 #result에 자동으로 0을 세팅함
    
    def add(self, num):
        self.result = num + self.result
        return self.result

cal1 = Calculator()
print(cal1.add(3))
#>>>3
print(cal1.add(4))
#>>>7

cal2 = Calculator()
print(cal2.add(5))
#>>>5
print(cal2.add(10))
#>>>15

del cal2
```

- 클래스를 사용하여 객체를 만들 경우 서로의 객체에 영향을 미치지 않음
- 코드가 짧아짐
- 사용하고 필요 없어진 객체는 del 코드로 지울 수 있음

## **클래스 용어 정의**

```python
class Cookie:
    pass

a = Cookie()
b = Cookie()
c = Cookie()
d = Cookie()
e = Cookie()
```

- a~e : 객체

- Cookie : 클래스

- a 객체는 Cookie에 의해 만들어졌다.

  - a 객체는 Cookie의 인스턴스

  

- **self**

  - 클래스 생성시 클래스를 저장할 변수(클래스 자신)

    

  

- **사칙연산클래스 생성**

  ```python
  class FourCal:
      def setdata(self, first, second):
          self.first = first
          self.second = second
  
  a = FourCal()
  a.setdata(4, 2)
  ```

  - self = 객체 자신
  - a가 4와 2를 인자로 가져간다.

  - 어떤 객체가 호출했는 지 알려주기 위함. → 인자들이 바로  a에 귀속됨

  ```python
  b = FourCal()
  FourCal.setdata(b, 4, 2)
  ```

  위 방법으로 클래스 생성시 self를 쓰지 않고 b객체를 만들때 self자리에 b를 넣어서 어떤 객체가 호출했는지 알릴수 있음

  

  ```python
  class FourCal:
      def setdata(self, first, second):
          self.first = first
          self.second = second
      
      def add(self):#값은 이미 setdata()에 의해 대입되었기 때문에 매개변수 지정할 필요 없이 연산만 하면 됨
          result = self.first + self.second
          return result
  
  a = FourCal()
  a.setdata(4, 2)
  print(a.add()) #setdata로 인수가 정해졌기 때문에 함수만 호출하면 됨.
  #>>>6
  ```

  - 사칙연산 다 넣기

  ```python
  class FourCal:
      def setdata(self, first, second):
          self.first = first
          self.second = second
      
      def add(self):#값은 이미 setdata()에 의해 대입되었기 때문에 매개변수 지정할 필요 없이 연산만 하면 됨
          result = self.first + self.second
          return result       
      
      def mul(self):
          result = self.first * self.second
          return result
      
      def sub(self):
          result = self.first - self.second
          return result     
      
      def div(self):
          result = self.first / self.second
          return result
  
  a = FourCal()
  a.setdata(4, 2)
  
  print(a.add())
  print(a.mul())
  print(a.sub())
  print(a.div())
  
  #>>>6
  #>>>8
  #>>>2
  #>>>2.0
  ```

- **생성자**

  - **init**
  - 객체를 만드는 순간 동작하는 메서드
  - →클래스를 선언하면 실행됨!

  ```python
  class FourCal:
      def __init__(self, first, second):
          self.first = first
          self.second = second
      
      def add(self):#값은 이미 setdata()에 의해 대입되었기 때문에 매개변수 지정할 필요 없이 연산만 하면 됨
          result = self.first + self.second
          return result       
      
      def mul(self):
          result = self.first * self.second
          return result
      
      def sub(self):
          result = self.first - self.second
          return result     
      
      def div(self):
          result = self.first / self.second
          return result
  
  a = FourCal(4, 2)
  print(a.first)
  #>>>4
  print(a.second)
  #>>>2
  print(a.add())
  #>>>6
  print(a.mul())
  #>>>8
  ```

- **클래스의 상속**

  - 자식 클래스가 부모 클래스에게서 상속받는다는 의미
  - 자식 클래스가 부모 클래스의 메서드, 속성 등을 사용할 수 있다는 의미
  - `class 클래스이름(상속받는 클래스):`

  ```python
  class MoreFourcal(FourCal):
      pass
  
  b = MoreFourcal(5, 4)
  print(b.add())
  #>>>9
  ```

  > 클래스는 상속에 상속을 받을 수 있다.

  - 상속해서 메서드 추가하기

  ```python
  class MoreFourcal(FourCal):
      def pow(self):
          result = self.first ** self.second
          return result
  
  b = MoreFourcal(3, 4)
  print(b.pow())
  #>>>81
  ```

- **메서드 오버라이딩**

  - 메서드 덮어쓰기
  - 0으로 나누면?

  ```python
  a = MoreFourcal(4, 0)
  print(a.div())
  ZeroDivisionError: division by zero
  ```

  - 0으로 나눌 경우를 위해 상속 받을 때 div 메서드를덮어쓰기

  ```python
  class SafeFourCal(FourCal):
      def div(self):
          if self.second == 0:
              return 0
          else : 
              result = self.first / self.second
              return result
  ```

  <aside> 💡 init을 사용해서 self.first = first로 지정하는 이유

  </aside>

  __init__을 사용하는 이유 중 가장 큰 이유는 객체를 생성하면서 동시에 변수를 지정해주기 위함

  - init 생성자를 사용하지 않으면 따로 함수를 만들어서 값을 저장해줘야 함(위 예제의 setdata함수)
  - 매번 해당 함수를 호출해야 된다는 뜻(번거로움)
  - 이러한 이유로 init 생성자를 사용하면 편하게 객체를 생성 할 수 있음

  객체를 생성 할 때 변수 값을 초기화해서 지정해주기 위한 목적도 있음

  - 사칙연산클래스 처럼 a객체에 변수를 귀속시켜놓고, add, sub 같은 여러 다른 함수들에 사용하기 위함.
  - 클래스 내에서 연산만 할거면 변수를 지정해줄 필요가 없음 →클래스 내에 다른 함수에서 r변수를 사용할 필요가 없기 때문에

```python
class Math:
    def solv(self, r):
        return PI * (r**2)
```