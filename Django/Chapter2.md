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
