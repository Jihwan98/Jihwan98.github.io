---
title:  "GridSearch 사용 방법 및 학습 과정 시각화"
author: JIHWAN PARK
categories: [AI & 데이터분석, Python]
tag: [Python, Scikit-learn, Model]
math: true
date: 2023-03-09 18:00:00 +0900
last_modified_at: 2023-03-09 18:00:00 +0900
---
> Hypyer Parameter Tunning 중 GridSearch의 사용 방법 및 학습 과정 시각화 방법을 알아보자.
{: .prompt-info}

## ✅ Scikit-learn을 활용한 GridSearch

```python
from sklearn.model_selection import GridSearchCV

params = {'learning_rate' : np.linspace(0.01, 0.5, 20), 'n_estimators' : [50, 100, 150]}

model = GridSearchCV(XGBClassifier(), params, cv = 5)
model.fit(train_x, train_y)
```


<style>#sk-container-id-1 {color: black;background-color: white;}#sk-container-id-1 pre{padding: 0;}#sk-container-id-1 div.sk-toggleable {background-color: white;}#sk-container-id-1 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-1 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-1 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-1 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-1 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-1 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-1 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-1 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-1 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-1 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-1 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-1 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-1 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-1 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-1 div.sk-item {position: relative;z-index: 1;}#sk-container-id-1 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-1 div.sk-item::before, #sk-container-id-1 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-1 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-1 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-1 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-1 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-1 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-1 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-1 div.sk-label-container {text-align: center;}#sk-container-id-1 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-1 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-1" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>GridSearchCV(cv=5,
             estimator=XGBClassifier(base_score=None, booster=None,
                                     callbacks=None, colsample_bylevel=None,
                                     colsample_bynode=None,
                                     colsample_bytree=None,
                                     early_stopping_rounds=None,
                                     enable_categorical=False, eval_metric=None,
                                     feature_types=None, gamma=None,
                                     gpu_id=None, grow_policy=None,
                                     importance_type=None,
                                     interaction_constraints=None,
                                     learning_rate=None,...
                                     n_estimators=100, n_jobs=None,
                                     num_parallel_tree=None, predictor=None,
                                     random_state=None, ...),
             param_grid={&#x27;learning_rate&#x27;: array([0.01      , 0.03578947, 0.06157895, 0.08736842, 0.11315789,
       0.13894737, 0.16473684, 0.19052632, 0.21631579, 0.24210526,
       0.26789474, 0.29368421, 0.31947368, 0.34526316, 0.37105263,
       0.39684211, 0.42263158, 0.44842105, 0.47421053, 0.5       ]),
                         &#x27;n_estimators&#x27;: [50, 100, 150]})</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item sk-dashed-wrapped"><div class="sk-label-container"><div class="sk-label sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-1" type="checkbox" ><label for="sk-estimator-id-1" class="sk-toggleable__label sk-toggleable__label-arrow">GridSearchCV</label><div class="sk-toggleable__content"><pre>GridSearchCV(cv=5,
             estimator=XGBClassifier(base_score=None, booster=None,
                                     callbacks=None, colsample_bylevel=None,
                                     colsample_bynode=None,
                                     colsample_bytree=None,
                                     early_stopping_rounds=None,
                                     enable_categorical=False, eval_metric=None,
                                     feature_types=None, gamma=None,
                                     gpu_id=None, grow_policy=None,
                                     importance_type=None,
                                     interaction_constraints=None,
                                     learning_rate=None,...
                                     n_estimators=100, n_jobs=None,
                                     num_parallel_tree=None, predictor=None,
                                     random_state=None, ...),
             param_grid={&#x27;learning_rate&#x27;: array([0.01      , 0.03578947, 0.06157895, 0.08736842, 0.11315789,
       0.13894737, 0.16473684, 0.19052632, 0.21631579, 0.24210526,
       0.26789474, 0.29368421, 0.31947368, 0.34526316, 0.37105263,
       0.39684211, 0.42263158, 0.44842105, 0.47421053, 0.5       ]),
                         &#x27;n_estimators&#x27;: [50, 100, 150]})</pre></div></div></div><div class="sk-parallel"><div class="sk-parallel-item"><div class="sk-item"><div class="sk-label-container"><div class="sk-label sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-2" type="checkbox" ><label for="sk-estimator-id-2" class="sk-toggleable__label sk-toggleable__label-arrow">estimator: XGBClassifier</label><div class="sk-toggleable__content"><pre>XGBClassifier(base_score=None, booster=None, callbacks=None,
              colsample_bylevel=None, colsample_bynode=None,
              colsample_bytree=None, early_stopping_rounds=None,
              enable_categorical=False, eval_metric=None, feature_types=None,
              gamma=None, gpu_id=None, grow_policy=None, importance_type=None,
              interaction_constraints=None, learning_rate=None, max_bin=None,
              max_cat_threshold=None, max_cat_to_onehot=None,
              max_delta_step=None, max_depth=None, max_leaves=None,
              min_child_weight=None, missing=nan, monotone_constraints=None,
              n_estimators=100, n_jobs=None, num_parallel_tree=None,
              predictor=None, random_state=None, ...)</pre></div></div></div><div class="sk-serial"><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-3" type="checkbox" ><label for="sk-estimator-id-3" class="sk-toggleable__label sk-toggleable__label-arrow">XGBClassifier</label><div class="sk-toggleable__content"><pre>XGBClassifier(base_score=None, booster=None, callbacks=None,
              colsample_bylevel=None, colsample_bynode=None,
              colsample_bytree=None, early_stopping_rounds=None,
              enable_categorical=False, eval_metric=None, feature_types=None,
              gamma=None, gpu_id=None, grow_policy=None, importance_type=None,
              interaction_constraints=None, learning_rate=None, max_bin=None,
              max_cat_threshold=None, max_cat_to_onehot=None,
              max_delta_step=None, max_depth=None, max_leaves=None,
              min_child_weight=None, missing=nan, monotone_constraints=None,
              n_estimators=100, n_jobs=None, num_parallel_tree=None,
              predictor=None, random_state=None, ...)</pre></div></div></div></div></div></div></div></div></div></div>



