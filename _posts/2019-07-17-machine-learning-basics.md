---
layout:     post
title:      "Machine Learning Basics"
subtitle:   " "
date:       2019-07-17 09:45:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: mathematics
---

Regularization is any modification we make to a learning algorithm that is intended to reduce its generalization error but not its training error.

<br>

### 5.3 Hyperparameters and Validation Sets

| Training set | parameter를 학습하기 위해 사용하는 데이터셋 |
| Validation set | 학습을 한 후에 generalization error를 계산하고 hyperparameter를 알맞게 업데이트하기 위해 사용하는 데이터셋|
| Test set | 모델의 성능을 평가하기 위해 사용하는 데이터셋 |

<br>
##### Cross-Validation
데이터셋이 너무 작으면 모델을 학습하고 평가하는 데 결과에 불확실성이 생긴다. 이때 전체 데이터셋에서 training set과 test set을 랜덤으로 여러 번 나누어 모델을 학습하고 평가하는 데 활용한다. 가장 많이 사용되는 방법은 k-fold cross-validation이다. 데이터셋을 겹치지 않게 k개의 subset으로 분할하여 training/validation/test set으로 나누어 k번 실험을 진행하는 것이다. Test error는 k번의 실험 결과 test error의 평균값으로 계산할 수 있다.


<br>
### 5.4 Estimators, Bias and Variance

##### Point Estimation
단 하나의 추정값을 정하는 것이다.