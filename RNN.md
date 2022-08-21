# RNN
- For Sequential data(데이터의 순서도 데이터의 일부임)
- 데이터의 순서가 중요한 데이터
	- 단어(h->e->l->l->o)
- 기존 Neural Network는 position index를 추가해서 순서 정보를 받아서 학습했었음 -> 그러나 사람의 언어와 같이 순서에 얽혀있는 복잡한 구조를 모델이 학습하기 어려움 => RNN 설계
- 첫번째 입력값이 셀 0에서 출력이 되고, 다른 가지의 값이 다음 셀로 전달(hiddenstate)
- 그 전의 입력값의 처리 결과를 반영 -> 모델이 처리 순서를 기억함
- RNN은 모든 셀이 파라미터를 공유함. (입력된 단어가 길든 짧든)
- RNN의 설계
		- LSTM, GRU

![[Pasted image 20220821174129.png]]
- 기본적으로 셀 A에서 함수 연산이 발생
	- 전 단계의 hidden state와 현 단계의 입력 값을 파라미터로 받는 함수로 ht를 출력
tanh = 하이퍼볼릭탄젠트(활성화 함수)

복잡한 셀을 쓰면 더 많은 학습 자원이 필요함
- 셀의 복잡도(오른쪽으로 갈 수록 복잡)
RNN ->  GRU -> LSTM

![[Pasted image 20220821174607.png]]
- one to many : 하나의 입력에 대해서 여러개의 출력 ( 하나의 이미지가 들어가고, 여러 단어(문장)가 출력된다.)
- many to one : 여러개의 입력값이 있고 출력값은 하나 (여러개의 단어들(문장)이 입력되면 하나의 값(감정 label 등)이 나옴) 
- many to many : 번역 테스크 (한문장이 다 들어오고 그 문장을 다 들은 이후에 다른 문장 생성)
- many to many : 비디오(이전 이미지에 없었던 일들이 발생하는 경우 출력되는 값에서 어떤 변화가 있었는지 인식하고 분석)


# RNN in PyTorch
```
rnn = torch.nn.RNN(input_size, hidden_size)

outputs, _status = rnn(input_data)
```
- 윗줄 : 셀 a를 선언하는 과정
- 아랫줄 : xt를 a에 집어 넣고, ht를 반환받는 과정 
- 그 위의 생략된 줄 : 텐서를 맞춰주는 과정(tensor manipulation)

## Input
![[Pasted image 20220821175758.png]]
- h라는 문자를 표현하기 위해서는 4개의 차원을 가진 벡터가 필요
- input_size : 인베딩 벡터의 dimension 
- A가 선언될 때부터 input_size를 알려줘야함

## Hidden State

![[Pasted image 20220821175954.png]]
- output 데이터로 몇개의 차원을 가진 벡터를 출력할 것인지
- 여러가지의 감정 중에서 추론할 수 있는 감정의 개수가 hidden_size의 개수가 될 것임
- hidden state : (숨겨진 상태) - 셀 A의 입장에서 ht는 바깥으로 내논 상태, 바깥으로 내놓 지 않고 다음 셀에 전달하는 것을 hidden state라고 함
	- hidden_size를 왜 outputs_size가 따라가는지?
		:출력 직전에 똑같은 값이 두개의 가지로 나눠짐
		output size = hidden size

## Sequence Length

![[Pasted image 20220821180420.png]]
![[Pasted image 20220821180552.png]]
h, e, l, l, o -> 5개의 문자가 입력되기에 Sequence Length는 5다
- Sequence Length를 모델에게 알려주지 않아도, PyTorch는 알아서 인식함

## Batch Size
![[Pasted image 20220821180711.png]]
- 여러개의 데이터를 하나의 batch로 묶어서 모델에게 학습
- Batch Size 역시 모델이 알아서 파악하게 됨
- input data만 잘 굿어하기


# 'Hihello' example
- 'h', 'i', 'h', 'e', 'l', 'l', 'o'
- 캐릭터가 들어오면 다음 캐릭터를 예측하는 모델

### 문자를 표현하는 방법
- one-hot encoding
![[Pasted image 20220821182453.png]]
x_data에 o가 없는 이유 -> 다음 문자를 예측하는 모델이기 때문에 o는 input으로 사용되지 않음
y_data는 어차피 다음 문자를 예측하는 것이기 때문에 첫 문자를 빼고 정답으로 넣음

## Cross Entropy Loss
- cartigorical output에 많이 사용됨
- 첫번째 파라미터 -> 모델의 아웃풋, 두번째 파라미터 -> 정답

\[np.eye(dic_size)[x] for x in x_data\]  => dic_size만큼의 벡터를 만들어, index 위치의 스칼라만 1로 만들고 나머지는 0으로 만드는 코드
![[Pasted image 20220821183517.png]]
- identity matrix를 만들 수 있음

```
sample = 'if you want you'

char_set = list(set(sample))
char_dic = {c : i for i, c in enumerate(char_set)}

# hyper parameters
dic_size = len(char_dic)
hidden_size = len(char_dic)
learning_rate = 0.1

# data setting
sample_idx = [char_dic[c] for c in sample]
x_data = [sample_idx[:-1]]
x_one_hot = [np.eye(dic_size)[x] for x in x_data]
y_data = [sample_idx[1:]]
X = torch.FloatTensor(x_one_hot)
Y = torch.LongTensor(y_data)
```

```
rnn = torch.nn.RNN(dic_size, hidden_size, batch_first = True)

criterion = torch.nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(rnn.parameters(), learning_rate)

#start training
for i in range(100):
	optimizer.zero_grad()
	outputs, _status = rnn(X)
	loss = criterion(outputs.view(-1, dic_size), Y.view(-1))
	loss.backward()
	optimizer.step()
	result = outputs.data.numpy().argmax(axis=2)
	result_str = ''.join([char_set[c] for c in np.squeeze(result)])
	print(i, "loss :", loss.item(), 'prediction :', result, 'true Y :', Y.data, 'prediction_str :', result_str)
```

`optimizer.zero_grad`를 꼭 해줘야함. 해주지 않으면 기존의 gradient에 축적이 되어 학습이 정상적으로 진행되지 않는다.
`optimizer.step()` => 옵티마이저 업데이트
argmax => 가장 확률이 큰 index를 가져옴
np.sqeeze는 shape에서 dimension이 1인 축을 없애주는 함수
