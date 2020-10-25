---
title: \[PyTorch\] 2 - 선형회귀
excerpt: 이런 거 쓴다고 누가 읽으러 오나
date: 2019-11-07
image: https://unsplash.it/1200/628?image=1075
tags: ["work"]
---

# 들어가며

/<모두를 위한 딥러닝 강의/> 2강에서는 Linear Regression을 활용한 학습을 연습해 봅니다. 본 포스트는 제가 학습한 내용을 복기해 두기 위한 것이며, 원본 강의와 슬라이드는 아래에서 보실 수 있습니다.

[유튜브 강의](https://youtu.be/kyjBMuNM1DI)
[슬라이드](https://drive.google.com/file/d/1zKsvxMZAsZDsPWO5vokxlA5wGZBdbB8S/view?usp=sharing)

# Data Definition

Data Definition은 학습에 사용할 데이터를 정의하는 과정입니다. 이 강의에서는 "학습 시간과 성적의 관계"를 학습하는 상황을 상정합니다.

## Training Dataset

모델을 만들고 학습시키려면 우선 학습할 데이터가 있어야 합니다. 예를 들어, "학습 시간과 성적의 관계"에 대해 학습하려면 이전에 `x`시간 학습한 학생들이 `y`의 성적을 거두었다는 데이터들이 있어야 하다는 것이죠. 이를 Training Dataset이라고 부릅니다. 여기서는 다음과 같은 데이터가 있다고 합시다.

|학습 시간(x)|점수(y)|
|--|--|
|1|2|
|2|4|
|3|6|

## Test Dataset

모델이 잘 작동하는 지 테스트하기 위한 데이터의 모임을 Test Dataset이라고 부릅니다.

## 데이터 입력하기

데이터는 1강에서 본 `torch.tensor` 형식으로 입력됩니다. 시간과 성적은 `(1)` 크기의 실수 텐서라고 생각할 수 있을테니, 이들의 모임은 `(n, 1)` 크기의 실수 텐서가 됩니다.

입력은 x, 출력은 y라고 구분합니다. 즉, Training Dataset에 있는 3개의 데이터의 입력과 출력은 각각 `x_train`, `y_train`이라는 변수에 다음과 같이 저장하게 됩니다.

```python
x_train = torch.FloatTensor([[1], [2], [3]])
y_train = torch.FloatTensor([[2], [4], [6]])
```

# Hypothesis

이렇게 데이터만 주면 인-공 지능이 알아서 공부를 해 주는 거라면 좋겠지만, 입력과 출력의 관계가 대충 어떻게 될 거라는 가설(Hypothesis) 정도는 우리가 직접 정해 줘야 합니다. 
선형 회귀, 즉 Linear Regression은 입력과 출력의 관계가 선형적일 것이는 가정을 기반으로 합니다. 즉, 입력 x에 대해 출력 y는 ![y=Wx+b](https://latex.codecogs.com/svg.latex?%5Cdpi%7B150%7D%20y%3DWx&plus;b)라는 식을 따를 것이라고 가정하는 것이죠.

이 때, ![W](https://latex.codecogs.com/svg.latex?%5Cdpi%7B150%7D%20W)를 Weight, ![b](https://latex.codecogs.com/svg.latex?%5Cdpi%7B150%7D%20b)를 bias라고 부릅니다.

## 구현

우리의 가정을 파이썬 코드로 나타내면 당연히,
```python
hypothesis = x_train*W+b
```
일 것입니다. 이 때 W와 b는 어떻게 정의해 줘야 할까요? 아직 모델이 학습되지 않았으니 우리는 W와 b에 어떤 값이 들어가야 할지 알지 못합니다. 그러니 우선 모든 값이 0으로 초기화된 텐서를 만들어 줍시다. 이는 `torch.zeros` 함수로 진행할 수 있습니다.

```python
torch.zeros(*size, out=None, dtype=None, layout=torch.strided, device=None, requires_grad=False): Tensor
```
> *Prototype from [Pytorch Docs](https://pytorch.org/docs/stable/torch.html)*

여기에서 우리가 주목해야 할 것은 `size`와 `requires_grad`입니다. `size`는 하나 이상의 정수로 반환되는 텐서의 모양을 결정하고, `requires_grad`를 `True`로 설정하면 PyTorch가 해당 텐서를 Gradient를 통해 학습해야 함을 명시할 수 있습니다.

정리하면, 모양이 (1)이고 학습이 필요한 텐서를 사용하는 우리는 다음과 같은 코드로 Hypothesis를 표현할 수 있습니다.

```python
W = torch.zeros(1, requires_grad = True)
b = torch.zeros(1, requires_grad = True)
hypothesis = x_train * W + b
```

# Loss

이렇게 모델이 어떤 가정을 사용하는지는 알게 되었습니다. 이제 이 모델을 학습시키려면 모델이 얼마나 정확하며 어떻게 해야 정확도를 높일 수 있는지 알아야 합니다.

그 방법은 모델이 "부정확한 정도"를 Loss라는 변수로 정의하고, Training Dataset에서 모델의 오차를 통해 Loss(aka cost)를 계산한 다음 Loss를 줄이는 방향으로 모델을 변경하는 것입니다. 우리는 여기에서 Mean Squared Error (MSE) 라는 방식으로 Loss를 정의합니다.

![Equation](https://latex.codecogs.com/svg.latex?%5Cdpi%7B150%7D%20cost%28W%2Cb%29%3D%20%7B1%20%5Cover%20m%7D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%28H%28x%5E%7B%28i%29%7D%29-y%5E%7B%28i%29%7D%29%5E2)

Mean Squared Error는 말 그대로 오차(Error)의 제곱(Square)의 평균(Mean)입니다. 즉, 예측값인 ![Equation](https://latex.codecogs.com/svg.latex?%5Cdpi%7B150%7D%20H%28x%5E%7B%28i%29%7D%29)와 ![Equation](https://latex.codecogs.com/svg.latex?%5Cdpi%7B150%7D%20y%5E%7B%28i%29%7D)의 차의 제곱의 평균이 됩니다. 이 값이 0에 가장 가까워지도록 하는 것이 Linear Regression의 목표입니다.

## 구현
다행히 PyTorch는 이미 평균을 `Tensor` 형식으로 구하는 함수인 `mean`을 제공하므로, MSE를 통해 cost를 구하는 식은 비교적 단순히 구현할 수 있습니다.

```python
cost = torch.mean((hypothesis - y_train) ** 2)
```

# Gradient Descent

Gradient Descent는 loss의 Gradient를 계산하여 위에서 구한 cost를 0으로 줄이는 과정입니다. 이것의 원리에 대해서는 3강에서 더 자세히 알아보겠지만, 여기에서는 PyTorch에서 Gradient Descent를 통해 모델을 학습시켜 최적화하는 방법을 알아봅니다.

## torch.optim
우선, PyTorch에서는 모델을 최적화하기 위한 방법('Optimizer')들을 `torch.optim` 라이브러리를 통해 제공하고 있습니다. 이 중에서 Gradient Descent는 `SGD`라는 클래스로 제공되고 있습니다.

```python
torch.optim.SGD(params, lr=<required parameter>, momentum=0, dampening=0, weight_decay=0, nesterov=false)
```
> *Prototype from [PyTorch Docs](https://pytorch.org/docs/stable/optim.html?highlight=optim#module-torch.optim)*

여기에서 `params`에는 우리가 최적화할 변수의 리스트가 들어가고, `lr`에는 Learning Rate가 들어갑니다. Learning Rate는 말 그대로 '학습 속도'인데, 이것의 기술적 의미는 3강에서 자세히 설명됩니다. 우선 `lr`을 0.01로 설정해 보면 다음과 같은 코드가 됩니다.

```python
optimizer = optim.SGD([W, b], lr = 0.01)
```

이 클래스는 학습을 수행하기 위해 다음과 같은 메소드를 제공합니다.

```python
zero_grad()
```

> *Prototype from [PyTorch Docs](https://pytorch.org/docs/stable/optim.html?highlight=optim#module-torch.optim)*

`zero_grad`는 `SGD`를 비롯하여 모든 Optimizer 클래스들이 상속하는 `torch.optim.Optimizer` 클래스에서 제공하는 메소드로, Optimize될 모든 텐서(즉, optimizer를 초기화할 때 `param`에 들어간 텐서들)의 그라디언트를 0으로 초기화합니다.

```python
step(closure=None)
```

> *Prototype from [PyTorch Docs](https://pytorch.org/docs/stable/optim.html?highlight=optim#module-torch.optim)*

`step`을 실행하면 각 텐서에 저장된 loss gradient에 따라 Optimization이 실행됩니다.

## Gradient 계산하기

PyTorch의 `Tensor`가 제공하는 `backward` 함수를 통해 각 텐서의 그라디언트를 계산하여 저장할 수 있습니다. `cost.backward()`를 실행하기만 하면 `W`와 `b`에 따른 `cost`의 그라디언트가 계산되어 `W`와 `b`에 대해 각각 저장되는 거죠.

## 구현

위에서 설명된 내용을 종합하면, 다음의 과정을 반복함으로써 Optimization을 실행할 수 있습니다.
* Gradient를 초기화한다.
* Gradient를 계산한다.
* Gradient에 따라 Gradient Descent를 실행한다.

이를 코드로 구현하면 다음과 같습니다.

```python
hypothesis = x_train * W + b
cost = torch.mean((hypothesis - y_train) ** 2)

optimizer.zero_grad()
cost.backward()
optimizer.step()
```

이 과정을 한 번 반복하는 것을 `epoch`라고 부릅니다.

# 최종 코드

지금까지 설명한 내용을 종합한 코드는 다음과 같습니다.

```python
import torch
import torch.optim as optim

x_train = torch.FloatTensor([[1], [2], [3]])
y_train = torch.FloatTensor([[2], [4], [6]])

W = torch.zeros(1, requires_grad = True)
b = torch.zeros(1, requires_grad = True)

optimizer = optim.SGD([W, b], lr = 0.01)

nb_epochs = 1000 # Number of epochs

for epoch in range(nb_epochs):
    hypothesis = x_train * W + b # Calculate H(x)
    cost = torch.mean((hypothesis - y_train) ** 2) # Calculate cost
    
    optimizer.zero_grad() # Grad to zero
    cost.backward() # Calculate grad
    optimizer.step() # Optimize
```
---
```python
print('W   : '+str(W))
print('b   : '+str(b))
print('cost: '+str(cost))
```
를 사용해 `nb_epoch`가 10, 100, 1000...일 때 `W`, `b`와 `cost`의 값을 출력해 보면 epoch를 많이 반복할수록 최적값에 가까워짐을 알 수 있습니다.

```python
# nb_epoch = 10
W   : tensor([1.1663], requires_grad=True)
b   : tensor([0.4922], requires_grad=True)
cost: tensor(2.3139, grad_fn=<MeanBackward1>)
# nb_epoch = 100
W   : tensor([1.7451], requires_grad=True)
b   : tensor([0.5795], requires_grad=True)
cost: tensor(0.0484, grad_fn=<MeanBackward1>)
# nb_epoch = 1000
W   : tensor([1.9708], requires_grad=True)
b   : tensor([0.0664], requires_grad=True)
cost: tensor(0.0006, grad_fn=<MeanBackward1>)
# nb_epoch = 10000
W   : tensor([2.0000], requires_grad=True)
b   : tensor([8.7018e-06], requires_grad=True)
cost: tensor(1.2278e-11, grad_fn=<MeanBackward1>)
```

---
> Written with [StackEdit](https://stackedit.io/).