## ✅ GridSearch 학습 과정 시각화
`model.cv_results_`에는 학습 과정에 대한 많은 정보가 들어있다.


```python
result = pd.DataFrame(model.cv_results_)

plt.figure(figsize = (10, 6))
sns.lineplot(x = 'param_learning_rate', y = 'mean_test_score', data = result,
             marker = 'o', hue = 'param_n_estimators')
plt.grid()
plt.show()
```


    
![output_3_0](https://user-images.githubusercontent.com/76936390/223984823-d2280d1c-62af-46c1-952b-c741fd9c43a5.png)
    


## ✅ KNN에 사용 예시


```python
params = {'n_neighbors' : range(3, 100, 2), 'metric' : ['manhattan', 'euclidean']}

model = GridSearchCV(KNeighborsClassifier(), params, cv=5)
model.fit(train_x_s, train_y)

result = pd.DataFrame(model.cv_results_)

plt.figure(figsize = (10, 6))
sns.lineplot(x = 'param_n_neighbors', y = 'mean_test_score', data = result,
             marker = 'o', hue = 'param_metric')
plt.grid()
plt.show()
```


    
![output_5_0](https://user-images.githubusercontent.com/76936390/223984819-bce536be-e9d2-4433-9720-6678c4d3d8e6.png)
    


## ✅ GridSearchCV 변수에 들어있는 정보들
기본적으로 GridSearch 후 `model.predict`를 하면 자동으로 가장 좋은 성능의 모델로 predict를 진행한다. 하지만 `feature_importances_` 같은 경우는 자동으로 모델을 잡아주지 않아 에러가 난다. 이런 경우, `model.best_estimator_`가 가장 좋은 성능의 모델을 가지고 있음으로 `model.best_estimator_.feature_importances_`로 사용하면 된다. ~~(KNN과 같이 feature_importances_를 가지고 있지 않는 모델의 경우엔 당연히 에러가 날것임)~~

또한, `model.best_score_`, `model.best_params_` 으로 최고 성능을 보여줬을 때의 score 값과 parameter 값을 확인할 수 있다. 이때 score 값은 GridSearchCV에서 넣어준 `scoring=`에 해당하는 값이다. 값을 넣어주지 않으면 default로 분류문제는 accuracy, 회귀 문제는 r2 이다.


```python
model.best_score_, model.best_params_
```




    (0.73, {'metric': 'euclidean', 'n_neighbors': 15})


