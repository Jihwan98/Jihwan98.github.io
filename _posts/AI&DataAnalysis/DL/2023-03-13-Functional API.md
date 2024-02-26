---
title:  "[Deep Learning] Keras Functional API로 모델 생성"
author: JIHWAN PARK
categories: [AI & 데이터분석, DL]
tag: [Deep Learning, Tensorflow, Keras]
math: true
date: 2023-03-13 16:30:00 +0900
last_modified_at: 2023-03-13 16:30:00 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/b3273300-8735-47f7-9c77-7ba5b411f531"
---
> Keras Functional API로 모델 생성하기.
{: .prompt-info}

## ✅ Functional API
- Tensorflow Keras를 활용하여 Functional API 방식으로 모델을 생성하는 방법을 알아보자.
- Sequential API에서는 Sequential Model을 먼저 선언한 후 Layer를 쌓았다면,
- Functional API에서는 모델을 먼저 엮은 후, Input과 Output Layer를 지정해주어서 모델을 생성해준다. 
- 특징은 Layer를 엮을 때, 이전 Layer를 꼭 명시 해주어야 한다.

```python
import tensorflow as tf
from tensorflow import keras

x = ~   # n개의 feature가 있다고 가정
y = ~   # 선형회귀와 이중 분류는 그대로 진행

# 1. 세션 초기화
keras.backend.clear_session()

# 2. 모델 엮기
input_layer = keras.layers.Input(shape=(n, ))
hidden_layer = keras.layers.Dense(64, activation='relu')(input_layer)
hidden_layer = keras.layers.Dense(64, activation='relu')(hidden_layer)
output_layer = keras.layers.Dense(1)

# 3. 모델 선언 (Input, Output 지정)
model = keras.models.Model(input_layer, output_layer)

# 4. 모델 컴파일
model.compile(loss='mse', optimizer='adam') # 선형 회귀

# 5. 모델 구조 확인
model.summary()
```