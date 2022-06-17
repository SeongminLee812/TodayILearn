# Models

# Models

---

# sqlite 설치, 연결

django-admin startproject dbtest로 dbtest 프로젝트를 만들고 아래 과정을 진행하였다.

## 1. 홈페이지에서 sqlite3 설치하기

[SQLite Download Page](https://www.sqlite.org/download.html)

**Precompiled Binaries for Windows 중**

[sqlite-tools-win32-x86-3380500.zip](https://www.sqlite.org/2022/sqlite-tools-win32-x86-3380500.zip) 설치

## 2. 실행시키기

- 실행되나 확인
    - 위 압출 풀어서 sqlite3.exe를 프로젝트 폴더에 넣어줌
    - 폴더에 넣고, sqlite3 명령어 입력해서 실행
    - 종료 ⇒ .exit

runserver로 서버를 실행하면 

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled.png)

위와 같은 문장을 볼 수 있음

`python [manage.py](http://manage.py) migrate` 실행해서 적용해야한다는 사실을 확인

## 3. [settings.py](http://settings.py)

> 프로젝트 ⇒ 전체 웹 어플리케이션
앱 ⇒ 특정 기능이 모여저 있는 앱
> 

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%201.png)

사용되는 앱들을 database에 요청해서 실행하는 명령어가 migrate임

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%202.png)

engine의 default값으로 sqlite3이 지정되어 있다.

## 4. 앱에서 확인하기

- dbtest 앱 폴더에 들어가면 db.sqlite3파일이 만들어져 있다. 2번의 migrate로 만들어짐
- `sqlite3 db.sqlite3` 명령어 입력하여 실행

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%203.png)

실행됨

.table로 db에 저장된 table로 볼 수 있고, django가 필요한 table들을 만들어 놓음

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%204.png)

## 6. settings.py에서 앱 추가

- database와 연결하기 위해서는 dbtest 앱 안 settings.py에 INSTALLED_APPS에 dbtest앱을 추가해준다.

```python
INSTALLED_APPS = [ # 설치된 앱들 확인 가능
    'django.contrib.admin', # 관리자
    'django.contrib.auth', # 승인 등
    'django.contrib.contenttypes', # 내용 타입
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles', # 정적파일 관리 앱
    'dbtest',
]
```

## 7. dbtest앱 안에 [models.py](http://models.py) 만들기

- MyBoard라는 클래스는 django의 테이블이 된다.
- 필드 이름이 될 내용들도 작성해줘야한다

```python
from django.db import models

class Myboard(models.Model): # models.Model 클래스를 상속받아 클래스 생성
    myname = models.CharField(max_length = 100) #필드가 될 것
    mytitle = models.CharField(max_length = 500)
    mycontent = models.CharField(max_length=2000)
    mydate = models.DateTimeField()
```

- django는 개발자들에게 Model layer를 제공한다. (관계형 데이터 베이스를 몰라도 이용할 수 있게)
- CharField란?
    - 문자열 필드이며, 작은 사이즈부터 큰 사이즈의 문자열 까지 가능
    - 큰 양의 텍스트는 → TextField를 이용해야함
    - dv_collation을 인자로 받을 수 있다(optional)
- ***class* `DateTimeField`(*auto_now=False*, *auto_now_add=False*, ***options*)[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.DateTimeField)**
    - 파이썬에서 datetime.datetime을 대신해 date와 시간을 표시한다.

[Model field reference | Django documentation | Django](https://docs.djangoproject.com/en/4.0/ref/models/fields/)

## 8. 새로운 migration만들기

- django에 명령문을 내리면 알아서 쿼리문으로 바꿔줌
- model은 django에서 db와 소통해주는 도구임
- `python [manage.py](http://manage.py) makemigrations dbtest`
- 명령문을 실행하면 migrations 밑에 파일이 생성됨.

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%205.png)

## 9. 0001_initial.py

- 8번과 같이 만들어진 dbtest/migrations/0001_initial.py를 확인

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%206.png)

## 10. 데이터 베이스에 명령전달

`python [manage.py](http://manage.py) migrate` 로 명령 전달해서 db에 적용함.

## 11. 데이터 베이스에 table 생겼는지 확인하기

1. `sqlite3 db.sqlite3` 로 sqlite 실행
2. `.table` 로 테이블 생성되었나 확인 ⇒ dbtest_myboard 생성됨

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%207.png)

1. `.schema dbtest_myboard`로 내용 보기
    - 쿼리문 형식으로 작성되어 있다!

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%208.png)

