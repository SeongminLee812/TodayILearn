# File_upload

## 기본세팅

### 1. 에서 프로젝트만들기

```
django-admin startproject updown
```

### 2. settings.py에서 설정

- [settings.py](http://settings.py) 에 media url과 root 지정하기

```python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR/'media'
```

### 3. templates 폴더, media폴더 만들기

- media 폴더도 만듬



# upload 구현하기

## 1. index.html 템플릿 만들기

```html
<h1>File Upload</h1>

    <form action="upload/" method="post" enctype="multipart/form-data"> {% csrf_token %}
        <input type="file" name="uploadfile">
        <br>
        <input type="submit" value="업로드">
    </form>
```

- file upload할 때, 반드시  enctype 잡아줘야한다.

## 2. views.py만들기 (updown 앱 안에)

- 모듈 임포트 시키기
- 함수 정의하기

```python
from django.shortcuts import render
from django.core.files.base import ContentFile
from django.core.files.storage import default_storage

def index(request):
    return render(request, 'index.html')

def upload_proc(request):
    upload_file = request.FILES['uploadfile'] # => index.html에서 form태그 안의 input태그

    uploaded = default_storage.save(upload_file.name, ContentFile(upload_file.read()))

    return render(request, 'download.html', {'filename': uploaded})
```

- 실제 db에 저장할 때는 파일이름으로 그냥하지 않고, 날짜시간 + 로그인 아이디 등으로 파일이름을 지정하는 경우가 많다.
- .read()는 파이썬에서 파일 읽기와 같은 것이다.

## 3. urls.py

```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('upload/', views.upload_proc, name='upload'),
]
```

- media폴더에 파일이 저장됨을 확인가능



# download 구현하기

## 1. download.html 만들기

```html
<h1>Download</h1>

    <input type="button" value="download" onclick="location.href='/download/{{ filename }}'">
```

## 2. views.py에서 함수 정의

```python
from django.http import HttpResponse

def download_proc(request, filename):

    return HttpResponse(default_storage.open(filename).read(), content_type='application/force-download')
```

## 3. urls.py

```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('upload/', views.upload_proc, name='upload'),
    path('download/<str:filename>', views.download_proc, name='download'),
]
```



## | settings.py에서 지역설정

```python
# Internationalization
# <https://docs.djangoproject.com/en/4.0/topics/i18n/>

LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'

USE_I18N = True

USE_TZ = False #모델에서 사용하는 tz => false로 바꿔 줘야함
```

- setting의 # Internationalization 부분에서 언어와 time_zone을 설정 가능하다!

- 그리고 USE_TZ = False 반드시 설정해줘야한다!