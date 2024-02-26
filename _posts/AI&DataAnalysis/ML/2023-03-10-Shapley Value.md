---
title:  "Shapley Value와 SHAP"
author: JIHWAN PARK
categories: [AI & 데이터분석, ML]
tag: [Python, Scikit-learn, Machine Learning, Data Analysis]
math: true
date: 2023-03-10 19:00:00 +0900
last_modified_at: 2023-03-10 19:00:00 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/d9c3eabe-9815-4280-b058-b7eb2be7b177"
---
> Shapley Value와 SHAP에 대해서 간략한 설명과 실습 코드
{: .prompt-info}

# Shapley Value와 SHAP(SHapley Additive exPlanations)
Shapley Value 와 SHAP는 이 데이터(분석단위)는 왜 그러한 결과로 예측 되었을까? 를 설명할 수 있는 방법 중 하나이다. 즉, 모델이 하나의 분석단위를 그렇게 예측한 이유에 대해 설명할 수 있다. 이는 실제 업무에서 사용자에게 설명할 수 있는 좋은 방법이라고 한다.

## ✅ Shapley Value
Shapley Value는 Feature의 가중 평균 기여도라고 할 수 있다. 

예를 들어, A B C 라는 특성이 있을 때 A에 대한 Shapley Value를 계산하는 방법은 다음과 같다. 

1. A를 고정 시켜 놓고, 나머지 변수들로 조합을 구성한다.<br> 즉, (A), (A, B), (A, C), (A, B, C) 총 4(=$2^2$)개 가 있다. 
2. 이 각각의 조합에서 A를 포함했을 때의 예측 값과 A를 제외했을 때의 예측 값의 차이를 구한다.<br> ex) (A, B, C) 에서의 예측값 - (B, C) 에서의 예측 값
3. 여기에 **가중치**를 곱해서, 각 조합의 기여도를 합친다.

이를 식으로 표현하면 아래와 같다.

$$
\large \phi_i(f, x) = \sum_{z^\prime \subseteq x^\prime}{\frac{\vert z^\prime \vert (M - \vert z^\prime \vert - 1)!}{M!}} \cdot [ f_x(z^\prime ) - f_x ( z^\prime | i) ]
$$

식을 뜯어보지말고, 대략적인 느낌으로만 이해해보자.

- $\sum_{z^\prime \subseteq x^\prime}$ : 조합 하나씩 더하기

- ${\frac{\vert z^\prime \vert (M - \vert z^\prime \vert - 1)!}{M!}}$ : 가중치

- $[f_x(z^\prime ) - f_x (z^\prime \vert i)]$ : 기여도

즉, 조합 별로 가중치 x 기여도의 값을 더한 것이 Shapley Value이다.

여기서 중요한 것은 <mark>예측 값에 대한 설명이라는 점이다. 실제 값과는 전혀 상관이 없다!</mark>


## ✅ SHAP(SHapely Additive exPlanation) 코드 실습
<a href='https://github.com/Jihwan98/aivle_school/blob/main/2023.03.09_AI%EB%AA%A8%EB%8D%B8%20%ED%95%B4%EC%84%9D%ED%8F%89%EA%B0%80_%EC%8B%A4%EC%8A%B5%EC%9E%90%EB%A3%8C/chapter%204-1.%20%EB%AA%A8%EB%8D%B8%EC%97%90%20%EB%8C%80%ED%95%9C%20%EC%84%A4%EB%AA%852_shap.ipynb' target='_blank'>[SHAP 관련 코드]</a>

파이썬에서 Shapley Value를 구하고, SHAP 분석을 시각화해보자. SHAP Explainer는 종류가 다양하게 있으며, 각 모델에 맞게 사용하면 된다.<br><a href='https://shap-lrjball.readthedocs.io/en/latest/api.html#core-explainers' target='_blank'>[SHAP Explainers API Reference]</a>


1. 먼저 shap 라이브러리를 받자. `pip install shap` or `conda install shap`. Colab에서 한다면 `!pip install shap`을 실행하자.
2. 모델을 학습시키고, SHAP 값으로 모델의 예측을 설명하면 된다.

### ✔️ Shapley Value 구하기


```python
# 학습
rf_model = RandomForestRegressor()
rf_model.fit(x_train, y_train)

# 해석
explainer = shap.TreeExplainer(rf_model)
shap_values = explainer.shap_values(x_train)
```

