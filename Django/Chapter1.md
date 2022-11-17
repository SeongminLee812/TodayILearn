# 파이썬 기억 안나는 부분 복습

- dic에 존재하지 않는 key 값을 가져오라고 하면 key Error가 발생
- 디폴트 값 지정으로 문제 해결하기
```python
dic1 = {'Name', 'seongmin', 'Phone', '010-1111-2222'}
dic1['job']
 # KeyError 발생
dic1.get('job', 'student')
# 'student'
```


