---
title: \[PyTorch\] 0 - 설치하기
excerpt: 이런 거 쓴다고 누가 읽으러 오나
date: 2019-10-08
image: https://unsplash.it/1200/628?image=1075
tags: ["work"]
---
# 시작하기 전에
이 글은 제가 PyTorch를 공부하면서 정리를 목적으로 쓰는 것입니다. 원본 강의는 [모두를 위한 딥러닝 시즌 2](https://deeplearningzerotoall.github.io/season2/)입니다.
Windows 10 AMD64, Python 3.7 환경에서 시작하였습니다.

저는 PIP로 설치하느라 명령어가 꽤 복잡해졌는데, Anaconda를 사용하면 훨씬 간편해집니다. 전체 설치 과정은 [여기](https://pytorch.org/get-started/locally/)에서 확인해 주세요!

# 설치하기
## numpy 설치하기
```pip3 install numpy```

## PyTorch 설치하기
### CPU만 사용하는 경우
```pip3 install https://download.pytorch.org/whl/cpu/torch-1.0.1-cp37-cp37m-win_amd64.whl```

### CUDA 8
```pip3 install https://download.pytorch.org/whl/cu80/torch-1.0.1-cp37-cp37m-win_amd64.whl```

### CUDA 9
```pip3 install https://download.pytorch.org/whl/cu90/torch-1.0.1-cp37-cp37m-win_amd64.whl```

### CUDA 10
```pip3 install https://download.pytorch.org/whl/cu100/torch-1.0.1-cp37-cp37m-win_amd64.whl```

## Torchvision 설치하기
> The torchvision package consists of popular datasets, model architectures, and common image transformations for computer vision.
*- PyTorch master documentation*

Torchvision은 컴퓨터 비전에 유용한 도구들을 포함하는 패키지입니다.
```pip3 install torchvision```

> Written with [StackEdit](https://stackedit.io/).