- x_train과 shap_values의 크기는 동일하다.
- shap_values가 각 feature에 대한 기여도를 담고 있기 때문이다.
- Shapley Value의 행 별의 합 = $\hat{y}$ - 평균


```python
# 동일한 크기
x_train.shape, shap_values.shape
```




    ((404, 12), (404, 12))




```python
pd.DataFrame(shap_values, columns=list(x))[:1]
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
      <th>crim</th>
      <th>zn</th>
      <th>indus</th>
      <th>chas</th>
      <th>nox</th>
      <th>rm</th>
      <th>age</th>
      <th>dis</th>
      <th>rad</th>
      <th>tax</th>
      <th>ptratio</th>
      <th>lstat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.401934</td>
      <td>-0.006239</td>
      <td>-0.092199</td>
      <td>-0.014515</td>
      <td>0.241373</td>
      <td>-2.366129</td>
      <td>0.085752</td>
      <td>-0.365353</td>
      <td>-0.009492</td>
      <td>0.343864</td>
      <td>0.25944</td>
      <td>-0.504673</td>
    </tr>
  </tbody>
</table>
</div>




```python
x_train.iloc[:1, :]
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
      <th>crim</th>
      <th>zn</th>
      <th>indus</th>
      <th>chas</th>
      <th>nox</th>
      <th>rm</th>
      <th>age</th>
      <th>dis</th>
      <th>rad</th>
      <th>tax</th>
      <th>ptratio</th>
      <th>lstat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>50</th>
      <td>0.08873</td>
      <td>21.0</td>
      <td>5.64</td>
      <td>0</td>
      <td>0.439</td>
      <td>5.963</td>
      <td>45.7</td>
      <td>6.8147</td>
      <td>4</td>
      <td>243</td>
      <td>16.8</td>
      <td>13.45</td>
    </tr>
  </tbody>
</table>
</div>



- 예측 값


```python
y_pred = rf_model.predict(x_train.iloc[:1, :])
y_pred[0]
```




    19.83200000000003



- train의 예측 값의 전체 평균


```python
rf_model.predict(x_train).mean()
```




    21.84210891089109



- 예측 값과 예측 평균 간의 차이


```python
y_pred[0] - rf_model.predict(x_train).mean()
```




    -2.0101089108910593



- Shapley Value의 합


```python
shap_values[:1, :].sum()
```




    -2.026235148514834



- 하나의 분석단위에 대한 Shapley Value 의 합 = 예측 값 - 예측 평균
    - 랜덤 포레스트의 경우, 알고리즘의 특성상 약간의 차이가 발생 될 수 있다고 한다.

### ✔️ `shap.force_plot`
- 화살표의 길이 : Shapley Value, 기여도
- base value : 예측 값의 평균


```python
shap.initjs() # javascript 시각화 라이브러리 --> colab에서는 모든 셀에 포함시켜야 함

idx = 0
# force_plot(전체평균, shapley_values, input)
shap.force_plot(explainer.expected_value, shap_values[idx, :], x_train.iloc[idx,:])
```

![force_plot](https://user-images.githubusercontent.com/76936390/224327844-2c84c036-1e2b-4e51-9808-ef2183c50840.png)


### ✔️ `shap.summary_plot`
- 각 변수별 shapley value 분포를 한눈에 보여준다.
- 색깔은 해당 변수의 값의 크기를 나타낸다.


```python
shap.summary_plot(shap_values, x_train)
```


    
![output_21_0](https://user-images.githubusercontent.com/76936390/224327818-455e388d-c86c-45a8-a050-3b6696c32f5c.png)
    


### ✔️ `shap.force_plot` 특정 관점으로 보기


```python
shap.initjs()
shap.force_plot(explainer.expected_value, shap_values, x_train)
```

![force_plot2](https://user-images.githubusercontent.com/76936390/224327854-44df83a3-628b-48c7-bd4d-5247d1516d6d.png)


### ✔️ `shap.dependence_plot`
- 특정 변수 값과 변수의 shap value 간의 관계 확인


```python
shap.dependence_plot('lstat', shap_values, x_train)
```


    
![output_25_0](https://user-images.githubusercontent.com/76936390/224327822-9699e1ad-0de7-44ce-ac4f-6d6135b7532e.png)
    



```python
shap.dependence_plot('lstat', shap_values, x_train, interaction_index = 'nox')
```


    
![output_26_0](https://user-images.githubusercontent.com/76936390/224327825-3a2ec20b-d429-4272-b062-e25724223e67.png)
    

