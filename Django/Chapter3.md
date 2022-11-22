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

