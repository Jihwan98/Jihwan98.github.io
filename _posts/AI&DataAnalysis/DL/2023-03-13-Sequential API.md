---
title:  "[Deep Learning] Keras Sequential API로 모델 생성"
author: JIHWAN PARK
categories: [AI & 데이터분석, DL]
tag: [Deep Learning, Tensorflow, Keras]
math: true
date: 2023-03-13 16:00:00 +0900
last_modified_at: 2023-03-13 16:00:00 +0900
---
> 선형회귀 & 로지스틱 회귀 & 멀티 클래스 분류를 Keras Sequential API로 구현하기.
{: .prompt-info}

## ✅ Sequential API
- Tensorflow Keras를 활용하여 Sequential API 방식으로 모델을 생성하는 방법을 알아보자.
- 선형회귀, 로지스틱 회귀(이중 분류), 다중 분류에 대한 예시이다. 
- Sequential API로 진행할 때, 전체적인 코드는 비슷하며,
- 다만, activation function과 complie할 때의 loss와 metrics 값 정도가 다르다. 다중분류는 one hot encoding 까지 진행해주어야 함.

```python
import tensorflow as tf
from tensorflow import keras

from tensorflow.keras.utils import to_categorical # One-Hot Encoding

x = ~   # n개의 feature가 있다고 가정
y = ~   # 선형회귀와 이중 분류는 그대로 진행
y = to_categorical(y, m)   # 다중 분류는 One-Hot Encoding 진행, class 개수 m개

# 1. 세션 초기화
keras.backend.clear_session()

# 2. 모델 발판 만들기
model = keras.models.Sequential()

# 3. 모델 쌓기
model.add( keras.layers.Input(shape=(n, )) )
model.add( keras.layers.Dense(64, activation='relu') ) # Hidden Layer
model.add( keras.layers.Dense(32, activation='relu') ) # Hidden Layer

## 목적에 맞게 아래 코드 중 하나 씩 써야함.
model.add( keras.layers.Dense(1) )  # 선형회귀
model.add( keras.layers.Dense(1, activation='sigmoid') )  # 로지스틱 회귀(이중 분류)
model.add( keras.layers.Dense(m, activation='softmax') )  # 다중 분류 class가 m개

# 4. 모델 컴파일
## 목적에 맞게 아래 코드 중 하나 씩 써야함.
model.compile(loss='mse', optimizer='adam') # 선형 회귀
model.compile(loss='binary_crossentropy', metrics=['accuracy'], optimizer='adam')   # 로지스틱 회귀(이중분류)
model.compile(loss='categorical_crossentropy', metrics=['accuracy'], optimizer='adam')   # 다중 분류

# 5. 모델 구조 확인
model.summary()

# 6. 모델 훈련
model.fit(x, y, epochs=10)

# 7. 예측
y_pred = model.predict(x)
```

## ✅ 문제 종류별 activation function, loss, metrics 정리

||선형 회귀|로지스틱 회귀(이중 분류)|다중 분류|
|---|---|---|---|
|**activation**|linear|sigmoid|softmax|
|**loss**|mse|binary_crossentropy|categorical_crossentropy|
|**metrcics**|-|accuracy, ...|accuracy, ...|