---
title:  "Feature Importance"
author: JIHWAN PARK
categories: [AI & 데이터분석, ML]
tag: [Python, Scikit-learn, Machine Learning]
math: true
date: 2023-03-09 19:00:00 +0900
last_modified_at: 2023-03-09 19:00:00 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/d9c3eabe-9815-4280-b058-b7eb2be7b177"
---
> 모델의 Feature Importance를 확인하는 방법을 알아보자.
{: .prompt-info}

# ✅ Feature Importance
변수 중요도 : 모델 전체에서 어떤 feature가 중요할까? feature와 예측 결과 간의 관계이며, 당연히 성능이 좋은 모델이 아니라면 이 작업은 무의미하다.

## ✔️ Tree Based Model
Tree 기반 모델은 Feature Importance를 제공한다. Decision Tree, Random Forest, XGB ... 등등.

**✔ Decision Tree**

Decision Tree에서는 Mean Decrease Impurity(MDI)로 표현한다. Tree 전체에 대해서, feature 별로 Information Gain의 (가중) 평균을 계산한다. (Information Gain : 지니 불순도가 감소하는 정도.)

**✔ Random Forest**

Random Forest에서는 각 트리 모델의 변수 중요도의 평균이다.

**✔ XGB**

XGBoost에서는 변수 중요도를 계산하는 3가지 방법이 있다.
- weight
    - 모델 전체에서 해당 feature가 split 할 때 사용된 횟수의 합
    - `plot_importance`에서의 기본값
- gain
    - feature 별 평균 information gain
    - `model.feature_importances_`의 기본값
    - `total_gain` : feature 별 information gain의 총 합
- cover
    - feature가 split 할 때 샘플 수의 평균
    - `total_cover` : 샘플수의 총 합

### ✔ **feature importance plot 함수**


```python
# 변수 중요도 plot
def plot_feature_importance(importance, names, topn = 'all'):
    feature_importance = np.array(importance)
    feature_names = np.array(names)

    data={'feature_names':feature_names,'feature_importance':feature_importance}
    fi_temp = pd.DataFrame(data)

    fi_temp.sort_values(by=['feature_importance'], ascending=False,inplace=True)
    fi_temp.reset_index(drop=True, inplace = True)

    if topn == 'all' :
        fi_df = fi_temp.copy()
    else :
        fi_df = fi_temp.iloc[:topn]

    plt.figure(figsize=(10,8))
    sns.barplot(x='feature_importance', y='feature_names', data = fi_df)

    plt.xlabel('importance')
    plt.ylabel('feature names')
    plt.grid()

    return fi_df
```

### ✔ **Decision Tree**


