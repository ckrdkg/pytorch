# 가중치 초기화 (Weight Initialization)

## 0. 가중치 초기화의 필요성
- Vanishing Gradient
    - Backpropagation 중 gradient 항이 사라지는 문제   
    ![image](https://user-images.githubusercontent.com/34912004/125193515-af090b80-e287-11eb-88e1-9ca083d84ece.png)
        - 학습이 제대로 이루어지지 않음

## 1. 초기 가중치 설정
- 가중치를 어떻게 초기화하냐에 따라서 성능이 달라짐   
    - layer, neuron, activation func 등과 연관됨   
    <activation funcion 별 가중치 초기화에 따른 error>
    ![image](https://user-images.githubusercontent.com/34912004/125193521-b6301980-e287-11eb-9a2d-1bb6cf1ea6bf.png)
    - sigmoid layer가 추가되면 error가 굉장히 높아짐
- 기존의 가중치 초기화 방식
    - uniform 분포로 랜덤하게 초기화
- 초기의 가중치를 모두 0으로 하거나 동일한 값으로 할 경우, 모든 뉴런이 동일한 output을 내보냄.
    - backpropagation에서 모두 동일한 gradient 값을 갖게 됨.
- 초기 가중치 값은 작은 값으로 해야 함.
    - activation functinon이 Sigmoid일 경우, 0 또는 1로 수렴하기 때문에 vanishing gradient문제 발생
        - why? 활성화 값이 주로 0과 1에 분포되는데 이러면 미분값이 0에 가까워짐
    - activation function이 ReLU일 경우, 음수면 모두 0, 양수면 gradient 폭주가 발생
    - Backpropagation식에서의 Grradient이 0이거나 0에 가까워져 학습 불가 현상 발생
- 일반적으로 가중치 초기값은 평균 0, 표준편차 0.01인 정규분포로 초기화
- 서로 다른 특징을 찾기 위해 neuron을 만드는 건데 동일하게 가중치가 update되면 층을 쌓을 이유가 없음.
- activation function이 Sigmoid일 경우
## 2. 가중치 초기화 방법
- 상수기반, 선형대수 기반, 확률분포 기반 등이 있지만 잘 사용하진 않음
- Lecun Initialization   
![image](https://user-images.githubusercontent.com/34912004/125193526-bf20eb00-e287-11eb-84b3-e36022219081.png)
    - input 크기가 커질수록 초기화 값의 분산을 작게 만드는 것
- Xavier Initialization (sigmoid, tanh)   
![image](https://user-images.githubusercontent.com/34912004/125193528-c3e59f00-e287-11eb-876c-c72642403773.png)
    - fan in, fan out 모두 고려하여 확률 분포 조정
    - Vanishing Gradient를 완화시키기 위해선 Sigmoid, tanh같은 S자 함수가 정규분포 형태를 갖도록 해야 함
    - activation functino이 선형(linear)하다고 가정    
    - sigmoid와 tanh는 좌우 대칭이고 중앙 부분이 선형이라 적용 가능   
    ![image](https://user-images.githubusercontent.com/34912004/125193533-cba54380-e287-11eb-8457-e4e0339079d3.png)
    - ReLU에서는 제대로 작동하지 않으며 He의 제안 배경이 됨.

- He initialization (ReLU)   
![image](https://user-images.githubusercontent.com/34912004/125193539-d2cc5180-e287-11eb-9162-ec397fbc84ec.png)
    - ReLU가 0이하의 activation 값을 제거하므로 fan in을 더 고려한 상태 -> 그래서 2를 더 곱해주는 듯
    - Resnet에서 CNN을 할 때 잘 작동함을 보여줌.
    - activation function이 ReLU일 때 많이 사용되는 가중치 초기화 방법
