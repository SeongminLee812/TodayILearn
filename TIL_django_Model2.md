# mysql과 연결하기

> 가상황경을 만든 이유? 프로젝트마다 필요한 파이썬 버전이 다르기 때문

**파이썬 모듈 모아놓은 공식문서[https://pypi.org/]**

[참고 링크] 

Making queries https://docs.djangoproject.com/en/4.0/topics/db/queries/ 

QuerySet API reference https://docs.djangoproject.com/en/4.0/ref/models/querysets/ 

Model Instance reference https://docs.djangoproject.com/en/4.0/ref/models/instances/

# setting

- mysql서버가 있는 상태에서해야함

### 1. myboard 프로젝트 만들기

### 2. `django-admin startproject myboard`

### 3. `pip install mysqlclient` 로 mysql과 연결해주는 라이브러리 설치하기

- NameError: name '_mysql' is not defined가 나온다면 pip install말고 conda install로 해보자

### 4. settings.py의 DATABASES 설정

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mysql',
        'USER' : 'root',
        'PASSWORD' : '****',
        'HOST' : 'localhost',
        'PORT' : '3306',
    }
}
```

- mysql과 django를 연결하기 위해 필요한 정보를 입력해줘야함
  - user, password, host, port
  - mysql데이터 베이스와,  django 둘다 서버기 때문
  - 여기서의 NAME 은 database의 이름이다.(서버이름 X)

### 5. **INSTALLED_APPS에도 설정**

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myboard',
]
```

### 6. **myboard 앱폴더 안에 [models.py](http://models.py) 만들기**

```python
from django.db import models

class MyBoard(models.Model):
    myname = models.CharField(max_length=100)
    mytitle = models.CharField(max_length=500)
    mycontent = models.CharField(max_length=2000)
    mydate = models.DateTimeField()

    def __str__(self): #원래 class를 프린트하면 class와 메모리가 나오는데 str로 재정의하면 그 값이 나옴
        return str({'myname': self.myname, 'mytitle':self.mytitle, 'mycontent':self.mycontent, 'mydate':self.mydate})
```

- str ⇒ print로 값을 출력하기 위함

### 7. migrate

- 터미널에서

  ```
  (myweb) C:\\workspaces\\workspace_django\\myboard>python manage.py makemigrations myboard
  ```

   명령어로 migragions 만들기

  - 쿼리문으로 변경해주기 위함.

- `python manage.py migrate` 로 적용하기

mysql클라이언트와 mysql서버는 다름, 우리가 cmd로 키는 것(mysql -u root -p)이 mysql 클라이언트임. → 위 3번에서 설치한 `mysqlclient` 또한 클라이언트 라이브러리임

------

# mysql 연결하기

### 1. index.html 만들기

- myboard앱 폴더 안에 templates폴더 만들고 그 안에 index.html 만들기

```html
<h1>Hello, {{ request.session.myname | default:'Django' }} with MySQL</h1>

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
        <th colspan="4">-------작성된 글이 없습니다-------</th>
    </tr>
    {% else %}
        {% for row in list %}
    <tr>
        <td>{{ row.id }}</td> 
        <td>{{ row.myname }}</td>
        <td><a href="#">{{ row.mytitle }}</a></td>
        <td>{{ row.mydate | date:'Y-m-d' }}</td>
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

→ views.py에서 request에 전달해주는 값의 타입은 QuerySet, 즉 list형태로 되어있고, 리스트안의 값은 각 models 객체 하나로 이루어짐(데이터베이스 테이블의 한 행(row)) ⇒ for문은 각 model객체를 하나씩 가져오는 것임

- id ⇒ primary key를 지정해주지 않아서 migration과정에서 자동 생성됨
- `{{ row.mydate | date:'Y-m-d' }}` | 뒤에 date의 타입을 지정해줌. (년월일), 속성이나, 기본값을 지정도 가능

### 2. views.py

```python
from django.shortcuts import render
from .models import MyBoard

def index(request):
    return render(request, 'index.html', {'list':MyBoard.objects.all().order_by('-id')})
