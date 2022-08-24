-   **Image Similarity (레퍼런스 참고)**
 ![[Pasted image 20220824222713.png]]

    -   [레퍼런스](https://towardsdatascience.com/image-similarity-with-deep-learning-c17d83068f59)
        
    -   ****feature vector****
        
    -   이미지의 출력 값을 cat, dog 등으로 label하는 것이 아닌 vector 형태로 저장해 놓음
        
    -   0과 1사이의 스칼라 값들로 구성된 벡터
        
    -   XAI(eXplainable AI) 를 접목가능
        
    
    1.  **벡터값 얻기(Embedding)** [깃허브](https://gist.github.com/SaschaHeyer/223d8b0b04395462f68f05519f516bcf#file-model-py)
        -   [텐서플로우 허브](https://tfhub.dev/s?module-type=image-feature-vector)를 이용해 기 학습된 모델 사용가능
            
        -   fine-tuning 없이 우선 시도해 보는 것을 추천
            
        -   특정 사이즈의 패턴을 찾는 것을 학습하므로, 모델이 추천하는 이미지 사이즈를 input하면 됨
            
        -   모델 가져오기
            
            ```python
            import tensorflow as tf
            import tensorflow_hub as hub
            
            # tensorflow hub에 공개된 이미 학습된 모델
            model_url = "<https://tfhub.dev/tensorflow/efficientnet/lite0/feature-vector/2>"
            
            IMAGE_SHAPE = (224, 224)
            
            layer = hub.KerasLayer(model_url, input_shape=IMAGE_SHAPE+(3,))
            model = tf.keras.Sequential([layer])
            ```
            
        -   **Embedding(특정 차원의 벡터로 바꿔주는 과정)**
            
            1.  리사이징
            2.  각 픽셀의 색 표현 바꿔주기
            3.  0~1 사이의 값으로 정규화
            
            ```python
            file = Image.open(file).convert('L').resize(IMAGE_SHAPE)  #1
            file = np.stack((file,)*3, axis=-1)                       #2
            file = np.array(file)/255.0                               #3
            
            embedding = model.predict(file[np.newaxis, ...])
            embedding_np = np.array(embedding)
            flattended_feature = embedding_np.flatten()
            
            print(flattended_feature)
            ```
            
            -   vector length는 모델에 따라 다르다.(이번 예제에서는 1280개의 스칼라)
            
    
    **기타**
    
    labeling 필요 시 software 이용하기 ML label software 