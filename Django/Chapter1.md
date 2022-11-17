# 파이썬 기억 안나는 부분 복습

### 딕셔너리 키 에러 디폴트 값 주기
- dic에 존재하지 않는 key 값을 가져오라고 하면 key Error가 발생
- 디폴트 값 지정으로 문제 해결하기
```python
dic1 = {'Name', 'seongmin', 'Phone', '010-1111-2222'}
dic1['job']
 # KeyError 발생
dic1.get('job', 'student')
# 'student'
```

### 함수 만들 때 입력 값 전부를 인수로 받기
- 함수 내에서 *사용하기
```python
def input_sum(*numbers):
	sum = 0
	for number in numbers:
		sum = sum + number
	return sum

>>> print(input_sum(1, 2, 3))
# 6 
```

### 클래스의 __init__함수 
- 매직 메소드, 특별메소드, 더블언더메소드, 더블 메소드 등으로 불림
- 클래스에서 사용되는 함수로 객체가 생성된 후에 자동으로 호출되는 함수
- 'Account' 클래스는 객체가 생성될 때 인수로 전달한 name을 멤버 변수의 name으로 설정하며, 멤버 변수로 money를 10000으로 가진다.
```python
class Account:  
   def __init__(self, name):  
      print('[init start]객체가 생성됩니다.')  
      self.name = name  
      self.money = 10000  
      print('[init end]객체가 생성되었습니다.')  
     
   def deposit(self, money):  
      print(f'[deposit start]돈을 입금합니다. {money}원')  
      self.money = self.money + money  
      print(f'[deposit end]돈을 입금했습니다.{money}원')  
  
   def withdraw(self, money):  
      print(f'[withdraw start]돈을 출금합니다.{money}원')  
      self.money = self.money - money  
      print(f'[withdraw end]돈을 출금했습니다.{money}원')  
      return money  
     
   def print_balance(self):  
      print('[print_balance start]잔고를 출력합니다.')  
      print('잔고 :', self.money)  
      print('[print_balance end]잔고를 출력했습니다.')  
     
   def print_owner(self):  
      print('[print_owner start]소유자를 출력합니다.')  
      print('소유자 :', self.name)  
      print('[print_owner end]소유자를 출력했습니다.')  
  
account = Account('홍길동')
```
