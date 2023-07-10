---
title:  "[Deep Learning] Early Stopping 과 Validation Split, Data"
author: JIHWAN PARK
categories: [AI & 데이터분석, DL]
tag: [Deep Learning, Tensorflow, Keras, Callback]
math: true
date: 2023-03-13 16:10:00 +0900
last_modified_at: 2023-03-13 16:10:00 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/b3273300-8735-47f7-9c77-7ba5b411f531"
---
> model 학습 시, Early Stopping과 Validation Split, Validation Data 적용하기.
{: .prompt-info}

## ✅ Early Stopping
- 학습 진행 중에, 설정한 값에 따라 성능이 개선되지 않으면 학습을 중단 시킬 수 있는 옵션
- `from tensorflow.keras.callbacks import EarlyStopping`으로 함수를 불러온다.
- Early Stopping의 주요 파라미터
    - monitor : 관측 대상 (default : val_loss)
    - patience : 성능이 개선되지 않을 때 몇 번 참을래?
    - min_delta : Threshold
    - restore_best_weights : 가장 성능이 좋았던(monitor 기준) epoch의 가중치 적용 (default : False)

```python
from tensorflow.keras.callbacks import EarlyStopping

es = EarlyStopping(monitor='val_loss',            # 관측 대상
                   min_delta=0,                   # 학습 성능 Threshold
                   patience=5,                    # 성능 개선되지 않더라도 몇 번 참을래?
                   verbose=1,                     
                   restore_best_weights=True)     # 가장 성능이 좋았던 epochs의 가중치를 쓸래 (Default=False)

model.fit(train_x, train_y, validation_split=0.2, 
          callbacks=[es], verbose=1, epochs=50)
```

## ✅ Validation Split, Validation Data
- 모델 학습 시, Validation Data를 설정할 수 있는 파라미터이다.
- 만약 `EarlyStopping` 파라미터의 기본 값인 `monitor='val_loss'`로 지정했다면, 필수적으로 설정해야한다.(설정하지 않으면 관측값이 없어지므로)

**✔ Validation Split**

- Validation Split은 설정한 비율만큼 training set에서 자동으로 validation set으로 학습시켜준다.
- `model.fit(validation_split=0.2)` : training set 에서 20%는 validation set으로 사용

**✔ Validation Data**

- Validation Data는 Validation set을 직접 설정해주는 것이다.
- `model.fit(validation_data=(val_x, val_y))` : validation set으로 val_x와 val_y를 사용

즉, Early Stopping 과 Validation Set을 적용한 학습 코드는 다음과 같이 작성할 수 있다.

```python
from tensorflow.keras.callbacks import EarlyStopping

es = EarlyStopping(monitor='val_loss',            # 관측 대상
                   min_delta=0,                   # 학습 성능 Threshold
                   patience=5,                    # 성능 개선되지 않더라도 몇 번 참을래?
                   verbose=1,                     
                   restore_best_weights=True)     # 가장 성능이 좋았던 epochs의 가중치를 쓸래 (Default=False)

# validation split 사용
model.fit(train_x, train_y, validation_split=0.2, 
          callbacks=[es], verbose=1, epochs=50)

# validation data 사용
model.fit(train_x, train_y, validation_data=(val_x, val_y), 
          callbacks=[es], verbose=1, epochs=50)
```



