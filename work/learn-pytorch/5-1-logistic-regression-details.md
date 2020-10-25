---
title: \[PyTorch\] 5.1 - 로지스틱 회귀의 이론적 배경
excerpt: 솔직히 저도 아직 이해 못 했어요
date: 2019-11-14
image: https://unsplash.it/1200/628?image=1075
tags: work
---
# Logistic Regression
## 로지스틱 회귀란?
로지스틱 회귀는 선형 회귀와 달리 "사건이 발생할 가능성"을 예측합니다. 어떤 특별한 값이 있는 게 아니라 사건이 일어나거나 일어나지 않는 경우만 있는 것이며, 이러한 특성으로 인해 로지스틱 회귀는 보통 분류에 사용됩니다.
## Odds
오즈(Odds)는 어떤 사건이 일어날 확률이 그렇지 않을 확률에 비해 몇 배 더 높은지를 나타냅니다.
## Logit
로짓 함수는 ![logit(p) = log(p/1-p)](https://latex.codecogs.com/svg.latex?logit%28p%29%3Dlog%28%7B%7Bp%7D%5Cover%7B1-p%7D%7D%29) 형태의 함수로 입력 값의 범위가 ![[0,1]](https://latex.codecogs.com/svg.latex?%5B0%2C1%5D)일 때 출력 값의 범위를 ![(-∞,∞)](https://latex.codecogs.com/svg.latex?%28-%5Cinfty%2C%20%5Cinfty%29)로 조정합니다. 여기에서 사건이 일어난 경우(1)와 사건이 일어나지 않은 경우(0)는 각각 ![∞](https://latex.codecogs.com/svg.latex?%5Cinfty)와 ![-∞](https://latex.codecogs.com/svg.latex?-%5Cinfty)에 위치하게 되는데, 로지스틱 회귀는 이렇게 로짓 변환된 데이터에서 선형 회귀를 사용하는 것과 유사합니다.
# Coefficients
로지스틱 회귀에서는 선형 회귀를 통해 로짓 함수, 즉 ![logit(p) = log(p/1-p)](https://latex.codecogs.com/svg.latex?logit%28p%29%3Dlog%28%7B%7Bp%7D%5Cover%7B1-p%7D%7D%29)를 구합니다. 이러한 선형 회귀의 결과는 ![y=Wx+b](https://latex.codecogs.com/svg.latex?y%3DWx&plus;b) 형태로 나타나고, 여기에서 Y절편 B와 기울기 W가 실제로 우리가 추측해야 하는 계수입니다.
이 때, 각 계수가 통계적으로 얼마나 유의미한지 다음과 같이 판단할 수 있습니다.
## 계수의 통계적 유의미성
### Z-Value
Z-Value는 예측된 모델의 Y 절편을 표준 오차로 나눈 것입니다. 이 값이 2보다 크거나 같으면 통계적으로 유의미하다 할 수 있습니다.
### P-Value
P-Value는 정규분포곡선에서 Z-Value보다 더 극단적인 값이 발생할 확률을 말합니다. 이 값이 0에 가까울수록 통계적으로 유의미하다 할 수 있습니다.
## 이산적인 변수에 대해
확률을 추측하는 데에 사용되는 변수가 연속적이지 않고 이산적일 경우에는 각 변수에 대해 해당 변수가 참일 경우 사건이 일어날 확률을 구하여 로지스틱 회귀를 실행할 수 있습니다. 이 경우 변수의 값에 따라 사건이 일어날 확률들이 로지스틱 회귀의 계수가 됩니다.

# Maximum Likelihood
선형으로 변환된 로지스틱 회귀에서는 데이터의 값이 양의 무한대 혹은 음의 무한대이기 때문에 오차를 정확히 정의할 수 없습니다. 따라서 오차 대신에 모델의 정확성을 설명할 수 있는 값이 필요한데, "예측된 모델 하에서 해당 데이터셋이 존재할 확률"을 구하면 이 값이 높을수록 모델이 정확하다고 판단할 수 있습니다.
이 값은 모델 하에서 각 데이터가 존재할 확률의 확률곱으로 구할 수 있고, 다시 말하자면 각 데이터의 X 값이 우리 모델의 회귀선상에서 어떤 Y 값에 대응하는지 구한 뒤 이에 따라 해당 X값에서 사건이 일어나거나 일어나지 않을 확률을 구한 뒤 모두 곱하는 것입니다.
곱의 로그는 로그의 합으로 표현할 수 있기 때문에, 실제로는 각 데이터가 존재할 확률의 로그의 합을 주로 사용합니다. 이 값이 최대가 되도록 하는 것이 로지스틱 회귀의 목적입니다.

# R^2^ and P-Values
사실 로지스틱 회귀의 R^2^ 값을 계산하는 것은 상당히 복잡하기 때문에 열 가지 이상의 방법들이 존재합니다. 이 강의에서는 맥패든의 의사 R^2^(Pseudo R^2^)를 사용합니다.
## McFadden's Pseudo R^2^
회귀선상에 모든 데이터를 배열한 뒤 각 데이터가 존재할 확률의 로그의 합을 구하고, 예측된 모델 하에서 데이터셋이 발생할 확률인 이 값을 LL(Fit)이라 부릅니다. 또한 사건이 발생할 확률을 사건이 발생하지 않을 확률으로 나눈 값을 구하고, 별도의 변수가 없을 때 데이터셋이 발생할 확률인 이 값을 LL(overall probability)라고 부릅니다. 이 때 Pseudo R^2^는 ![R^2 = {{LL(overall\: probability) - LL(fit)}\over LL(overall\: probability)}](https://latex.codecogs.com/svg.latex?R%5E2%20%3D%20%7B%7BLL%28overall%5C%3A%20probability%29%20-%20LL%28fit%29%7D%5Cover%20LL%28overall%5C%3A%20probability%29%7D) 로 정의할 수 있고, 이 값을 통해 우리가 예측한 모델이 얼마나 통계적으로 유의미한지 계산할 수 있습니다.
> Written with [StackEdit](https://stackedit.io/).


> Written with [StackEdit](https://stackedit.io/).
