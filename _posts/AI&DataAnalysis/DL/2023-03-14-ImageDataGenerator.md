---
title: '[Deep Learning] ImageDataGenerator 사용법'
author: JIHWAN PARK
categories:
  - AI & 데이터분석
  - DL
tag:
  - Deep Learning
  - Tensorflow
  - Keras
math: true
date: '2023-03-14 18:54:18 +0900'
last_modified_at: '2023-03-14 18:54:18 +0900'
published: true
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/b3273300-8735-47f7-9c77-7ba5b411f531"
---
> Data Augmentation을 하기 위한 keras의 ImageDataGenerator 사용법.
{: .prompt-info}

# ✅ Data Augmentation
실제 현실에서는 충분한 Data가 존재하지 않으므로(충분한 데이터를 수집하기가 어려움), 부족한 데이터를 채워주기 위해 기존의 데이터를 변형 시켜 데이터를 늘리는 방법이 Data Augmentaion이다.

# ✅ Keras - ImageDataGenerator
<a href='https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator' target='_blank'>[Tensorflow 공식 문서]</a>

Keras의 ImageDataGenerator를 이용하면, 쉽게 이미지 데이터를 Augmentation 할 수 있다.

Tensorflow 공식 문서를 보면 `tf.keras.preprocessing.image.ImageDataGenerator`는 곧 없어진다고 하는데, 일단은 아직까지 많이 사용하고 있으므로 해당 함수를 사용해보자.

**Augmentaion의 결과 예시**

![dogs](https://user-images.githubusercontent.com/76936390/224969232-1601383d-4969-4b81-993a-e1ccd10114ab.png)

## ✔️ 학습 코드에 적용해보기
```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=30,
    zoom_range=0.2,
    width_shift_range=0.1,
    height_shift_range=0.1,
    horizontal_flip=True,
    vertical_flip=True,
    shear_range=0.2
)

train_gen = datagen.flow(x_train, y_train, batch_size=128)

history = model.fit(train_gen, epochs=100, validation_data=(x_val, y_val), callbacks=[es])
```

## ✔️ ImageDataGenerator 간단한 설명
ImageDataGenerator에는 다양한 Augmentation 조건들의 범위를 설정할 수 있다. 파라미터 이름이 직관적으로 잘 되어있어서 파라미터 이름으로 해당 설정 값을 이해하면 되고, 구체적인 설명은 공식 문서에서 확인하면 된다. 그 중에서 `rescale` 파라미터는 전처리를 따로 안해줘도 되서 유용한 파라미터이다.

`ImageDataGenerator`를 사용해 ImageDataGenerator 객체를 생성한 후, `flow` 메소드에 'x, y, batch_size, shuffle 유무'를 넣어준다. 이 flow 메소드로 생성한 객체는 Numpy Array Iterator 객체로 반복문이나 next() 함수를 이용해서 iterator 안의 데이터를 호출할 수 있다.

이렇게 진행하면, Augment 한 데이터를 메모리에 쌓아두지 않고 쓰고 버리기 때문에 Out Of Memory 문제가 발생하지 않는 장점이 있다.

만약 Augment 된 데이터를 저장하고 싶다면, flow 객체에서 저장할 위치를 담는 파라미터에 값을 넣어주면 된다.

### ✔ 데이터 저장 코드 예시

```python
train_gen = datagen.flow(
    x_train, 
    y_train, 
    batch_size=128,
    save_to_dir='aug_data',
    save_prefix='data',
    save_format='jpg'
)
```

`aug_data` 폴더 밑에 `data_00.jpg` 형식으로 저장된다.


## ✔️ flow_from_directory
ImageDataGenerator 객체에서 `flow` 메소드 말고, `flow_from_directory`와 `flow_from_dataframe`이 있다. 그 중 `flow_from_directory`는 앞으로 많이 쓰일 것 같아 작성해본다.

`flow_from_directory`는 directory에서 데이터를 가져오면서 각 폴더명으로 Label을 설정하여 Augmentation을 해주는 기능이다. 만약 폴더명으로 Label을 하기 싫다면, Label 설정을 할 수 있는 파라미터가 있으니(`classes` 파라미터) 공식 문서를 참고하여 수정하면 된다.

### ✔ flow_from_directory 학습 코드 예시
```python
from keras.preprocessing.image import ImageDataGenerator

train_datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=30,
    zoom_range=0.2,
    width_shift_range=0.1,
    height_shift_range=0.1,
    horizontal_flip=True,
    vertical_flip=True,
    shear_range=0.2
)
validation_datagen = ImageDataGenerator(rescale = 1./255)

train_generator = train_datagen.flow_from_directory(
    train_dir, 
    target_size=(150, 150), 
    batch_size=128,
    class_mode='binary'
)

validation_generator = validation_datagen.flow_from_directory(
    validation_dir,
    target_size=(150, 150),
    batch_size=128,
    class_mode='binary',
    shuffle=False
)

history = model.fit(train_generator, epochs=100, validation_data=validation_generator, callbacks=[es])
```
