---
title:  "[Deep Learning] Keras Locally Connection(concatenate, add) 구현"
author: JIHWAN PARK
categories: [AI & 데이터분석, DL]
tag: [Deep Learning, Tensorflow, Keras]
math: true
date: 2023-03-13 16:40:00 +0900
last_modified_at: 2023-03-13 16:40:00 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/b3273300-8735-47f7-9c77-7ba5b411f531"
---
> Keras Functional API를 사용한 Locally Connection 구현하기.
{: .prompt-info}

## ✅ Locally Connection
- 딥러닝 Connection에는 `Concatenate` 와 `Add` Layer가 있다.
- 특징들을 종류별로 따로 연결하여 학습한다.
- `Concatenate`는 각각의 feature를 살려 합치는 형태
    - ex) (None, 2) 와 (None, 4)를 concatenate 하면 (None, 6)이 된다.
- `Add`는 단순히 각각 element wise하게 더하기 때문에, Shape이 같아야 한다.
    - ex) (None, 2) 와 (None, 2)를 add 하면 (None, 2)이 된다.

## ✅ 코드 예시

```python
input_layer1 = keras.layers.Input(shape=(2, )) # ex) 특징을 구분하여 따로 넣어줌
input_layer2 = keras.layers.Input(shape=(2, )) # ex) 특징을 구분하여 따로 넣어줌

input_layer3 = keras.layers.Input(shape=(4, )) # ex) 전체 특징

hl_1 = keras.layers.Dense(2, activation='relu')(input_layer1)
hl_2 = keras.layers.Dense(2, activation='relu')(input_layer2)

add_layer = keras.layers.Add()([hl_1, hl_2])
concat_layer = keras.layers.Concatenate()([add_layer, input_layer3])

output_layer = keras.layers.Dense(1, activation='sigmoid')(concat_layer)

model = keras.models.Model([input_layer1, input_layer2, input_layer3], output_layer)
```

위 코드의 모델 구조도

![image](https://user-images.githubusercontent.com/76936390/222691703-f1575fd7-8b1e-46d8-a917-398d1bd891c8.png)
