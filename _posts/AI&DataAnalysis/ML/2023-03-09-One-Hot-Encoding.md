---
title:  "가변수화, One-Hot Encoding(원핫인코딩) 방법"
author: JIHWAN PARK
categories: [AI & 데이터분석, ML]
tag: [Python, Pandas, Tensorflow, Data Preprocessing, Machine Learning]
math: true
date: 2023-03-09 17:00:00 +0900
last_modified_at: 2023-04-11 12:25:04 +0900
---
> 데이터 전처리시 가변수화 하는 방법에 대해서 알아보자.
{: .prompt-info}

## ✅ scikit-learn
`sklearn.preprocessing.LabelEncoder()` 함수를 통해 범주형 변수를 숫자로 변환할 수 있다.

scikit-learn 공식문서 : [scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)

```python
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
temp1 = ['A', 'B', 'C', 'D', 'A', 'C']
temp2 = ['A', 'B', 'D', 'B', 'A', 'C']
output1 = le.fit_transform(temp1) # fit_transform을 해도 되고, fit 하고 transform을 해도 됨.
# 즉, 아래와 같이 가능
# le.fit(temp1)
# output1 = le.transform(temp1)
output2 = le.transform(temp2)
output1, output2
```

```
(array([0, 1, 2, 3, 0, 2]), array([0, 1, 3, 1, 0, 2]))
```

```python
le.classes_
```

```
array(['A', 'B', 'C', 'D'], dtype='<U1')
```

```python
le.inverse_transform(output1)
```

```
array(['A', 'B', 'C', 'D', 'A', 'C'], dtype='<U1')
```


## ✅ Pandas
가변수화, One-Hot Encoding은 `Pandas`의 `get_dummies` 함수를 쓰면 간단하게 구현 가능하다.


```python
titanic.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
</div>



`get_dummies` 함수의 `columns=`에 가변수화를 진행할 컬럼을 넣어주면 해당 컬럼에 대해서 가변수화를 수행한다. `drop_first = True`로 하면, 다중 공선성을 방지하기 위해 첫 번째 컬럼을 제외하고 컬럼을 생성해준다. 예를 들어, 아래 예시에서 `Pclass_1` 컬럼은 생성되지 않은 것을 확인할 수 있다.


```python
# 가변수 대상 변수 식별
dumm_cols = ['Pclass', 'Sex', 'Embarked']
num_cols = [x for x in list(titanic) if x not in dumm_cols]

# 가변수화
titanic = pd.get_dummies(titanic, columns=dumm_cols, drop_first=True)

# 확인
titanic.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Survived</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Pclass_2</th>
      <th>Pclass_3</th>
      <th>Sex_male</th>
      <th>Embarked_Q</th>
      <th>Embarked_S</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## ✅ Tensorflow
`tensorflow.keras.utils`에 있는 `to_categorical` 함수를 사용하면 One-Hot Encoding을 간단하게 구현할 수 있다. `to_categorical(y = y, num_classes = n)` 형태로 사용하는데, `num_classes` 에는 class의 개수가 들어가지만 값을 넣지 않는다면 알아서 개수에 맞춰 One-Hot Encoding 해준다. 


```python
y = np.array([0, 0, 1, 1, 2, 2])
y
```




    array([0, 0, 1, 1, 2, 2])




```python
from tensorflow.keras.utils import to_categorical
y_one = to_categorical(y)
y_one
```




    array([[1., 0., 0.],
           [1., 0., 0.],
           [0., 1., 0.],
           [0., 1., 0.],
           [0., 0., 1.],
           [0., 0., 1.]], dtype=float32)
