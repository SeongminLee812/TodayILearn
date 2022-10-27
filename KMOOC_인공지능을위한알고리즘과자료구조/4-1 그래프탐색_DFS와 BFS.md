
그래프의노드들을 하나씩 순회하는 방법
## Graph Traversel
- 그래프 순회 / 검색 search라고도 표현됨
- Breadth First Search(BFS) : 너비우선탐색
- Depth First Search(DFS) : 깊이우선탐색 깊이를 우선적으로 늘려감
![[Pasted image 20221026113152.png]]

### BFS 알고리즘
- vertex 선택 - visit 체크 - 이 vertex를 미리 만들어 놓은 queue에 push
	- 큐가 비어있지 않는 동안 반복 : 
		- vertex를 pop
		- 이 vertex에 인접한 vertex가 큐에 들어간 적이 없다면, visit 마크 하고 큐에 푸쉬함.
	- 위 과정을 큐가 빌 때 까지, 
	- 만약 방문하지 않은 vertax가 있다면 그래프는 연결되지 않은 것임

1. 시작 노드를 queue에 넣음
![[Pasted image 20221026113905.png]]

2. 큐에서 A를 빼고 인접한 노드들을 순서대로 추가함
![[Pasted image 20221026113925.png]]

3. front에서 하나를 빼고 인접한 노드를 차례대로 삽입(A와 C는 이미 들어갔기 때문에 추가하지 않고, 한번도 들어가지 않은 D만 추가함)
![[Pasted image 20221026114028.png]]

4. 위 과정을 큐가 빌 때 까지 반복
![[Pasted image 20221026114122.png]]
![[Pasted image 20221026114135.png]]
![[Pasted image 20221026114150.png]]
![[Pasted image 20221026114157.png]]

### DFS
- BFS와 알고리즘은 같으나 자료구조를 stack을 사용
![[Pasted image 20221026114307.png]]
![[Pasted image 20221026114318.png]]
1. A를 stack에 넣고 A를 pop하면서 인접노드 push
2. 최상단 pop 및 인접노드 push
- A에서 멀어지는 방향으로 방문 -> 깊이 우선

- DFS order, BFS order가 꼭 유니크한 것은 아님
- 다른 순서대로 쌓게 되면 다른 순서가 된다
- 하지만 이것 역시도 옳은 결과임
![[Pasted image 20221026114627.png]]

connected component를 구하기 위해 BFS나 DFS활용 가능
![[Pasted image 20221026114826.png]]
예를 들어 3번을 처음 노드로 지정하고 BFS나 DFS를 수행하면, 3, 1, 7, 4를 순회한 후에 스택or큐가 비게 된다. -> 4개만 connected 되어있다.