## 데이터베이스에 필드값 추가하기 테스트

### python 객체를 이용해서 db에 insert함.

`pyhon [manage.py](http://manage.py) shell`을 입력하면 python shell이 나온다.

django 와 소통할 수 있는 interactive shell이다.

shell(Terminal)에 아래와 같이 입력하여

클래스명(필드이름=필드값)

```python
>>> from dbtest.models import Myboard
>>> from django.utils import timezone
>>> test = Myboard(myname='testuser', mytitle='test title', mycontent='t
est 1234 abcd', mydate=timezone.now())
>>> test.save()

>>> exit()
```

# test 데이터 읽어보기

## 1. templates 연결하기

1. dbtest 앱 밑에 templates 폴더 만들기
    - 따로 templates 경로를 저장해주지 않으면 기본 앱 밑에 templates 폴더를 찾아감
2. templates 폴더 안에 index.html 만들기

```html
<body>

    <h1>Hello, Django (with sqlite3)</h1>

    <table border="1">
        <col width="50">
        <col width="100">
        <col width="500">
        <col width="100">

        <tr>
            <th>번호</th>
            <th>작성자</th>
            <th>제목</th>
            <th>작성일</th>
        </tr>

        {% if not list %}
            <tr>
                <th>----------작성된 글이 없습니다----------</th>
            </tr>
        {% else %}
            {% for data in list %}
                <tr>
                    <td>{{ data.id }}</td>
                    <td>{{ data.myname }}</td>
                    <td><a href="#">{{ data.mytitle }}</a></td>
				<!--제목 누르면 다른 곳으로 넘어갈 수 있게 a태그 생성-->
                    <td>{{ data.mydate }}</td>
                </tr>
            {% endfor %}
        {% endif %}
        <tr>
            <td colspan="4" align="right">
                <input type="button" value="글작성" onclick="">
            </td>
        </tr>

    </table>
```

## 2. views.py만들어서 함수 정의

```python
from django.shortcuts import render
from .models import Myboard # 내가 만든 클래스를 불러와야함

def index(request):
    # object : django.db.models.Manager 객체
    myboard = Myboard.objects.all() 
    return render(request, 'index.html', {'list':myboard})
```

- objects?
    - objects.all() ⇒ select * from table
    - `myboard = Myboard.objects.all() => select * from Myboard`

## 3. urls.py에서 경로요청 위임

```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index),
]
```

> object랑 charfield, datetimefield가 뭔지?
> 

# 게시판 제목 클릭하면 연결되는 페이지 만들기

## 1. templates폴더에 detail.html만들어주기

- 현재 html은 목록에서 게시글을 클릭하면 나오는 상세페이지임

```html
<body>

    <h1>Detail</h1>

    <table border="1">
        <tr>
            <th>작성자</th>
            <td><input type="text" value="{{ dto.myname }}" readonly></td>
        </tr>
        <tr>
            <th>제목</th>
            <td><input type="text" value="{{ dto.mytitle }}" readonly></td>
        </tr>
        <tr>
            <th>내용</th>
            <td><textarea cols="60" rows="10" readonly>{{ dto.mycontent }}</textarea></td>
        </tr>
        <tr>
            <td colspan="2" align="right">
                <input type="button" onclick="" value="목록">
                <input type="button" onclick="" value="수정">
                <input type="button" onclick="" value="삭제">
            </td>
        </tr>
    </table>
</body>
```

- dto.myname은 views.py에서 딕셔너리 형태로 들어올 것이고, myname은 dto 딕셔너리의 key일 것이다.

## 2. views.py

```python
def detail(request, id): #=> detail/<int:id> 로 지정된것
    return render(request, 'detail.html', {'dto': Myboard.objects.get(id=id)}) #매개변수로 받은 id(뒤에 있는 id) /쿼리문의 where id=id
```

- detail의 매개변수로 받는 id는 아직 지정되지 않았고 아래 urls.py에서 변수가 지정될 것이다.
- Myboard(내가만들어논 모델)에 objects.get함수를 통하여 값을 가져오는데,
    - 이때 get(id=id)의 첫번째 id는 migration을 통해 생성된 primary key인 id이고, 두번째 id는 위 detail함수의 인자로 받은 id이다.
- database(Myboard)에서 id가 urls.py를 통해 요청될 id와 같은 것을 가져와 렌더링해줄 것이다.

