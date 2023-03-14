---
title:  "[Numpy] 행렬 축 바꾸기. np.transpose()"
author: JIHWAN PARK
categories: [AI & 데이터분석, ML]
tag: [Python, Numpy, Machine Learning]
math: true
date: 2023-03-14 20:21:51 +0900
last_modified_at: 2023-03-14 20:21:51 +0900
---
> Numpy에서 
{: .prompt-info}


## ✅ 2차원 전치행렬

2차원 numpy array의 전치행렬은 `.T` attribute와 `np.transpose(a)` method로 얻을 수 있다.

```python
import numpy as np

a = np.arange(10).reshape(2, 5)
b = a.T
c = np.transpose(a)

print("a.shape :", a.shape, end='\n\n')
print(a)

print('-'*30)

print("b.shape :", b.shape, end='\n\n')
print(b)

print('-'*30)

print("c.shape :", c.shape, end='\n\n')
print(c)
```

```
a.shape : (2, 5)

[[0 1 2 3 4]
 [5 6 7 8 9]]
------------------------------
b.shape : (5, 2)

[[0 5]
 [1 6]
 [2 7]
 [3 8]
 [4 9]]
------------------------------
c.shape : (5, 2)

[[0 5]
 [1 6]
 [2 7]
 [3 8]
 [4 9]]
```

## ✅ 3차원 이상 행렬 축 바꾸기

3차원 이상의 numpy array에도 `.T` attribute가 존재하지만, 자유도가 높은(원하는 대로 축을 바꿀 수 있는) `np.transpose()`를 이용하면 된다.

특히 딥러닝의 경우 input 데이터가 (데이터 개수, 세로, 가로, depth) 형태로 들어가게 되는데, 중요한 점은 데이터의 개수가 제일 앞에 위치한다는 것이다. 하지만 종종 데이터를 보면 데이터의 개수가 제일 뒤에 있는 경우가 있는데, 이런 경우에 `np.transpose()`로 데이터의 shape을 맞춰주면 된다.

```python
import numpy as np

v = 32 * 32 * 3 * 100
a = np.arange(v).reshape(32, 32, 3, 100)
b = a.T
c = np.transpose(a, (3, 0, 1, 2))

print("a.shape :", a.shape)
print('-'*30)

print("b.shape :", b.shape)
print('-'*30)

print("c.shape :", c.shape)
```

```
a.shape : (32, 32, 3, 100)
------------------------------
b.shape : (100, 3, 32, 32)
------------------------------
c.shape : (100, 32, 32, 3)
```