# 1. Maximum Likelihood Estimation (MLE)
- 최대 우도 추정 or 최대 가능도 추정
- Likelihood 
    - 압정을 던졌을 때 평평한 면이 아래로 가는 case1과 뾰족한 면이 아래로 가는 case2 2가지가 존재
    - 압정이 어떻게 떨어질지 예측 (주어진 관측치나 데이터만으로 parameter 추정)
![](2021-06-28-23-41-50.png)

# 2. Overfitting
- 학습 데이터를 과도하게 학습하는 것
- 학습 데이터에 대한 오차는 줄지만 실제 데이터에서의 오차는 증가
![](2021-06-28-23-44-57.png)
- 해결 방법
    - Validation set (0.8, 0.1, 0.1)
    - More data
        - 데이터가 적을수록 편향된 데이터를 얻을 가능성이 높음
    - Less features
    - Regularization
        - Early Stopping
        - Dropout
        - Batch Normalization
        - L1
            - 실제값과 예측값 사이의 오차의 절댓값 합
        - L2
            - 실제값과 예측값 사이의 오차 제곱 합
        - L2 loss는 오차의 제곱 합을 더해나가기 때문에 Outlier(이상치)에 더욱 민감하게 반응 
        - 따라서 L1 loss가 L2 loss에 비해 Outlier에 좀 더 robust, 즉 둔감하다고 할 수 있음
        - 그렇기에 Outlier가 중요한 상황이라면 L1 loss보다 L2 loss를 쓰는 것이 더 좋다고 함.