### 3. urls.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index),
    path('detail/<int:id>', views.detail),
    # 꺽새 안 int:id는 값 그 값을 id 라고 할 것이다(int type). 그 값이 def detail의 인자로 감
    # int type의 id 변수가 지정됨.
]
```

- /detail을 통해 요청이 들어올 건데, detail/뒤에 오는 것들을 값으로 만들어 줄 것이다.
- datail/ 뒤에 오는 것을은 int 형태로 id라는 변수에 지정해준다.
- 이 id변수는 2번 views.py에서 만들어 놓은 detial함수의 id인자로 전달된다.

### 4. index.html에서 a태그 손보기

```python

{% for data in list %}
    <tr>
        <td>{{ data.id }}</td>
        <td>{{ data.myname }}</td>
        <td><a href="detail/{{ data.id }}">{{ data.mytitle }}</a></td>
        <td>{{ data.mydate }}</td>
    </tr>
{% endfor %}
```

- a태그를 통해서 요청을 보낼때 detail/뒤에 {{ [data.id](http://data.id) }}로 되어있는 것이 확인되며, 이 data.id 는 3번 urls.py의 <int:id>로 전달된다.
    - 즉, data.id가 id 변수로 저장될 것이다.
    - 위 for문의 data인데 위 for문에 전달된 list views.index에서 확인 가능하다
    
    ```python
    def index(request):
        # object : django.db.models.Manager 객체
        myboard = Myboard.objects.all() 
        return render(request, 'index.html', {'list':myboard})
    ```
    
    - ‘list’에 내가 만든 모델 Myboard의 객체 전부가 전달되었다.
    - for문을 통해 모든 게시글 하나하나(data변수로 지정) 반복하여 탐색하며, 그 게시글의 id가 {{[data.id](http://data.id)}} 가 된다.

---

# 내용 만들어서 입력하기

## 1. templates에 insert.html 만들기

- 글 작성하는 페이지
- form태그에 값을 보내기 위해서 name속성 반드시 필요함
- form태그에 submit 이벤트를 발생시키는경우
    - name속성이 key가 되고, input태그에 작성된 내용이 value가 되어 전달될 것임
        - {name:value} 형태로 전달
- **form 태그 사용하여 post방식으로 보내는 경우 반드시! {% csrf_token %} 사용!**

```html
<body>

    <h1>Insert</h1>

    <form action="" method="post">{% csrf_token %}
        <table border="1">
            <tr>
                <th>작성자</th>
                <td><input type="text" name="myname"></td>
            </tr>
            <tr>
                <th>제목</th>
                <td><input type="text" name="mytitle"></td>
            </tr>
            <tr>
                <th>내용</th>
                <td><textarea cols="60" rows="10" name="mycontent"></textarea></td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="취소" onclick="">
                    <input type="submit" value="글작성">
                </td>
            </tr>
        </table>
    </form>
</body>
```

### 2. [views.py](http://views.py)에서 함수 정의하기

- insert 페이지를 열어주는 함수

```python
def insert_form(request): # insert 페이지 열어주는 함수
    return render(request, 'insert.html')
```

- submit 이벤트 발생하면 db에 저장해줄 함수
    - POST[’myname’] ⇒ insert.html에서 form tag로 요청할 때 name속성으로 잡아준 key값

```python
from django.shortcuts import render, redirect
from django.utils import timezone

def insert_proc(request): # submit 이벤트가 발생하면 db에 저장해줄 함수
    myname = request.POST['myname'] # request에 POST로 전달된 값 myname을 가져옴
    mytitle = request.POST['mytitle']
    mycontent = request.POST['mycontent']
		result = Myboard.objects.create(myname=myname, mytitle=mytitle, mycontent=mycontent, mydate=timezone.now)
		
		if result:
        return redirect('index')
    else:
        return redirect('insertform')
```

- result 변수는 위에서 본 [python shell로 넣기](https://www.notion.so/Models-5041998cff2a482eaadeef643b3a54e9)와 같은 것임. ⇒ result의 결과가 어떻게 나오는 지는 이따 보자
- redirect 안에다 써주는 것들은 urls.py에 name속성이 있어야함 ⇒ 밑에 3번에서 name속성 정의
- 

### 3. urls.py에서 위임해주기 및 name 속성 정의하기

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'), #name속성 정의
    path('detail/<int:id>', views.detail, name='detail'),
    path('insertform/', views.insert_form, name='insertform'), #name 속성 정의
    path('insertres/', views.insert_proc),
]
```

- redirect를 이용하기 위해서 name속성 정의

### 4. index.html 다시 돌아가서

