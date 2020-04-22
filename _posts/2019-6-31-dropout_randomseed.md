---
layout: post
title: "Dropout random seed"
date: 2019-6-31
---
### assignment2에서 dropout 적용할 때 random seed 변경

`layers.py`에서 `dropout_forward` 함수를 들여다보면 인풋으로 `x`(array), `dropout_param`(dictionary)을 받습니다. `dropout_param`에는 `seed`가 들어있는데 이 `seed` 값으로 난수생성 시드를 초기화하고, 그 이후에 `np.random.rand`를 이용해서 `dropout_mask`를 랜덤하게 생성하게 됩니다.

그런데 `FullyConnectedNet` 모델이 여러 개의 레이어를 통과하는 동안 `seed` 값이 일정하게 유지된다면 `dropout_mask`도 역시 똑같은 값만 나타나게 됩니다. 이것이 문제가 되는 이유는 `dropout_mask`를 *랜덤*하게 생성한다는 의미를 완전히 상실한 셈이기 때문입니다. 결과적으로 특정 위치의 노드들은 loss에 미치는 영향력이 영원히 0이 될 것입니다.

아래에서는 인풋 레이어와 히든 레이어의 크기가 모두 5인 4-layer-net을 만들어서 각 레이어마다 `dropout_mask`가 어떻게 생성되는지 실험을 해봤습니다. 결과는 예상했던 대로 모든 레이어에서 동일한 `dropout_mask`가 나타났습니다. 이 문제를 해결하기 위해서 각 레이어를 지날 때마다 `self.dropout_param['seed'] += 1`를 실행시켜주면 dropout을 할 때 매번 다른 `seed`를 사용하게 됩니다.

```python
import numpy as np
from cs231n.classifiers.fc_net import *
```

```python
# 데이터 생성
np.random.seed(231)
N, D, H1, H2, H3, C = 4, 5, 5, 5, 5, 3
X = np.random.randn(N, D)
y = np.random.randint(C, size=(N,))

dropout = 0.6
# 4-layer net: input, hidden layer 모두 size 5
model = FullyConnectedNet([H1, H2, H3], input_dim=D, num_classes=C,
                          weight_scale=5e-2, dtype=np.float64,
                          dropout=dropout, seed=123)

# initial loss 계산
# dropout layer를 통과할 때마다 해당 dropout cache를 출력하도록 설정함
loss, grads = model.loss(X, y)
```

> {'mode': 'train', 'p': 0.6, 'seed': 123}  
> [[0.         1.66666667 1.66666667 1.66666667 0.        ]  
>  [1.66666667 0.         0.         1.66666667 1.66666667]  
>  [1.66666667 0.         1.66666667 1.66666667 1.66666667]  
>  [0.         1.66666667 1.66666667 1.66666667 1.66666667]]  
> {'mode': 'train', 'p': 0.6, 'seed': 123}  
> [[0.         1.66666667 1.66666667 1.66666667 0.        ]  
>  [1.66666667 0.         0.         1.66666667 1.66666667]  
>  [1.66666667 0.         1.66666667 1.66666667 1.66666667]  
>  [0.         1.66666667 1.66666667 1.66666667 1.66666667]]  
> {'mode': 'train', 'p': 0.6, 'seed': 123}  
> [[0.         1.66666667 1.66666667 1.66666667 0.        ]  
>  [1.66666667 0.         0.         1.66666667 1.66666667]  
>  [1.66666667 0.         1.66666667 1.66666667 1.66666667]  
>  [0.         1.66666667 1.66666667 1.66666667 1.66666667]]