```

- table에서 위에서 아래로 긁어오기 때문에, 최신글이 가장 아래에 있는 구조이다.
- order_by(’id’)를 통해 id를 기준으로 정렬하고, 앞에 -를 붙여주면 desc(내림차순) 정렬

### 3. urls.py

```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
]
```

# create

## 1. templates폴더에 insert.html만들기

```html
<h1>Insert</h1>

    <form action="/insertres/" method="post">  {% csrf_token %}
        <table>
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
                    <input type="button" value="취소" onclick="">
                    <input type="submit" value="작성">
                </td>
            </tr>
        </table>
    </form>
```

- **post에 submit으로 넘기는 경우 name속성 반드시필요!**
- 나중에 작성자 부분에는 login한 사람의 id를 띄워줘야한다.
- action 부분의 경로는 /insertres/로 해줘야함
  - 이미 insertform/ 경로에 있기 때문에 루트경로 + insertres/로 보내주자

## 2. views.py

```python
def insert_form(request):
    return render(request, 'insert.html')
```

## 3. urls.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('insertform/', views.insert_form, name='insert'),
]
```

## 4. index.html에서 a태그 수정

```python
<tr>
    <td colspan="4" align="right">
        <input type="button" value="글작성" onclick="location.href='{% url 'insert' %}'">
    </td>
</tr>
```

`location.href='{% url 'insert' %}'` ⇒ urls.py에서 name이 insert인 path를 찾아 요청함.



# 위와 같이 받아서 DB에 저장하기

### 1. views.py에서 함수 정의하기

- insertform으로부터 submit 받아서 db에 저장하는 함수

```python
from django.utils import timezone
from django.shortcuts import render, redirect

def insert_res(request):
    myname = request.POST['myname']
    mytitle = request.POST['mytitle']
    mycontent = request.POST['mycontent']

'''
   myboard = MyBoard(myname=myname, mytitle=mytitle, mycontent=mycontent, mydate=timezone.now())
   myboard.save()
   '''

    result = MyBoard.objects.create(myname=myname, mytitle=mytitle, mycontent=mycontent, mydate=timezone.now())
		if result:
        return redirect('index')
    else:
        return redirect('insert')
```

- timezone과 redirect import하기
- 객체.POST[’name’] ⇒ 객체에 post방식으로 전달된 name이 name인 요소의 값을 가져옴
- 주석부분과 같이 객체를 지정해서 save()할 수도 있으나, save는 값을 반환하지 않는다. 따라서, 저장이 되었는지, 안되었는 지 확인할 수 없다.
- redirect로 보내는 페이지는 기획자, 개발자의 의도에 따라 보내면 된다.

```python
print(result)
>>> {'myname': '12323',
 'mytitle': '123232',
 'mycontent': '312312312',
 'mydate': datetime.datetime(2022, 6, 20, 2, 24, 13, 666221, tzinfo=datetime.timezone.utc)}
```

- 위 result 프린트하니, 실제 만들어진 models 객체가 result로 print됨 → 아까 models.py에서 def \_\_str\_\_(self): 을 했기 때문에 값이 출력될 수 있는 것임

# 게시글 보여주는 화면 만들기

## 1. detail.html 생성

```html
<h1>Detail</h1>

<table border="1">
    <tr>
        <th>작성자</th>
        <td><input type="text" readonly value="{{ dto.myname }}"></td>
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
            <input type="button" value="목록" onclick="">
            <input type="button" value="수정" onclick="">
            <input type="button" value="삭제" onclick="">
        </td>
    </tr>
</table>
```

## 2. views.py에서 함수 정의하기

```python
def detail(request, id):
    myboard_one = MyBoard.objects.get(id=id)
    print(myboard_one)
    return render(request, 'detail.html', {'dto': myboard_one})
```

- id를 인자로 받아,
- MyBoard.objects.get(id=id)⇒ table의 primary key인 id를 이용
- dto : Data Transfer Object의 약자로 여기서는 그냥 변수



## 3. urls.py에서 처리할 views 지정

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('insertform/', views.insert_form, name='insert'),
    path('insertres/', views.insert_res),
    path('detail/<int:id>', views.detail, name='detail'),
]
```

- `detail<int:id>` detail/숫자 ⇒ detail 뒤의 값을 int 타입으로 id 변수에 저장함

## 4. index.html에서 제목 눌러서 넘어가는 a 태그를 수정하기

```html
<tr>
    <td>{{ row.id }}</td>
    <td>{{ row.myname }}</td>
    <td><a href="{% url 'detail' row.id %}">{{ row.mytitle }}</a></td>
    <td>{{ row.mydate | date:'Y-m-d' }}</td>