```html
<body>

    <h1>Hello, Django (with sqlite3)</h1>

    <table border="1">
        <col width="50">
        <col width="100">
        <col width="500">
        <col width="100">

        <tr>
            <th>번호</th>
            <th>작성자</th>
            <th>제목</th>
            <th>작성일</th>
        </tr>

        {% if not list %}
            <tr>
                <th>----------작성된 글이 없습니다----------</th>
            </tr>
        {% else %}
            {% for data in list %}
                <tr>
                    <td>{{ data.id }}</td>
                    <td>{{ data.myname }}</td>
                    <td><a href="{% url 'detail' data.id %}">{{ data.mytitle }}</a></td>
                    <td>{{ data.mydate }}</td>
                </tr>
            {% endfor %}
        {% endif %}
        <tr>
            <td colspan="4" align="right">
                <input type="button" value="글작성" onclick="location.href='insertform/'">
										<!--위부분 onclick-->
            </td>
        </tr>
    </table>
</body>
```

- `<td><a href="{% url 'detail' [data.id](http://data.id) %}">{{ data.mytitle }}</a></td>` a 태그의 경로를 {% url %}으로 바꿔주고, 위 urls.py에서 정의한 name=’detail’로 보내주며, 뒤의 data.id 데이터를 같이 보내줄 것임
- `<input type="button" value="글작성" onclick="location.href=insertform">`
    - onclick=””은 js의 영역이므로 js코드로 location.href실행해서 insertform/이라고 요청함. 
    ⇒ urls.py에서 views.insertform으로 위임하고, views.py에서 처리함.
- urls.py

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%209.png)

- views.py

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%2010.png)

- insert.html 템플릿을 렌더링해줌

## 4. insert.html에서 form 경로 지정

```html
<h1>Insert</h1>

    <form action="/insertres/" method="post">{% csrf_token %}
        <table border="1">
            <tr>
                <th>작성자</th>
                <td><input type="text" name="myname"></td>
            </tr>
            <tr>
                <th>제목</th>
                <td><input type="text" name="mytitle"></td>
            </tr>
            <tr>
                <th>내용</th>
                <td><textarea cols="60" rows="10" name="mycontent"></textarea></td>
            </tr>
            <tr>
                <td colspan="2" align="right">
                    <input type="button" value="취소" onclick="location.href='/'">
                    <input type="submit" value="글작성">
                </td>
            </tr>
        </table>
    </form>
</body>
```

- `<form action="/insertres/" method="post">{% csrf_token %}`
/insertres/ 와 같이 루트/insertres/로 경로 요청 해줘야한다.
- 그렇지 않으면, 현재 insertform url에서 insertres로 다시 요청한 것이기 때문에 insertform/insertres/로 요청되기 때문에 404에러가 됨.
    
    > 404 에러 → 페이지가 안만들어졌거나, 경로 에러
    > 
- 취소 버튼에는 다시 / 경로로 요청함.

# update

> 순서도
> 

## 1. template에 update.html만들기

- 수정버튼을 클릭하면 열릴 화면

```html
<h1>Update</h1>

<form action="" method="post"> {% csrf_token %}
    <table border="1">
    <input type="hidden" name="id" value="{{ dto.id }}">
        <tr>
            <th>작성자</th>
            <td><input type="text" value="{{ dto.myname }}" readonly></td>
        </tr>
        <tr>
            <th>제목</th>
            <td><input type="text" value="{{ dto.mytitle }}" name="mytitle"></td>
        </tr>
        <tr>
            <th>내용</th>
            <td><textarea cols="60" rows="10" name="mycontent">{{ dto.mycontent }}</textarea></td>
        </tr>
        <tr>
            <td>
                <input type="button" value="취소" onclick="">
                <input type="submit" value="글수정">
            </td>
        </tr>
    </table>

</form>
```

- 제목과 내용을 수정가능, 내용을 불러와서 보여줌
- `<input type="hidden" name="id" value="{{ dto.id }}">` where문을 위해 id 값을 보내줘야함
- 해당 id부분만 update 해주기 위함.
- sql 쿼리문에서 그냥 update하고 where를 설정해주지 않으면 해당 필드의 모든 값이 변하므로 where문을 설정해주기 위해 id또한 같이 보내주는 것임

## 2. views.py

- views.py에서 db에서 id에 맞게 위 {{}} 안의 내용들을 불러올 수 있게하기
- update.html 렌더링 해주는 코드 만들기

```python
def update_form(request, id):
    return render(request, 'update.html', {'dto':Myboard.objects.get(id=id)})
```

