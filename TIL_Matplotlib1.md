# Matplotlib

![Matplotlib](C:\Users\leesu\OneDrive\바탕 화면\TIL\Matplotlib.png)



## 그리기

```python
import matplotlib.pyplot as plt

# figure 생성 : 도화지
fig = plt.figure()
# subplots : 도화지를 몇개로 나눌건지
ax = fig.subplots()
# 그래프 그리기
ax.plot([1, 2, 3, 4, 5])
# 그래프 출력
plt.show()
```

![image-20220626103651130](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626103651130.png)

- figure 생성 : 도화지
- subplots : 도화지를 몇개로 나눌건지



### 한 그래프에 두개 그리기

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
x = [1, 2, 3, 4, 5]
y01 = list(map(lambda i: i**2, x))
y02 = list(map(lambda i: i**3, x))

ax.plot(x, y01, color='red', label='pow2')
ax.plot(x, y02, color='blue', label='pow3')

#라벨표시
plt.legend()

plt.show()
```

- .legend로 라벨 표시 가능

  ![image-20220626103705002](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626103705002.png)

  

### 두개 그리기

```python
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(10, 5))

ax01 = fig.add_subplot(1, 2, 1)
ax02 = fig.add_subplot(1, 2, 2)

x = [1, 2, 3, 4, 5]
y01 = list(map(lambda i: i**2, x))
y02 = list(map(lambda i: i**3, x))

ax01.plot(x, y01, color='red', label='pow2')
ax02.plot(x, y02, color='blue', label='pow3')

ax01.legend()
ax02.legend()

ax01.set_title('x**2')
ax02.set_title('x**3')

plt.show()
```

- add_subplot으로 (행, 열, 해당 그림 위치)로 그래프 나누기

![image-20220626103754783](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626103754783.png)



## [scatter][https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html]

```python
import matplotlib.pyplot as plt
import random

fig = plt.figure()
ax01 = fig.add_subplot(2, 2, 1)
ax02 = fig.add_subplot(2, 2, 2)
ax03 = fig.add_subplot(2, 2, 3)
ax04 = fig.add_subplot(2, 2, 4)

x = list(range(50))
y01 = list(random. randint(0, 50) for i in range(50))
y02 = list(random. randint(0, 50) for i in range(50))
y03 = list(random. randint(0, 50) for i in range(50))
y04 = list(random. randint(0, 50) for i in range(50))

ax01.scatter(x, y01, color='red')
ax02.scatter(x, y02, color='green')
ax03.scatter(x, y03, color='blue')
ax04.scatter(x, y04, color='yellow')

ax01.set_title('y01')
ax02.set_title('y02')
ax03.set_title('y03')
ax04.set_title('y04')

# 레이아웃을 자동으로 적당히 조절
fig.tight_layout()

plt.show()
```

- set_title로 각 그래프의 이름 지정가능

- tight_layout() : subplo들 간의 간격(padding) 가장 적합하게 조정해줌

- **matplotlib.pyplot.****scatter****(***x***,** *y***,** *s=None***,** *c=None***,** *marker=None***,** *cmap=None***,** *norm=None***,** *vmin=None***,** *vmax=None***,** *alpha=None***,** *linewidths=None***,** *****,** *edgecolors=None***,** *plotnonfinite=False***,** *data=None***,** ***kwargs***)**

  

### 변수에 저장하지 않고 figure 코드 한번에 만들기

```python
import matplotlib.pyplot as plt
import random

'''
fig = plt.figure()
ax01 = fig.add_subplot(2, 2, 1)
ax02 = fig.add_subplot(2, 2, 2)
ax03 = fig.add_subplot(2, 2, 3)
ax04 = fig.add_subplot(2, 2, 4)
'''
# 코드 짧게 쓰기
fig, ax = plt.subplots(2, 2, figsize=(10, 10))

x = list(range(50))
y01 = list(random. randint(0, 50) for i in range(50))
y02 = list(random. randint(0, 50) for i in range(50))
y03 = list(random. randint(0, 50) for i in range(50))
y04 = list(random. randint(0, 50) for i in range(50))

ax[0, 0].scatter(x, y01, color='red')
ax[0, 1].scatter(x, y02, color='green')
ax[1, 0].scatter(x, y03, color='blue')
ax[1, 1].scatter(x, y04, color='yellow')

