---
title:  "[Deep Learning] YOLO V5를 Transfer Learning 해보자!"
author: JIHWAN PARK
categories: [AI & 데이터분석, DL]
tag: [Deep Learning, YOLO, Object Detection, Trnasfer Learning]
math: true
date: 2023-03-17 19:47:05 +0900
last_modified_at: 2023-03-17 19:47:05 +0900
---
> Object Detection 모델 중 성능이 준수한 YOLO V5를 Transfer Learning 해보자.
{: .prompt-info}

<a href='https://github.com/ultralytics/yolov5' target='_blank'>[ultralytics/yolov5 Github Page]</a>

# ✅ YOLO V5 Transfer Learning
Object Detection 모델 중 성능이 준수한 YOLO V5를 Trnasfer Learning 해보자! 아래 코드는 Colab을 기준으로 작성했다.

## ✔️ ybat을 통해 Annotation 생성하기
먼저 자신이 원하는 데이터를 학습시키기 위해서, 데이터 Annotation 파일이 필요하다.

다양한 방법 중 ybat을 사용해서 생성해보자.

<a href='https://github.com/drainingsun/ybat' target='_blank'>[ybat Github Page]</a> 먼저 링크로 가서 해당 Repository를 다운 받고 ybat.html을 실행 시키면 인터넷 창에 Tool이 열릴 것이다.

