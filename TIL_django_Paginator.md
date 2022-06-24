# paginator



- 페이지 목록 구분하기
- 전체 글 부분을 보는 부분에서

## view.py 에서 paginator import 하고 코드 작성

```python
from django.core.paginator import Paginator #대문자 주의

def index(request):
    myboard_all = MyBoard.objects.all().order_by('-id') # id를 기준으로 역순 정렬하여 전부 가져옴

    paginator = Paginator(myboard_all, 5) # 객체 생성 글 5개씩 보여주기
    page_num = request.GET.get('page', '1') # get 방식으로 page를 가져오고, 없으면 1부터 시작

    #페이지에 맞는 모델
    page_obj = paginator.get_page(page_num)
    print(type(page_obj))

    # 총 모델 수
    print(page_obj.count)
    # 총 페이지 수
    print(page_obj.paginator.num_pages)
    # 총 페이지 range 객체
    print(page_obj.paginator.page_range)
    # 다음 페이지 유무
    print(page_obj.has_next())
    # 이전 페이지 유무
    print(page_obj.has_previous())

    try:
        # 다음 페이지 숫자 (없으면 에러남)
        print(page_obj.next_page_number())
        # 이전 페이지 숫자
        print(page_obj.previous_page_number())
    except:
        pass

    # The 1-based index of the first item on this page
    print(page_obj.start_index())
    # The 1-based index of the last item on this page
    print(page_obj.end_index())

    return render(request, 'index.html', {'list': page_obj})
```





### 2. index.html

- table 밑에 페이징 부분 만들어주기

```html
<!--처음으로-->
    <a href="?page=1">처음</a>
    <!-- 이전 페이지 -->
    {% if list.has_previous %}
        <a href="?page={{ list.previous_page_number }}">이전</a>
    {% else %}
        <a>이전</a>
    {% endif %}
    <!-- 페이징 -->
    {% for page_num in list.paginator.page_range %}
        {% if page_num == list.number %}
            <b>{{ page_num }}</b>
        {% else %}
            <a href="?page={{ page_num }}">{{ page_num }}</a>
        {% endif %}
    {% endfor %}
    
    <!-- 다음 페이지-->
    {% if list.has_next %}
        <a href="?page={{ list.next_page_number }}">다음</a>
    {% else %}
        <a>다음</a>
    {% endif %}

    <!--끝으로-->
    <a href="?page={{ list.paginator.num_pages }}">끝</a>
```

### 3. 범위 데이터 만들어보기 (shell 이용)

```
(myweb) C:\\workspaces\\workspace_django\\myboard>python manage.py shell
>>> from myboard.models import MyBoard 
>>> from django.utils import timezone
for i in range(1, 100):                                                     
...     temp = MyBoard(myname=i, mytitle=i, mycontent=i, mydate=timezone.now())
...     temp.save()
```

- python shell에서 위와 같이 반복문 이용하여 만들어주기

> ?page=~ ⇒ get방식에서 정보를 전달할 때 ?key=value형태임