ax[0, 0].set_title('y01')
ax[0, 1].set_title('y02')
ax[1, 0].set_title('y03')
ax[1, 1].set_title('y04')

# 레이아웃을 자동으로 적당히 조절
fig.tight_layout()

plt.show()
```

![image-20220626104432661](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626104432661.png)







## [hist][https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist.html]

```python
import matplotlib.pyplot as plt
from random import randint

fig, ax = plt.subplots()

x = list(randint(0, 10) for i in range(100))

ax.hist(x, bins=10)

#축 눈금
plt.xticks(list(range(0, 11)))
plt.yticks(list(range(0, 101, 5)))

#축 제한
plt.xlim(0, 10)
plt.ylim(0, 100)

plt.show()
```

- **matplotlib.pyplot.****hist****(***x***,** *bins=None***,** *range=None***,** *density=False***,** *weights=None***,** *cumulative=False***,** *bottom=None***,** *histtype='bar'***,** *align='mid'***,** *orientation='vertical'***,** *rwidth=None***,** *log=False***,** *color=None***,** *label=None***,** *stacked=False***,** *****,** *data=None***,** ***kwargs***)**

![image-20220626104504736](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626104504736.png)

## cumulative

```python
import matplotlib.pyplot as plt
from random import randint

fig, ax = plt.subplots()

x = list(randint(0, 10) for i in range(100))
# cumulative
ax.hist(x, bins=10, cumulative=True)
ax.hist(x, bins=10, cumulative=False)

plt.xticks(list(range(1, 11)))
plt.yticks(list(range(1, 101, 5)))
plt.xlim(0, 10)
plt.ylim(0, 100)

plt.show()
```

- **cumulative**bool or -1, default: False

  If `True`, then a histogram is computed where each bin gives the counts in that bin plus all bins for smaller values. The last bin gives the total number of datapoints.

  If *density* is also `True` then the histogram is normalized such that the last bin equals 1.

  If *cumulative* is a number less than 0 (e.g., -1), the direction of accumulation is reversed. In this case, if *density* is also `True`, then the histogram is normalized such that the first bin equals 1.

- cumulative -> 누적

![image-20220626104511378](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626104511378.png)



## [boxplot][https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.boxplot.html]

```python
import matplotlib.pyplot as plt
from random import randint
import numpy as np

fig, ax = plt.subplots()

x = list(randint(0, 1000) for i in range(10))
print(f'평균 : {np.mean(x)}')
print(f'중위값 : {np.median(x)}')

ax.boxplot(x)

plt.show()
```

- np.mean ⇒ 산술평균 구하기
- np.median ⇒ 중위값 구하기

![image-20220626104531125](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626104531125.png)





## [pie][https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.pie.html]

```python
import matplotlib.pyplot as plt
from random import randint

fig, ax = plt.subplots()

ages = list(randint(1, 100) for i in range(100))

child = list(x for x in ages if x < 19)
young = list(x for x in ages if 19 <= x < 40)
middle = list(x for x in ages if 40 <= x < 60)
old = list(x for x in ages if 60 < x)

labels = ['child', 'young', 'moddle', 'old']
count = [len(child), len(young), len(middle), len(old)]

ax.pie(count, labels=labels, startangle=90, counterclock=False)

plt.show()
```

- pie 인자

  - **startangle** : float, default: 0 degrees

    The angle by which the start of the pie is rotated, counterclockwise from the x-axis.

    - 90으로 맞춰야 아래처럼 보기 편한 각도가 된다.

- **counterclock** : bool, default: True

  Specify fractions direction, clockwise or counterclockwise.

  - True : 반시계방향으로 그림
  - False : 시계방향으로 그림 (이게 더 편안)

![image-20220626104948543](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626104948543.png)



## [bar][https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.bar.html]

```python
import matplotlib.pyplot as plt
from random import randint

fig = plt.figure()
ax01 = fig.add_subplot(1, 2, 1)
ax02 = fig.add_subplot(1, 2, 2)

x = [1, 2, 3, 4, 5]
y = list(randint(1, 100) for i in range(5))

ax01.bar(x, y, color='r')
ax02.barh(x, y, color='b')

plt.show()
```

- bar : 세로막대 그래프
- barh : bar horizontal, 가로막대 그래프

![image-20220626105320829](C:\Users\leesu\AppData\Roaming\Typora\typora-user-images\image-20220626105320829.png)