![image](https://user-images.githubusercontent.com/76936390/225886620-9886827e-fc4d-475f-996d-04e94d5efe3d.png)

여기서 `파일 선택`을 누른 후 원하는 사진들을 넣는다. 그리고 txt파일로 Class 파일을 생성하는데, 만약 human, dog, cat으로 한다면 아래와 같이 txt파일을 생성하고 `파일 선택`을 눌러 불러오면 된다.

```
human
dog
cat
```

![image](https://user-images.githubusercontent.com/76936390/225887485-e7187ab4-9634-49e5-94c6-73ef25321091.png)

그 다음, 위 사진 처럼 각 클래스를 선택하고 드래그해서 Object를 선택해준다. 

모든 데이터에 대해 진행했다면 `Save YOLO`를 클릭해 Annotation file을 다운로드 받는다. 해당 파일에는 (클래스, x, y, w, h) 정보가 들어있다.

이제 이 데이터들과 Annotation 파일을 아래와 같이 폴더를 구성해주면 데이터 준비가 끝난다.

Person_dog_cat<br>
|--- images<br>
| &nbsp; &nbsp; &nbsp; &nbsp; |--- train<br>
|--- labels<br>
| &nbsp; &nbsp; &nbsp; &nbsp; |--- train<br>

## ✔️ 환경 구성

해당 Github를 Clone 해오자.

```python
!git clone https://github.com/ultralytics/yolov5
```

그리고 버전 업데이트에 따른 경고 문구가 뜬다고 해서(동작하는데는 이상이 없다고는 함) 아래와 같은 코드를 실행해 `requirements.txt`를 수정하면 된다.

```python
## yolov5 폴더 requirements.txt 수정 필요
## setuptools<=64.0.2

temp_str = 'setuptools<=64.0.2\n' 
f = open('/content/yolov5/requirements.txt', 'r') 
f_str = f.readlines() 
f.close() 

f2 = open('/content/yolov5/requirements.txt', 'w') 

for idx, val in enumerate(f_str) : 
    if 'setuptools' in val : 
        idx_v = idx 
        f_str.remove(val) 
        f_str.insert(idx_v, temp_str) 

for val in f_str : 
    f2.write(val) 
    
f2.close()
```

그리고 requirements.txt 를 실행해서 환경을 구성하자.

```python
!cd yolov5; pip install -r requirements.txt
```


## ✔️ Pretrained Weight 다운로드
 
<a href='https://github.com/ultralytics/yolov5' target='_blank'>[ultralytics/yolov5 Github Page]</a> 해당 페이지로 가서 Pretrained Weight 파일을 다운로드 받는다. 해당 페이지에 들어가서 아래로 조금 내리면 Pretrained Weight 파일들이 있고, 파라미터와 속도를 확인해서 원하는 파일을 다운 받으면 된다. 나는 `YOLOv5m.pt` 파일을 받았다. 마찬가지로 wget으로 받아주면 된다.

![image](https://user-images.githubusercontent.com/76936390/225902139-cc174153-2557-4efe-a1f7-b353c29321a9.png)

```python
!mkdir /content/yolov5/pretrained
!wget -O /content/yolov5/pretrained/yolov5m.pt https://github.com/ultralytics/yolov5/releases/download/v7.0/yolov5m.pt
```

## ✔️ yaml 파일 생성
Colab 에서 한다면 데이터를 구글 드라이브에 올리고 구글 드라이브를 마운트 해서 사용하는 것이 편하다. 그래서 아까 만든 데이터 폴더를 구글 드라이브에 올리고 경로를 찾은 다음 yaml 파일을 생성해보자.

```yaml
path: /content/drive/MyDrive/Person_dog_cat/  # dataset root dir
train: images/train  # train images (relative to 'path') 128 images
val: images/train  # val images (relative to 'path') 128 images
test:  # test images (optional)

# Classes
nc: 3  # number of classes
names: ['human', 'dog', 'cat']  # class names
```

아래와 같이 path, train, val, test, class 부분을 각자에 경로에 맞춰 수정해주면 된다. train, val, test는 path의 상대경로이기 때문에 하위 폴더에 폴더명만 제대로 해놓고 설정해주면된다. (나는 일단 validation data를 trian으로 했다)

## ✔️ train.py 실행
이제 학습을 시켜보자. 자세한 명령어 도움말은 `python train.py -h`로 확인할 수 있다. 

- --data : 학습시킬 데이터 경로와 class 정보가 담긴 yaml 파일 경로
- --cfg : 우리가 학습시킬 모델 구조를 담은 yaml 파일 경로. 나는 yolov5m weight를 쓸 거기 때문에 '/content/yolov5/models/yolov5m.yaml' 이 된다.
- --weights : pretrained weights 파일 경로
- --epochs : epoch
- --patience : Early Stopping 설정. 몇 번 이상 성능 개성이 안일어나면 멈출 것인지. 기본적으로 mAP50, mAP50-95를 쓴다(?)
- --project : 학습한 모델 weight가 저장될 폴더
- --name : 학습한 모델 weight가 저장될 하위 폴더(--proejct 로 설정한 경로 밑)

```python
!cd yolov5; python train.py \
    --data '/content/person_dog_cat.yaml' \
    --cfg '/content/yolov5/models/yolov5m.yaml' \
    --weights '/content/yolov5/pretrained/yolov5m.pt' \
    --epochs 1000 \
    --patience 10 \
    --project 'trained' \
    --name 'train_person_dog_cat'
```

## ✔️ detect.py 실행
이제 원하는 사진에다가 학습된 모델로 detect해보자.

```python
!cd yolov5; python detect.py \
    --weights '/content/yolov5/trained/train_person_dog_cat/weights/best.pt' \
    --source '/content/drive/MyDrive/Person_dog_cat/images/train' \
    --project '/content/detected' \
    --name 'images' \
    --line-thickness 2 \
    --conf-thres 0.5 \
    --iou-thres 0.3 \
    --exist-ok  # 설정한 project/name 이 이미 있으면 덮어씀
```

## ✔️ Detected Image 확인해보기

```python
from IPython.display import Image
import os

file_path = '/content/detected/images'
files = os.listdir(file_path)

for f in files:
    display(Image(os.path.join(file_path, f)))
```

colab에서 detected 된 사진들을 zip파일로 묶어서 다운받아보자.

```python
from google.colab import files

!zip -r /content/detected_person_dog_cat.zip /content/detected/images

files.download(filename='/content/detected_person_dog_cat.zip')