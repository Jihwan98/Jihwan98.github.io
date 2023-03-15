---
title:  "[Deep Learning] keras 콜백 함수 ReduceLROnPlateau"
author: JIHWAN PARK
categories: [AI & 데이터분석, DL]
tag: [Deep Learning, Tensorflow, Keras, Callback]
math: true
date: 2023-03-15 23:41:59 +0900
last_modified_at: 2023-03-15 23:41:59 +0900
---
> keras 콜백 함수인 ReduceLROnPlateau에 대해 알아보자.
{: .prompt-info}

<a href='https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ReduceLROnPlateau' target='_blank'>[Tensorflow 공식 문서]</a>

# ✅ ReduceLROnPlateau
모델의 성능 개선이 없을 경우 Learning Rate를 조절하는 Callback 함수이다.

```python
from tf.keras.callbacks import ReduceLROnPlateau

lr_reduction = ReduceLROnPlateau(monitor='val_loss',
                                 factor=0.5,
                                 patience=5,
                                 min_lr=0.000001)

model.fit(x, y, callbacks=[lr_reduction])
```

파라미터 중 factor는 Learning Rate를 줄일 비율이다. `new_lr = lr * factor`

자세한 파라미터는 공식문서를 참고하면 될 듯하다.