</tr>
```

- `"{% url 'detail' row.id %}"` row.id는 중괄호로 감싸지 않는다. 이미 중괄호로 파이썬 명령어임을 표현하고 있기 때문에

### objects.all()과 objects.get()으로 불러오는 객체의 차이

```python
# 1번
print(MyBoard.objects.all().order_by('-id'))
<QuerySet [<MyBoard: {'myname': '12323', 'mytitle': '123232', 'mycontent': '312312312', 'myda
te': datetime.datetime(2022, 6, 20, 2, 24, 13, 666221, tzinfo=datetime.timezone.utc)}>, <MyBo
ard: {'myname': '12', 'mytitle': '1233', 'mycontent': '32323', 'mydate': datetime.datetime(20
22, 6, 20, 2, 24, 6, 4600, tzinfo=datetime.timezone.utc)}>]>

# 2번
print(MyBoard.objects.get(id=id))
{'myname': '12', 'mytitle': '1233', 'mycontent': '32323', 'mydate': datetime.datetime(2022, 6, 20, 2, 24, 6, 4600, tzinfo=datetime.timezone.utc)}
```

views에서 함수 정의할 때 두가지를 print 해보니

- 1번 objects.all() => QuerySet이라는 객체 안에 모델객체들이 저장되어있음.
- 2번 objects.get(id=id) => 모델객체 하나



# 수정하는 화면 만들기

- 두개의 프로세스로 구성됨
  1. 수정할 값 보여주기(이미 저장된)
  2. 수정된 값을 전달

## 1. update.html만들기

```html
<h1>Update</h1>

<form action="/updateres/" method="post"> {% csrf_token %}
    <input type="hidden" name="id" value="{{ dto.id }}">
    <table border="1">
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
             <td><textarea col="60" rows="10" name="mycontent">{{ dto.mycontent }}</textarea></td>
        </tr>
         <tr>
            <td>
                <input type="button" value="취소" onclick="location.href='/'">
                <input type="submit" value="수정">
            </td>
        </tr>
    </table>
</form>
```

## 2. views.py

```python
def update_form(request, id):
    myboard_one = MyBoard.objects.get(id=id)
    return render(request, 'update.html', {'dto': myboard_one})
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('insertform/', views.insert_form, namㄷ='insert'),
    path('insertres/', views.insert_res),
    path('detail/<int:id>/', views.detail, name='detail'),
    path('updateform/<int:id>/', views.update_form, name='update'),
]
```

## 3. [urls.py](http://urls.py)

```html
<td colspan="2" align="right">
    <input type="button" value="목록" onclick="">
    <input type="button" value="수정" onclick="location.href='{% url 'update' dto.id %}'">
    <input type="button" value="삭제" onclick="">
</td>
```

- {% url %} 이용하여 경로요청과 데이터 전달가능

### 4. detail.html 수정하기

```html
<tr>
    <td colspan="2" align="right">
        <input type="button" value="목록" onclick="location.href='/'">
        <input type="button" value="수정" onclick="location.href='{% url 'update' dto.id %}'">
        <input type="button" value="삭제" onclick="">
    </td>
</tr>
```

## update.html에서 수정버튼 누른 후 작동하게하기

### 1. form action으로 /updateres/로 요청하기

### 2. views.py에서 함수 정의

```python
def update_res(request):
    mytitle = request.POST['mytitle']
    mycontent = request.POST['mycontent']
    id = request.POST['id']

''' myboard = MyBoard.objects.get(id=id)
    myboard.mytitle = mytitle
    myboard.mycontent = mycontent
    myboard.save()    '''

    myboard = MyBoard.objects.filter(id=id)
    result_title = myboard.update(mytitle=mytitle)
    result_content = myboard.update(mycontent=mycontent)

    if result_title + result_content == 2:
        return redirect(f'/detail/{id}')
    else:
        return redirect(f'/updateform/{id}')
```

> [get과 filter의 차이](https://docs.djangoproject.com/en/4.0/topics/db/queries/#retrieving-objects) → filter는 여러개의 model 객체들을 QuerySet으로 가져온다.(값이 하나라도) → get은 model 객체 하나만 가져옴

- 문자열 포멧팅 f’ ‘ 사용해서 id값 넣어줌
- 위에서 ``print(f'title update : {result_title} / content update : {result_content}')`로 찍으면 각각 1씩 나온다 → update 함수는 수정이 수행된 row의 개수를 반환
- 주석 내용과 같이 객체.속성 = 값 하고 save()로 update할 수 있으나, save()는 값을 반환하지 않기 때문에 잘 됬는 지 확인할 방법이 없어 update를 쓴 후 확인한다.

