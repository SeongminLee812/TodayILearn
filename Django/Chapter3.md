### 카테고리와 맛집 Model
- '카테고리가 다수의 맛집을 가지는 방법'
	- Category의 모델을 정의할 때 '카테고리의 이름' 뿐 아니라 어떠한 맛집을 가지는 지에 대한 정보도 포함하도록 모델을 정의해야한다
- **'하나의 맛집이 하나의 카테고리에 속하는 방법'**
	- 현재 Category의 모델 정의가 아닌, 추후 하게 될 Restaurant 모델 정의에서 고려해 주어야 한다.

-> 두번째 방법 채택, Category의 모델을 정의하는 데 단순히 카테고리 이름만 가지도록 모델 정의

## Restaurant Model
- ForeignKey
	`category = models.ForeignKey(Category, on_delete=model.SET_DEFAULT, default=8)`
	- Category 모델을 참조한다는 의미
	- Category 모델에 존재하는 데이터만 값으로 받는 다는 것
	- on_delete
		- 참조하는 모델이 삭제 되었을 때 해당 요소는 어떻게 할지 물어보는 항목
		- CASCADE : 참조하는 요소가 삭제될 때 이를 참조하는 모든 요소도 함께 삭제시키는 것
		- PROTECT : 참조하는 요소를 삭제하려고 할 때 해당 요소를 참조하는 요소가 하나라도 존재한다면 에러 발생(ProtectedError)
		- SET_NULL : 참조되는 요소가 삭제될 때 이를 참조하는 요소에 대해서 참조 값을 NULL로 설정 (이를 설정하려면 추가로 해당 값을 null로 가질 수 있도록 null=True 설정을 해줘야함)
		- SET_DEFAULT : 참조되는 요소가 삭제될 때 이를 참조하는 요소에 대하셔 참조값을 설정해 둔 DEFAULT 값으로 설정

- urlspatterns 작성 시 유의사항
- restaurantDetail/updatePage/update가 restaurantDetail/updatePage/<str:res_id>보다 위에 있어야한다. 그렇지 않으면 <str:res_id>로 먼저 처리하기 때문에 다른 함수로 처리가 넘어가 에러가 발생한다.
```python
urlpatterns = [  
   path('', views.index, name='index'),  
   path('restaurantDetail/<str:res_id>', views.restaurantDetail, name='resDetailPage'),  
   path('restaurantDetail/updatePage/update', views.Update_restaurant, name='resUpdate'),  
   path('restaurantDetail/updatePage/<str:res_id>', views.restaurantUpdate, name='resUpdatePage'),
```

- HttpResponseRedirect의 reverse의 인수 kwargs
- resDetailPage라는 이름을 갖는 url 뒷부분에 <str:res_id> 부분에 보내줄 데이터를 정의
```python
return HttpResponseRedirect(reverse('resDetailPage', kwargs={'res_id':resId}))
```

# 배포
- PythonAnyWhere이용
- 배포과정이 간단하며, 소규모 자원에 대해서는 무료로 제공

# Email 보내기 과정에서 계정 숨기기
- API 키 혹은 mysql password 등 중요한 자료 숨기기
- .json 파일로 만들어서 그 파일은 .gitignore로 보내는 방법

```python
import json  
from collections import OrderedDict  
  
file_data = OrderedDict()  
  
file_data['email'] = '이메일@이메일.com'  
file_data['password'] = '비밀번호'  
  
print(json.dumps(file_data, ensure_ascii=False, indent='\t'))  
  
with open('email.json', 'w', encoding='utf-8') as make_file:  
   json.dump(file_data, make_file, ensure_ascii=False, indent='\t')
```

아래 코드로 읽어오기
```python
import json  
with open('email.json', 'r') as f:  
   json_data = json.load(f)  
email = json_data['email']  
password = json_data['password']
```

# 배포시 신경써야할 점
- requirements
	- 라이브 러리들의 목록을 가진 파일
- git ignore

- Django의 관점에서 본다면...
	- static 파일에 대한 처리
	- settings 파일에 대한 설정
	- wsgi에 대한 설정
- 배포하는 다른 방법
	- heroky, aws(아마존), gcp(구글), azure(마이크로소프트)

# 장고 모델 구성 시 다른 요소를 참조하는 요소
1. ForeignKey(다대일 관계)
2. ManyToManyField (다대다 관계)