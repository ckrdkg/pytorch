# 1. What is Pytorch?
  
가. 2017년 facebook이 개발한 오픈소스 머신러닝 라이브러리

나. Defin by Run 방식   

1) Numpy vs Pytorch   
![image](https://user-images.githubusercontent.com/34912004/122864429-992ab980-d35f-11eb-98fa-4a493f8efb6e.png)   
2) Tensorflow vs Pytorch   
![image](https://user-images.githubusercontent.com/34912004/122864998-954b6700-d360-11eb-9a23-afacf08c82d5.png)   
Tensorflow : 모델을 먼저 만들고 그 후에 값을 따로 넣어주는 방식(Denfine and Run)   
Pytorch : 연산의 정의와 동시에 값 초기화가 이루어짐

다. GPU를 통한 연산 가능   
3) CUDA: 엔비디아에서 GPU를 통한 연산을 가능하게 만든 API 모델   
4) cuDNN: CUDA를 이용해 딥러닝 연산을 가속시켜 주는 라이브러리   
# 2. Tensor   
![image](https://user-images.githubusercontent.com/34912004/122886313-d0f32a80-d37a-11eb-9b2e-93922fb1a1f7.png)
- 1차원: 벡터, 2차원: 행렬(Matrix), 3차원: 텐서(Tensor)   
- 2차원 텐서 (batch size, dim)   
  - 예시   
  - 길이가 256인 데이터가 3,000개 있을 경우 데이터의 크기는 3,000 * 256   
  - 3,000개 중 64개씩 처리한다고 가정하면 2D 텐서의 크기는 64*256   
- 비전에서의 3차원 텐서 (batch size, width, height)   
  - 예시   
  - 가로(width), 세로(height)의 이미지가 여러 장(batch size)으로 구성   
- NLP에서의 3차원 텐서 (batch size, length, dim)   
  - 예시   
  - 문장 개수, 문장 길이, 단어 벡터 차원으로 구성   

# 3. Autograd
정답 - 예측 = 거리라고 했을 때, 거리의 평균이 오차(loss)   
즉, 오차가 적은 모델일수록 잘 학습된 좋은 모델   
Gradient Descent(경사 하강법) : 오차 f(x)를 미분한 후 함수의 기울기를 구해서 오차 최솟값이 작은 방향을 찾아내는 알고리즘   
![image](https://user-images.githubusercontent.com/34912004/122876778-dc415880-d370-11eb-8909-e02211324377.png)

가. Pytorch에는 Autograd 패키지가 있음   
  1) Tensor의 모든 연산에 대해 자동 미분화 함수(or 미분 계산 자동화 함수?)를 사용해서 GD를 구현하지 않고도 사용 가능   
  2) Tensor 생성 후 requires_grad = True로 설정하면 해당 tensor의 모든 연산들을 tracking함.   
  3) 연산이 끝난 후에 backward()를 호출하면 변화도(기울기)를 자동으로 계산함.   
  4) requires_grad, backward 예시   
  ![image](https://user-images.githubusercontent.com/34912004/122884019-aef8a880-d378-11eb-9d5b-4c5a99303ae6.png)

  6) tracking 중지 명령 -> .detach()   
  7) gradient가 필요없지만 requires_grad = True로 설정되어서 학습 가능한 파라미터를 가진 모델을 evaluation할 때 -> with torch.no_grad()


* Pytorch에서 메소드 뒤 _는 inplace를 의미
