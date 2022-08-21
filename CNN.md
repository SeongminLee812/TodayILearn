# Convolution

![[Pasted image 20220821203523.png]]
- 이미지 위에서 stride 값 만큼 filter(kernal)을 이동시키면서 겹쳐지는 부분의 각 원소의 값을 곱해서 모두 더한 값을 출력으로 하는 연산


![[Pasted image 20220821203642.png]]
![[Pasted image 20220821203748.png]]

## Stride and Padding
- stride : filter를 한번에 얼마나 이동시킬 것인가 
	- stride가 2라면 연산 후 2칸 이동
- padding : zero-padding
	- pad를 끼운다( 띠가 한번 둘러진다고 생각)
![[Pasted image 20220821203941.png]]



![[Pasted image 20220821204140.png]]

![[Pasted image 20220821204124.png]]
in_channels = 1, out_channels=1, kernel_size =3(2x4라면 kernel_size = (2, 4))

## 입력의 형태
`conv = nn.Con2d(1, 1, 3)`
`out = conv(inputs)`

- input type : torch.Tensor
- input shape : (N x C x H x W )
						(batch_size, channel, height, width)
						

## Convolution의 Output 크기
![[Pasted image 20220821204948.png]]

## Neuron과 Convolution
Convolution filter => Perceptron의 웨이트 값으로 들어감
****![[Pasted image 20220821211318.png]]

실제 출력은 값 + bias

## Pooling

![[Pasted image 20220821211440.png]]
- max pooling : size를 2로 잡으면 2x2 사이즈 않에서 가장 큰 값을 가지고 나옴
- Average pooling : 평균을 계산해서 가지고 나옴 

![[Pasted image 20220821211536.png]]
- kernel_size 설정, 나머지는 주로 Default값 사용


## CNN implementation
-![[Pasted image 20220821211613.png]]