- id를 인자로 받아 Myboard에 같은 id를 가진 객체를 불러와서 dto에 넣어줌

- update.html이 submit되면 그 데이터를 받아 db에 업데이트 해주는 코드

```python
def update_proc(request):
    mytitle = request.POST['mytitle']
    mycontent = request.POST['mycontent']
    id = request.POST['id']

    myboard = Myboard.objects.filter(id=id)
    result_title = myboard.update(mytitle=mytitle)
    result_content = myboard.update(mycontent=mycontent)

    if result_title + result_content == 2: # 수정 성공할 때
        return redirect('/detail/' + id)
    else: # 수정 실패할 때
        return redirect('/updateform/' + id)
```

- 원래 redirect는 name으로 찾아주나, 뒤에 /와 값이 들어가면 그냥 path로 찾아준다. → 따라서 루트경로 설정
- filter 함수를 통해 위 1번에서 받은 id와 동일한 id를 가진 필드값만 가져와서 변수 myboard에 지정한 후 update해준다.
- redirect는 server에서 나와서 client에 응답하는 것이 아니라 server에 다시 요청

![Untitled](Models%2068e1271fcaa2498289b0f158a5375992/Untitled%2011.png)

> update가 뭔지?
> 

## 3. urls.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('detail/<int:id>', views.detail, name='detail'),
    path('insertform/', views.insert_form, name='insertform'),
    path('insertres/', views.insert_proc),
    path('updateform/<int:id>', views.update_form, name='updateform'),
    path('updateres/', views.update_proc),
]
```

detail.html에서 updateform으로 요청이 들어오면 id도 같이 받아 변수에 저장

## 4. detail.html

```html
<h1>Detail</h1>

    <table border="1">
        <tr>
            <th>작성자</th>
            <td><input type="text" value="{{ dto.myname }}" readonly></td>
        </tr>
        <tr>
            <th>제목</th>
            <td><input type="text" value="{{ dto.mytitle }}" readonly></td>
        </tr>
        <tr>
            <th>내용</th>
            <td><textarea cols="60" rows="10" readonly>{{ dto.mycontent }}</textarea></td>
        </tr>
        <tr>
            <td colspan="2" align="right">
                <input type="button" onclick="" value="목록">
                <input type="button" onclick="location.href='/updateform/{{ dto.id }}'" value="수정">
                <input type="button" onclick="" value="삭제">
            </td>
        </tr>
    </table>
```

- 수정버튼 클릭했을 때
    - location.href 로 /updateform/ (root경로+updateform) 으로 보내주고, id까지 같이 보내준다
    

# Delete

- 삭제할 때는 삭제할 페이지가 일반적으로 나오지 않으니, 렌더링해줄 필요가 없다

## 1. views.py

- delete from ‘table’ where id=~

```python
def delete_proc(request, id):
    result_delete = Myboard.objects.filter(id=id).delete()
    
    if result_delete[0]:
        return redirect('index')
    else:
        return redirect('/detail/' + id)
```

마찬가지로 where 조건문을 위해 id를 받아 

filter를 통해 나온 객체를 삭제함

## 2. urls.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('detail/<int:id>', views.detail, name='detail'),
    # 꺽새 안 int:id는 값 그 값을 id 라고 할 것이다(int type). 그 값이 def detail의 인자로 감
    # int type의 id 변수가 지정됨.
    path('insertform/', views.insert_form, name='insertform'),
    path('insertres/', views.insert_proc),
    path('updateform/<int:id>', views.update_form, name='updateform'),
    path('updateres/', views.update_proc),
    path('delete/<int:id>', views.delete_proc),
]
```

## 3. detail.html에 삭제 버튼 누르면 이동하게 만들기

```html
<h1>Detail</h1>

    <table border="1">
        <tr>
            <th>작성자</th>
            <td><input type="text" value="{{ dto.myname }}" readonly></td>
        </tr>
        <tr>
            <th>제목</th>
            <td><input type="text" value="{{ dto.mytitle }}" readonly></td>
        </tr>
        <tr>
            <th>내용</th>
            <td><textarea cols="60" rows="10" readonly>{{ dto.mycontent }}</textarea></td>
        </tr>
        <tr>
            <td colspan="2" align="right">
                <input type="button" onclick="" value="목록">
                <input type="button" onclick="location.href='/updateform/{{ dto.id }}'" value="수정">
                <input type="button" onclick="location.href='/delete/{{ dto.id }}'" value="삭제">
            </td>
        </tr>
    </table>
```