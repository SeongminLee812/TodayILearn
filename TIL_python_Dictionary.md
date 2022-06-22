# Dictionary

- 데이터들의 집합

- 순서를 가지고 있지 않음

  → 인덱싱과 슬라이싱이 불가능 하다.

  → *리스트와 튜플은 순서와 값이 매핑되는 개념*

- 키와 값을 매핑시켜주는 자료형

- 딕셔너리의 key는 고유한 값이므로 리스트를 key로 생성할 수 없다.

- {} 중괄호로 묶어줌

- **딕셔너리 생성**

  - 키에는 리스트나 튜플이 포함될 수 없음

    → 문자열이나 변수가 온다.

  - 값에는 리스트나 튜플이 포함될 수 있음

  ```python
  dic = {'name' : 'seongmin', 'phone' : '01030670280', 'birth' : '0812'}
  print(type(dic))
  #>>><class 'dict'>
  
  a = {'a' : [1, 2, 3]}
  print(a)
  #>>>{'a': [1, 2, 3]}
  ```

  - dict()함수 사용

    ```python
    #dict 함수 사용 1
    a = dict(one=1, two=2)
    print(a)
    #>>>{'one': 1, 'two': 2}
    #이때는 문자열을 따옴표로 묶으면 안됨
    
    #dict 함수 사용 2
    #대괄호로 키, 값끼리 묶어주고 전체 크게 한번 대괄호 묶어줌
    b = dict([['one', 1], ['two', 2]])
    print(b)
    #>>>{'one': 1, 'two': 2}
    ```

  <aside> 💡 딕셔너리의 키에는 숫자형이 올 수 있다. 그러나, 위 dict 함수 사용 1번 방법에서 키 값에 수치형을 넣을 수 없다. →dict()함수는 ‘키워드 인수’를 사용하기 때문에 키가 문자열일 경우 동작함. 키값을 따옴표로 묶어주지 않는 이유도 같은 이유

  </aside>

  - 키와 값을 한곳에 모아서 딕셔너리 생성

    - 리스트도 되고 튜플도 됨

    ```python
    keys = ['one', 'two', 'three']
    values = (1, 2, 3)
    d = dict(zip(keys, values))
    print(d)
    #>>>{'one': 1, 'two': 2, 'three': 3}
    ```

- **딕셔너리에 쌍 추가**

  - 딕셔너리명[키] = 값

  ```python
  a = {1 : 'a'}
  a['name'] = 'pey'
  print(a)
  #>>>{1: 'a', 'name': 'pey'}
  ```

- **함수와 딕셔너리를 함께 사용가능**

  - 딕셔너리의 값으로 함수가 들어갈 수 있다.

  ```python
  def add(a, b):
      return a + b
  def sub(a, b):
      return a - b
  
  action = {0:add, 1:sub}
  print(action[0](4, 5))
  #>>>9
  ```

- **딕셔너리의 요소 제거**

  - del 코드 사용( `del 딕셔너리명[키]`)

  ```python
  a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
  del a[1]
  print(a)
  #>>>{2: 'b', 'name': 'pey', 3: [1, 2, 3]}
  ```

  ```python
  
  ```

- **딕셔너리에서 특정 값 얻기**

  - `딕셔너리명[키]`

  ```python
  a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
  print(a['name'])
  #>>>pey
  ```

- **딕셔너리 관련 함수들**

  - 키 리스트 만들기(keys)

    - `a.keys()`

    ```python
    a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
    
    print(a.keys())
    #dict_keys([1, 2, 'name', 3])
    
    print(type(a.keys()))
    #<class 'dict_keys'>
    ```

  - 값 리스트 만들기(values)

    - `a.values()`

    ```python
    a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
    print(a.values())
    #dict_values(['a', 'b', 'Lee', [1, 2, 3]])
    
    print(type(a.values()))
    #<class 'dict_values'>
    ```

  - 키, 값을 쌍으로 얻기(items)

    - `a.items()`

    ```python
    a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
    print(a.items())
    #>>>dict_items([(1, 'a'), (2, 'b'), ('name', 'Lee'), (3, [1, 2, 3])])
    
    print(type(a.items()))
    #>>><class 'dict_items'>
    ```

  - 딕셔너리 쌍 모두 지우기(clear)

    - a.clear()

    ```python
    a.clear()
    print(a)
    #>>>{}
    #딕셔너리의 내용을 다 비운다.
    ```

  - 키로 값 얻기(get)

    - `a.get('key')`

    ```python
    a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
    print(a.get('name'))
    #>>>'pey'
    
    print(a.get('birth'))
    #>>>None
    
    print(a['birth'])
    #>>>KeyError: 'birth'
    ```

    <aside> 💡 get함수는 없는 값을 찾을 때 none이 나오고 a[]로 찾으면 KeyError가 나온다

    </aside>

  - 키가 딕셔너리 안에 있는 지 확인(in)

    - True나 False 값을 돌려줌

    ```python
    a = {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
    print('name' in a)
    #>>>True
    
    print('birth' in a)
    #>>>False
    ```