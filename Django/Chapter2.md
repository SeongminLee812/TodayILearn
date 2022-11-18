# To do list 만들기

1. django-admin startproject 프로젝트명
2. python manage.py startapp 앱 명
3. settings.py의 INSTALLED_APPS에 만든 앱 넣어주기

### url  설정하기

# MVC 패턴
- **Model, View, Controller**로 이루어진 구조
- 소프트웨어 개발 방법론 중 하나.
- 많은 웹사이트와 웹 프레임워크들이 MVC  패턴을 기반으로 함.
- __Model__ : '어떤 것', 웹에서 백엔드에 존재하는 데이터베니스
- __View__ : '보여 주는 것', 웹에서의 화면
- __Controller__ : Model과 View를 컨트롤하는 역할. 사용자가 View를 통해 Controller에게 요청을 보내면, Controller는 내부적으로 특정한 처리를 진행
- ![[Pasted image 20221118161948.png]]
- ![[Pasted image 20221118162038.png]]
- models.py에서 테이블을 만든 이후에는
- `python manage.py makemigrations`
- `python manage.py migrate`를 해줘야한다.
- python manage.py db shell 명령어로 db에 접근 가능

## app을 나누는 이유
- app 구조화의 핵심은 유지 보수이다.
- 기능을 가져와 사용할 수 있다. 
- 기능별로 app 단위를 나눠 개발하는 것이 좋음

# MVC 패턴
### model
- models.py에 만드려는 model을 클래스로 만들고 내부 멤버 변수로 데이터의 타입 등을 지정
- makemigrations 명령어로 migrations 파일들을 생성
- migrate 명령어로 migrations 파일을 참조해 데이터 베이스에 테이블 생성
- 즉, models.py 파일에 만든 하나의 model이 데이터베이스에서 하나의 테이블이 되는 개념이며,
- 해당 테이블 내의 데이터 속성들은 우리가 model 클래스의 내부 멤버 변수로 지정해 준 이름과 데이터 타입을 가짐

- 데이터 타입의 종류
	- CharField
		- 문자열을 정의할 때 일반적으로 사용
		- max_length를 지정하여 최대길이(default = None)
	- DataField
		- 날짜 양식에 맞게 저장되는 데이터 타입
		- python의 datetime 라이브러리 형태
	- EmailField
		- 이메일 형식을 가지는 데이터 타입.
		- EmailValidator라는 것을 통해서 입력된 문자열이 이메일 형식인지를 자동으로 체크
	- TextField
		- 문자열을 저장하는 데이터 타입
		- TextField => textarea, CharField => varchar
		- textarea는 varchar보다 더 많은 문자열을 저장할 수 있다.
		- 특정 문자열 함수에 대해서 varchar에서는 적용되지만, textarea에서 적용되지 않는 경우가 있다.
	- IntegerField
		- 숫자를 저장
		- 유효 범위는 -2147483648~2147483647
		- 게시글의 조회수나 추천 수 등을 저장
	- BooleanField
		- True 또는 False를 저장하는 데이터 타입

### View
- Django에서 templates
- 사용자에게 보여주는 화면

### Controller
- Django에서 views.py
- 뒤에서 진행되는 내부적인 로직을 담당

# CRUD
| 이름   | 조작(기능) | SQL    |
| ------ | ---------- | ------ |
| Create | 생성       | INSERT |
| Read   | 읽기       | SELECT |
| Update | 갱신, 수정 | UPDATE |
| Delete | 삭제       | DELETE |

| 기능   | URI                |
| ------ | ------------------ |
| Create | 기본 도메인/create |
| Read   | 기본 도메인/read   |
| Update | 기본 도메인/update |
| Delete | 기본 도메인/delete                   |

# GET vs POST
- "요청에 대한 처리 로직에서 데이터에 대한 변경이 이루어지는가?"
- 해당 요청을 처리하는데 새로운 데이터가 발생(Create)하거나, 기존 데이터가 변경(Update) 또는 삭제(Delete)되는 행위 및 로직이 존재하는 경우 주로 POST 방식으로 처리
| 방식 | URI               | 상세        |
| ---- | ----------------- | ----------- |
| GET  | 기본도메인/write  | 작성 페이지 |
| POST | 기본 도메인/write | 작성 처리            |

### GET
- 주소 값에 전달되는 값이 표시됨
- 데이터가 보이므로 보안에 취약
- 데이터가 주소 값에 표시되야 하므로 데이터 길이제한이 있음(256바이트)
- 전송 속도는 POST보다 빠름

### POST
- 주소 값에 전달되는 값이 표시되지 않음
- 데이터가 보이지 않으므로 보안에 우수
- 데이터를 body안에 포함시켜 서버에 전달하기 때문에 데이터 길이 제한이 없으며 보다 복잡한 형태의 데이터 또한 전송 가능