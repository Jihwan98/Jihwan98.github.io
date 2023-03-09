---
title:  "Class Imbalance"
author: JIHWAN PARK
categories: [AI & 데이터분석, ML]
tag: [Python, Machine Learning]
math: true
date: 2023-03-09 18:30:00 +0900
last_modified_at: 2023-03-09 18:30:00 +0900
---
> Class Imbalance에 대해 알아보고 해결 방법을 알아보자.
{: .prompt-info}

# ✅ Class Imbalance

Class Imbalance는 우리가 예측해야하는 y값이 균등하게 존재하지 않고 한 쪽에 더 많은 데이터가 있는 경우이다. 현업에서는 대부분이 Imbalance 하다고 한다. 

이때의 문제점은 모델은 전체의 오차가 가장 적어지도록 학습하기 때문에, 개수가 가장 적은 클래스의 recall이 낮아지는 현상이 발생한다.

이를 처리할 수 있는 방법 중 3가지를 확인해보자.

## ✔ Down Sampling
Down Sampling은 개수가 많은 데이터를 개수가 적은 데이터에 맞추는 것이다. ex) 20개, 80개 -> 20개, 20개

## ✔ Up Sampling
Up Sampling은 개수가 적은 데이터를 개수가 많은 데이터에 맞추는 것이다. 이때 개수가 적은 데이터에서 복원추출로 개수를 맞춘다. ex) 20개, 80개 -> 80개, 80개

## ✔ SMOTE(Synthetic Minority Oversampling Tecnique)
SMOTE는 개수가 적은 데이터를 개수가 많은 데이터에 맞추는데, 보간법으로 데이터를 생성하는 방법이다. ex) 20개, 80개 -> 80개, 80개

**SMOTE 코드**

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE()
s, y = smote.fit_resample(x, y)
```

## ✔ 세가지 비교

||20개|80개|
|---|---|---|
|Down Sampling|20개|20개|
|Up Sampling|80개(복원추출)|80개|
|SMOTE|80개(생성, 보간법)|80개|
