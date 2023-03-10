---
title:  "[KT Aivle 3기 AI] 22일차. 딥러닝 (4) Functional API 활용 실습"
author: JIHWAN PARK
categories: [AI & 데이터분석, AIVLE SCHOOL]
tag: [AIVLE SCHOOL, Deep Learning]
math: true
date: 2023-03-03 18:30:00 +0900
last_modified_at: 2023-03-03 18:39:00 +0900
---
> KT Aivle School 3기 AI 22일차 
> - 강사 : 김건영 강사님
> - 주제 : 딥러닝 공부를 위한 기본 토대 쌓기
> - 내용 :
>   - Functional API를 사용한 Connection Layer(Concatenate, Add) 구현
>   - Functional API를 사용한 실습 (Iris, Breast_cancer)
{: .prompt-info}

## 딥러닝 Connection
- 특징들을 종류 별로 따로 연결하여 연결된 정보를 `Concatenate` or `Add`하는 Layer 추가.
- `Add`는 shape이 맞아야 연산 가능
- 코드 예시

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

## 모델 구조 시각화
- keras에는 모델 구조를 이미지로 시각화 해볼 수 있는 Tool이 존재
- `model.summary()`로 간단하게 확인할 수 있지만, 표로 더 직관적으로 확인할 수 있다.
- 코드

```python
from tensorflow.keras.utils import plot_model

plot_model(model, show_shapes=True)
```
![image](https://user-images.githubusercontent.com/76936390/222691703-f1575fd7-8b1e-46d8-a917-398d1bd891c8.png)


## Iris, Breast Cancer, Wine 데이터 실습
- <a href='https://github.com/Jihwan98/aivle_school/blob/main/2023.02.27_%EB%94%A5%EB%9F%AC%EB%8B%9D_%EC%8B%A4%EC%8A%B5%EC%9E%90%EB%A3%8C/2023-03-03.ipynb' target='_blank'>Wine 코드실습</a>
- <a href='https://github.com/Jihwan98/aivle_school/blob/main/2023.02.27_%EB%94%A5%EB%9F%AC%EB%8B%9D_%EC%8B%A4%EC%8A%B5%EC%9E%90%EB%A3%8C/3_1_iris_connection_I.ipynb' target='_blank'>Iris concatenate 코드실습</a>
- <a href='https://github.com/Jihwan98/aivle_school/blob/main/2023.02.27_%EB%94%A5%EB%9F%AC%EB%8B%9D_%EC%8B%A4%EC%8A%B5%EC%9E%90%EB%A3%8C/3_2_iris_connection_II.ipynb' target='_blank'>Iris add 코드실습</a>
- <a href='https://github.com/Jihwan98/aivle_school/blob/main/2023.02.27_%EB%94%A5%EB%9F%AC%EB%8B%9D_%EC%8B%A4%EC%8A%B5%EC%9E%90%EB%A3%8C/3_3_Breast_cancer.ipynb' target='_blank'>Breast Cancer 종합실습</a>
