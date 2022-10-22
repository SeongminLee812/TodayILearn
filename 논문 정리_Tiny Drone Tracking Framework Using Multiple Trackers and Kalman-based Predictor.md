![[08_Haechul.pdf]]

# 드론을 이용한 object tracking
[  

### Tiny Drone Tracking Framework Using Multiple Trackers and ...

https://journals.riverpublishers.com › download

](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiJzNLVnvP6AhVJyGEKHU0IDNcQFnoECA4QAQ&url=https%3A%2F%2Fjournals.riverpublishers.com%2Findex.php%2FJWE%2Farticle%2Fdownload%2F5455%2F10209%2F37913&usg=AOvVaw3XZ1BoYl9eL7LidVrUKYW_)

경희대학교 김휘용 교수님
## 1. Introduction
1. point-based tracking method
	- feature point로 움직이는 물체를 tracking

2. kernel-based
	- 모션
3. silhouette-based
	 - 

움직이는 카메라로 작은 드론을 추적하는 데 겪는 어려움
1. 드론은 매우 빠르게 날고, 이미지에서 작다.
2. 컬러피쳐는 적절하지 않다. 작은 드론은 엣지가 없거나 실루엣 정보가 없어서

ROI(region of interest) 관심영역
칼만필터가 주로 사용됨

## 2. 방법론
### 2-1. optical flow in object tracking
- 이미지 밝기 패턴의 명백한 이동. (이미지의 각 )
- 광학 흐름(optical flow)는 거리 벡터를 나타냄 -> 전 프레임과 현재 프레임에서 관심영역의 이동
- 공간적 일관성은 같은 객체에 속하고 같은 움직임에 속하는 것 같다. 공간적 인접 지점에서
- 

## 4. 결론
- 중앙위치에러 발생. 유클리디안 거리의 평균으로 인해 발생
- 