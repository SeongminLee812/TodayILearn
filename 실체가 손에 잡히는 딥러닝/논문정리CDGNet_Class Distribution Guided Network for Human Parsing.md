아주대 황원준 교수님
- divide and conquer 알고리즘 사용으로 human parsing 문제 해결
- CDG Net(Class Distribution Guided) : 
- vertical label과 horizontal label에 분배해줌(2D 공간적인 문제를 1차원으로 줄이기 위해)
- ![[Pasted image 20221102155529.png]]
- 수직과 수평 클래스를 분배하고, 방향을 이용하여 학습 시킨다.
- 이 guided feature들을 merge한다. 
- backbone network feature에 겹쳐놓는다
	- backbone network - 입력: 뇌를 통해, 출력: 팔, 다리 라고 생각하면 backbone은 입력이 처음 들어와서 출력에 관련된 모듈에 처리된 입력을 보내주는 역할이라고 생각할 수 있다.
