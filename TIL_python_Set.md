# # Set

- 자료들의 집합

- 중복을 허용하지 않음

- 순서가 없다

  → 인덱싱 사용 불가

- {} 중괄호로 만들지는 않지만 표기는 {} 중괄호로 표기됨

- set() 함수를 사용하여 생성

- ## **집합자료형 만들기**

  ```python
  s1 = set([1, 2, 3])
  print(s1)
  print(type(s1))
  #>>>{1, 2, 3}
  #>>><class 'set'>
  
  # set 함수 안에 튜플을 넣어서도 만들 수 있다.
  s1 = set((1, 2, 3))
  print(s1)
  print(type(s1))
  #{1, 2, 3}
  #<class 'set'>
  
  #결과물의  차이점은 없다
  ```

- ### **Hello 문자열로 집합만들기**

  ```python
  s3 = set('Hello')
  {'l', 'e', 'H', 'o'}
  #순서가 없다
  #중복을 허용하지 않는다
  #중복 제거용으로 사용할 수 있다.
  #리스트 -> set -> 리스트 하면 중복 제거
  
  a = [1, 2, 3, 4, 2, 3]
  a_set = set(a)
  a = list(a_set)
  print(a)
  #>>>[1, 2, 3, 4]
  ```

- ## **집합을 리스트, 튜플로 변경**

  ```python
  s1 = set([1, 2, 3])
  L1 = list(s1)
  print(L1)
  #>>>[1, 2, 3]
  
  T1 = tuple(s1)
  print(T1)
  #>>>(1, 2, 3)
  ```

- ## **교집합, 합집합, 차집합 구하기**

  - ### 교집합 구하기

    - 직접구하기

    ```python
    s1 = set([1, 2, 3, 4, 5, 6])
    s2 = set([4, 5, 6, 7, 8, 9])
    print(s1 & s2)
    #>>>{4, 5, 6}
    # 집합으로 결과 출력
    ```

    - ### intersection()함수 사용

      - 기준집합.intersection(비교집합)

      ```python
      s1 = set([1, 2, 3, 4, 5, 6])
      s2 = set([4, 5, 6, 7, 8, 9])
      print(s1.intersection(s2))
      #>>>{4, 5, 6}
      ```

      > 만약에 함수를 변수로 지정해버려서 오류가 나는 경우 jupyter notebook에서는 저장하고 껐다키면 되고, cmd에서 작성하던 경우에는 재부팅

  - ### 합집합 구하기

    - ‘|’ 기호 이용 (Shift+백슬래시)

      ```python
      s1 = set([1, 2, 3, 4, 5, 6])
      s2 = set([4, 5, 6, 7, 8, 9])
      print(s1 | s2)
      #>>>{1, 2, 3, 4, 5, 6, 7, 8, 9}
      ```

    - union() 함수사용

      ```python
      s1 = set([1, 2, 3, 4, 5, 6])
      s2 = set([4, 5, 6, 7, 8, 9])
      print(s1.union(s2))
      #>>>{1, 2, 3, 4, 5, 6, 7, 8, 9}
      ```

  - ### 차집합 구하기

    - ‘-’ 기호 이용

      ```python
      s1 = set([1, 2, 3, 4, 5, 6])
      s2 = set([4, 5, 6, 7, 8, 9])
      print(s1 - s2)
      #>>>{1, 2, 3}
      ```

    - .difference() 함수 사용

      - 기준집합.difference(비교집합)

      ```python
      s1 = set([1, 2, 3, 4, 5, 6])
      s2 = set([4, 5, 6, 7, 8, 9])
      print(s1.difference(s2))
      #>>>{1, 2, 3}
      ```

- ## **집합자료형 관련 함수들**

  - 값 한개 추가(add)

    - s1.add(4)

    ```python
    s1 = set([1, 2, 3])
    s1.add(4)
    print(s1)
    {1, 2, 3, 4}
    ```

  - 값 여러개 추가(update)

    - s1.update([추가할 원소])

    ```python
    s1 = set([1, 2, 3])
    s1.update([4, 5, 6])
    print(s1)
    #>>>{1, 2, 3, 4, 5, 6}
    
    s1 = set([1, 2, 3])
    s1.update((4,))
    print(s1)
    #>>>{1, 2, 3, 4}
    ```

  - 특정 값 제거 (remove)

    ```python
    s1 = set((1, 2, 3))
    s1.remove(2)
    print(s1)
    #>>>{1, 3}
    ```

- ## **수정 불가능한 집합 생성**

  - frozenset() 함수 사용

  ```python
  s2 = frozenset([1, 2, 3])
  print(s2)
  print(type(s2))
  #>>>frozenset({1, 2, 3})
  #>>><class 'frozenset'>
  
  s2.add(4)
  #AttributeError: 'frozenset' object has no attribute 'add'
  
  s2.update([4, 5])
  #AttributeError: 'frozenset' object has no attribute 'update'
  
  s2.remove(3)
  #AttributeError: 'frozenset' object has no attribute 'remove'
  ```