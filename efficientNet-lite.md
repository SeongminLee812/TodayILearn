# efficientNet-lite0 (성민)

[tensorflow hub](https://tfhub.dev/tensorflow/efficientnet/lite0/feature-vector/2)

# 코드

- 다른 모델을 사용하는 경우 2번, 3번 수정 필요

### 1. 모듈 임포트

```python
import os
import pandas as pd
import tensorflow_hub as hub
import tensorflow as tf
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
import numpy as np
```

### 2. 모델 생성

```python
model_url = "https://tfhub.dev/tensorflow/efficientnet/lite0/feature-vector/2"

IMAGE_SHAPE = (224, 224)

layer = hub.KerasLayer(model_url, input_shape=IMAGE_SHAPE+(3,))
model = tf.keras.Sequential([layer])
```

- pre-train 모델을 url로 가져오기
- 이미 학습된 모델의 뉴런층을 그대로 가져와 모델 생성

### 3. 특징 벡터를 추출하기

```python
def extract(file):
    file = Image.open(file).convert('L').resize(IMAGE_SHAPE)
		

    file = np.stack((file,)*3, axis=-1)

    file = np.array(file)/255.0 # 정규화

    embedding = model.predict(file[np.newaxis, ...])
    vgg16_feature_np = np.array(embedding)
    flattended_feature = vgg16_feature_np.flatten()

    return flattended_feature
```

- 인자로 file을 받아 flattened된 벡터를 돌려주는 함수
- np.newaxis : numpy 배열의 차원을 늘려주는 함수 → input shape을 맞춰주기 위함
- 입력값 : 이미지파일 / 출력값 : 벡터

```python
path = "[이미지가 있는 경로]"
file_list = os.listdir(path)
file_list_img = [file for file in file_list if file.endswith(".png") or file.endswith(".jpeg") or file.endswith(".jpg")]
```

- 이미지가 있는 경로(폴더)만 잡으면 폴더 안 이미지 파일을 전부 잡아 리스트에 파일명들을 담아줌

### 4. 파일명과 벡터 값 담기

```python
tmp_df = pd.DataFrame()
for i, img in enumerate(file_list_img): # 위에서 선언된 파일명 담은 리스트
    output = extract(path+'/'+img) # 특징벡터추출함수 사용
    tmp_df = tmp_df.append({'filename':img, 'output':output}, ignore_index=True)
```

- 데이터 프레임을 만들어, 아래 사진과 같이 0열에는 파일명, 1열에는 추출된 벡터값 저장
- tmp_df
    
    ![Untitled](efficientNet-lite0%20(%E1%84%89%E1%85%A5%E1%86%BC%E1%84%86%E1%85%B5%E1%86%AB)%20580b8e4958ca4f2da6e0cf242b191f55/Untitled.png)
    

### 5. 유사도 측정(코사인 유사도 사용)

```python
from scipy.spatial import distance
metric = 'cosine'
# metric으로 'euclidean', 'cosine', 'dot' 등등 사용 가능

cos_sim_array = np.zeros((len(tmp_df),len(tmp_df))) # 빈 넘파이 배열 생성
for i in range(0, len(tmp_df)):
    for j in range(0, len(tmp_df)):
        cos_sim_array[i][j] = distance.cdist([tmp_df.iloc[i, 1]] , [tmp_df.iloc[j, 1]], metric)[0]
```

```python
file_list = tmp_df['filename'].tolist()
cos_sim_df = pd.DataFrame(cos_sim_array, index=file_list, columns=file_list)
```

- metric에 어떤 값을 담느냐에 따라 유사도가 달라짐. [참고](https://docs.scipy.org/doc/scipy/reference/spatial.distance.html?highlight=distance)
- scipy의 거리 측정 모듈로 거리 측정하여 유사도 값들을 빈 넘파이 배열(정확히는 값들이 모두 0으로 채워진 넘파이 배열)에 담고 반복문으로 해당하는 파일들의 유사도들을 cos_sim_df 데이터 프레임에 담음

![Untitled](efficientNet-lite0%20(%E1%84%89%E1%85%A5%E1%86%BC%E1%84%86%E1%85%B5%E1%86%AB)%20580b8e4958ca4f2da6e0cf242b191f55/Untitled%201.png)

- 파일 개수 만큼 163row x 163 columns 생성 (히트맵처럼 그려짐)
- 나중에 파일이 커지면 이 데이터 프레임이 매우 커질 것이므로 속도 측면에서 다른 방법을 강구할 필요

### 6. 결과 보기

```python
def show_sim(filename):
    sim_series = cos_sim_df[filename].sort_values(ascending=True)[:10]
    f, ax = plt.subplots(2, 5, figsize=(40,20))
    for i in range(len(sim_series)): 
        tmp_img = Image.open(path+'/'+sim_series.index[i]).convert('RGB') # 이미지를 보기 위해 임시 변수 저장
        sim = f'cos : {sim_series[i]:.3f}' # 유사도 값 담아두기
        ax[i//5][i%5].imshow(tmp_img, aspect='auto')# 5열짜리 표를 만드는 것이므로 단순히 5로 나눈 나머지와 몫을 사용한 것임
        if i == 0: 
            title = f'original \n{sim_series.index[i]}' # 자기 자신은 제목을 original로 지정
        else: title = f'similarity no.{i} \n{sim_series.index[i]}'
        ax[i//5][i%5].set_title(title, pad=20, size=25) 
        ax[i//5][i%5].annotate(sim, (0,10), fontsize=18, color='red') # 유사도를 그림 좌측 상단에 표시
        ax[i//5][i%5].set_xticks([]); ax[i//5][i%5].set_yticks([])
    
    # plt로 출력된 이미지를 사진으로 저장하기
    # fig1 = plt.gcf() # plt.show 이후에 save를 하면 안되니까 미리 fig 저장
    plt.show()
    # fig1.savefig(f'test_{filename}', format='jpeg')
```

```python
show_sim('F2022061000002781-1.jpg')
```

# 모델 실행 결과

![test_F2022061000002781-1.jpg](efficientNet-lite0%20(%E1%84%89%E1%85%A5%E1%86%BC%E1%84%86%E1%85%B5%E1%86%AB)%20580b8e4958ca4f2da6e0cf242b191f55/test_F2022061000002781-1.jpg)

- cos 뒤 숫자가 작을 수록 유사함.
- 가장 유사한 9가지를 출력
- 눈으로 보기에는 왜 유사한 지 확인이 어렵다. → 유사도 측정을 위한 score 등 새로운 지표 확인할 필요 있음
- 가능하다면 XAI인 Class Activation Map을 이용해 확인하면 좋을 듯 하다.