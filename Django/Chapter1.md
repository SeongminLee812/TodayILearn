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
- 