### 3. urls.py에서 연결

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('insertform/', views.insert_form, name='insert'),
    path('insertres/', views.insert_res),
    path('detail/<int:id>/', views.detail, name='detail'),
    path('updateform/<int:id>/', views.update_form, name='update'),
    path('updateres/', views.update_res),
]
```

# 삭제하기

## 1. views.py에서 함수 만들기

```python
def delete_proc(request, id):
    result_delete = MyBoard.objects.filter(id=id).delete()
    print(result_delete)

    if result_delete[0]:
        return redirect('index')
    else:
        return redirect(f'/detail/{id}')
```

## 2. urls.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('insertform/', views.insert_form, name='insert'),
    path('insertres/', views.insert_res),
    path('detail/<int:id>/', views.detail, name='detail'),
    path('updateform/<int:id>/', views.update_form, name='update'),
    path('updateres/', views.update_res),
    path('delete/<int:id>', views.delete_proc, name='delete'),
]
```

- {% url %}로 경로 요청하기 위해서는 name 속성 지정해야한다.



# insert와 update의 함수를 하나로 바꾸기(get, post이용)

## .insert 함수 수정

### 1. urls.py에서 경로 하나로 합치기

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    #path('insertform/', views.insert_form, name='insert'),
    #path('insertres/', views.insert_res),
    path('insert', views.insert_proc, name='insert'),
    path('detail/<int:id>/', views.detail, name='detail'),
    path('updateform/<int:id>/', views.update_form, name='update'),
    path('updateres/', views.update_res),
    path('delete/<int:id>', views.delete_proc, name='delete'),
]
```

### 2. insert.html의 form action부분 수정

```html
<form action="{% url 'insert' %}" method="post">
```

### 3. views.py에서 함수만들기

```python
def insert_proc(request):
    if request.method == 'GET':
        return render(request, 'insert.html')
    else:
        myname = request.POST['myname']
        mytitle = request.POST['mytitle']
        mycontent = request.POST['mycontent']

        result = MyBoard.objects.create(myname=myname, mytitle=mytitle, mycontent=mycontent, mydate=timezone.now())

        if result:
            return redirect('index')
        else:
            return redirect('insert')
```

- if request.method 부분으로 get방식이냐 post방식이냐에 따라 작동하는 것을 달라지게함.



## update 함수 바꾸기

### 1. urls.py 수정

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    #path('insertform/', views.insert_form, name='insert'),
    #path('insertres/', views.insert_res),
    path('insert', views.insert_proc, name='insert'),
    path('detail/<int:id>/', views.detail, name='detail'),
    # path('updateform/<int:id>/', views.update_form, name='update'),
    # path('updateres/', views.update_res),
    path('update/<int:id>', views.update_proc, name='update'),
    path('delete/<int:id>', views.delete_proc, name='delete'),
]
```

### 2. update.html 수정

```html
<h1>Update</h1>

<form action="{% url 'update' dto.id %}" method="post"> {% csrf_token %}
<!--<input type="hidden" name="id" value="{{ dto.id }}">-->
    <table border="1">
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
            <td colspan="2">
                <input type="button" value="취소" onclick="">
                <input type="submit" value="수정">
            </td>
        </tr>
    </table>
</form>
```

- 3번 views.py에서 id를 인자로 받을 것이기 때문에 여기서 id를 post를 보낼 필요가 없음



### 3. views.py

```python
def update_proc(request, id):
    if request.method == 'GET':
        myboard_one = MyBoard.objects.get(id=id)
        return render(request, 'update.html', {'dto': myboard_one})
    else:
        mytitle = request.POST['mytitle']
        mycontent = request.POST['mycontent']
        # id = request.POST['id']

        myboard = MyBoard.objects.filter(id=id)
        result_title = myboard.update(mytitle=mytitle)
        result_content = myboard.update(mycontent=mycontent)

        if result_title + result_content == 2:
            return redirect(f'/detail/{id}')
        else:
            return redirect('update')
```

- update.html에서 id를 post로 보내주지 않기 때문에 id 부분을 지워줌