```python
from sklearn.tree import DecisionTreeRegressor, plot_tree
model = DecisionTreeRegressor()
model.fit(x_train, y_train)

# 모델 시각화
plt.figure(figsize = (20, 8))
plot_tree(model, feature_names = x.columns,
          filled = True, fontsize = 10);
```


    
![output_4_0](https://user-images.githubusercontent.com/76936390/224004244-3cd1fa18-5669-4817-bf7c-714ea633a111.png)
    



```python
# 변수 중요도
result = plot_feature_importance(model.feature_importances_, list(x))
```


    
![output_5_0](https://user-images.githubusercontent.com/76936390/224004254-0f60ca0e-42a4-45b0-88fe-f4320302043e.png)
    


### ✔ **Random Forest**


```python
from sklearn.ensemble import RandomForestRegressor
n_est = 3 # 시각화를 위해 3으로 진행
model = RandomForestRegressor(n_estimators = n_est, max_depth=2)
model.fit(x_train, y_train)

fn=list(x_train)
cn=["0","1"]
fig, axes = plt.subplots(nrows = 1,ncols = n_est,figsize = (24,4))
for index in range(0, n_est):
    plot_tree(model.estimators_[index],
                   feature_names = fn, 
                   class_names=cn,
                   filled = True, fontsize = 10,
                   ax = axes[index]);
    axes[index].set_title('Estimator: ' + str(index), fontsize = 12)
    
plt.tight_layout()
plt.show()
```


    
![output_7_0](https://user-images.githubusercontent.com/76936390/224004262-2ab77776-c474-4f8c-825f-729884bfaf24.png)
    



```python
n_est = 50
model = RandomForestRegressor(n_estimators = n_est)
model.fit(x_train, y_train)
result = plot_feature_importance(model.feature_importances_, list(x))
```


    
![output_8_0](https://user-images.githubusercontent.com/76936390/224004268-eea1c972-04cc-4840-9f15-cb9936da2d64.png)
    


### ✔ **XGB**


```python
from xgboost import XGBRegressor, plot_tree, plot_importance
model = XGBRegressor(n_estimators = 10, max_depth = 2)
model.fit(x_train, y_train)

# graphviz가 설치되어야 할 수 있는 시각화
# plt.rcParams['figure.figsize'] = 20,20
# plot_tree(model)
# plt.show()
```




<style>#sk-container-id-1 {color: black;background-color: white;}#sk-container-id-1 pre{padding: 0;}#sk-container-id-1 div.sk-toggleable {background-color: white;}#sk-container-id-1 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-1 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-1 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-1 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-1 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-1 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-1 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-1 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-1 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-1 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-1 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-1 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-1 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-1 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-1 div.sk-item {position: relative;z-index: 1;}#sk-container-id-1 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-1 div.sk-item::before, #sk-container-id-1 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-1 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-1 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-1 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-1 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-1 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-1 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-1 div.sk-label-container {text-align: center;}#sk-container-id-1 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-1 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-1" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>XGBRegressor(base_score=0.5, booster=&#x27;gbtree&#x27;, callbacks=None,
             colsample_bylevel=1, colsample_bynode=1, colsample_bytree=1,
             early_stopping_rounds=None, enable_categorical=False,
             eval_metric=None, feature_types=None, gamma=0, gpu_id=-1,
             grow_policy=&#x27;depthwise&#x27;, importance_type=None,
             interaction_constraints=&#x27;&#x27;, learning_rate=0.300000012, max_bin=256,
             max_cat_threshold=64, max_cat_to_onehot=4, max_delta_step=0,
             max_depth=2, max_leaves=0, min_child_weight=1, missing=nan,
             monotone_constraints=&#x27;()&#x27;, n_estimators=10, n_jobs=0,
             num_parallel_tree=1, predictor=&#x27;auto&#x27;, random_state=0, ...)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-1" type="checkbox" checked><label for="sk-estimator-id-1" class="sk-toggleable__label sk-toggleable__label-arrow">XGBRegressor</label><div class="sk-toggleable__content"><pre>XGBRegressor(base_score=0.5, booster=&#x27;gbtree&#x27;, callbacks=None,
             colsample_bylevel=1, colsample_bynode=1, colsample_bytree=1,
             early_stopping_rounds=None, enable_categorical=False,
             eval_metric=None, feature_types=None, gamma=0, gpu_id=-1,
             grow_policy=&#x27;depthwise&#x27;, importance_type=None,
             interaction_constraints=&#x27;&#x27;, learning_rate=0.300000012, max_bin=256,
             max_cat_threshold=64, max_cat_to_onehot=4, max_delta_step=0,
             max_depth=2, max_leaves=0, min_child_weight=1, missing=nan,
             monotone_constraints=&#x27;()&#x27;, n_estimators=10, n_jobs=0,
             num_parallel_tree=1, predictor=&#x27;auto&#x27;, random_state=0, ...)</pre></div></div></div></div></div>




```python
# plot_importance 의 변수중요도 기본값은 weight
plt.rcParams['figure.figsize'] = 8, 5
plot_importance(model)
plt.show()
```


    
![output_11_0](https://user-images.githubusercontent.com/76936390/224004275-61dd7aa4-3ed5-49df-bd86-4220ad3923d1.png)
    



```python
# model.feature_importances_ : 변수중요도 기본값은 gain
result = plot_feature_importance(model.feature_importances_, list(x),4)
```


    
![output_12_0](https://user-images.githubusercontent.com/76936390/224004278-f1aeaf43-569f-4455-8a02-171eaa3222c0.png)
    



```python
# importance_type = 'cover', 마찬가지로 'weight', 'gain' 됨
plot_importance(model, importance_type='cover')
plt.show()
```


    
![output_13_0](https://user-images.githubusercontent.com/76936390/224004282-7f3dbc26-ec21-454a-9543-e24393f7e459.png)
    


## ✔️ Permutation Feature Importance(PFI)
알고리즘과 상관 없이 변수 중요도를 파악할 수 있는 방법
- Feature 하나의 데이터를 무작위로 섞을 때, model의 score가 얼마나 감소되는지 계산

**✔ PFI 알고리즘 구조**

특정 feature $j$에 대해서 여러 번($K$) 섞고 계산해서 나온 score의 평균을 Base Line Score에서 빼서 최종 중요도($i$)를 계산한다.

$$
\Large i_j = s - \frac{1}{K}\sum_{k=1}^K s_{k,j}
$$

단점 : <mark>만약 다중 공선성이 있는 변수가 존재하면, score가 별로 줄어들지 않을 수 있음</mark>

시각화는 <mark>BoxPlot</mark>이 가장 잘 보이므로 추천!

### ✔ SVM


```python
from sklearn.inspection import permutation_importance
model1 = SVR()
model1.fit(x_train_s, y_train)
pfi1 = permutation_importance(model1, x_val_s, y_val, n_repeats=10, 
                              scoring = 'r2', random_state=20)
```


```python
sorted_idx = pfi1.importances_mean.argsort()
plt.figure(figsize = (10, 8))
plt.boxplot(pfi1.importances[sorted_idx].T, vert=False, labels=x.columns[sorted_idx])
plt.axvline(0, color = 'r')
plt.grid()
plt.show()
```
    
![output_18_0](https://user-images.githubusercontent.com/76936390/224004290-ba10f0e8-c7b5-45b0-a767-aea453ddf95e.png)
    

```python
plt.figure(figsize = (10,8))
for i,vars in enumerate(list(x)) :
    sns.kdeplot(pfi1.importances[i], label = vars)

plt.grid()
plt.legend()
plt.show()
```
    
![output_17_0](https://user-images.githubusercontent.com/76936390/224004286-47c98993-12f3-4678-b12c-401d26218710.png)
    
평균값으로 변수 중요도 그래프 그리기

```python
result = plot_feature_importance(pfi1.importances_mean, list(x_train))
```

![output_19_0](https://user-images.githubusercontent.com/76936390/224290974-fc5baae7-9ccf-41ba-99f2-a49cccb02225.png)


### ✔ Deep Learning
딥러닝 모델에도 그대로 적용 가능하나, 딥러닝 모델에 대해서는 `permutation_importance` 함수를 사용할 때, 명시적으로 `scoring='r2'`를 지정해줘야 한다.
