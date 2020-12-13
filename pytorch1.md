#20.12.13

# 1. What is Pytorch?
가. 2017년 facebook이 개발한 오픈소스 머신러닝 라이브러리
나. Defin by Run 방식
1 ) Tensorflow vs Pytorch
Tensorflow : 모델을 먼저 만들고 그 후에 값을 따로 넣어주는 방식(Denfine and Run)
Pytorch : 연산의 정의와 동시에 값 초기화가 이루어짐
다. GPU를 통한 연산 가능
1) CUDA: 엔비디아에서 GPU를 통한 연산을 가능하게 만든 API 모델
2) cuDNN: CUDA를 이용해 딥러닝 연산을 가속시켜 주는 라이브러리

# 2. Tensor
##### Tensor: Numpy의 ndarray와 유사, GPU를 사용한 연간 가속 가능
![2020-12-08-23-11-58](https://user-images.githubusercontent.com/34912004/102002875-420ab300-3d44-11eb-9b97-54d55a635ade.png)

초기화되지 않은 행렬 선언
print_function: Python 3.x와 2.x의 print 형식을 print()로 취급하도록 함.


##### 초기화되지 않은 5x3 행렬
![2020-12-08-23-13-37](https://user-images.githubusercontent.com/34912004/102002879-5189fc00-3d44-11eb-8007-e569d4fe72cf.png)


##### 무작위로 초기화된 행렬 생성
![2020-12-08-23-16-04](https://user-images.githubusercontent.com/34912004/102002885-5e0e5480-3d44-11eb-87b6-491339a0cf52.png)


##### Tensor 자료형
![2020-12-08-23-29-14](https://user-images.githubusercontent.com/34912004/102002888-69618000-3d44-11eb-9dbb-1282af211597.png)
![2020-12-08-23-29-32](https://user-images.githubusercontent.com/34912004/102002889-6b2b4380-3d44-11eb-8c77-6cbe335daf56.png)


##### tensor를 생성하는 가장 일반적인 방법
가. 직접 생성

![2020-12-08-23-30-33](https://user-images.githubusercontent.com/34912004/102002892-73837e80-3d44-11eb-8985-3623300f5b6d.png)


나. 기존 tensor를 바탕으로 새로운 tensor 생성

![2020-12-09-18-15-44](https://user-images.githubusercontent.com/34912004/102002904-80a06d80-3d44-11eb-8478-98eed6c853b4.png)

다. Tensor 생성
1 ) torch.rand() : 0과 1 사이의 숫자를 균등하게 생성
2 ) torch.rand_like() : 사이즈를 튜플로 입력하지 않고 기존의 텐서로 정의
3 ) torch.randn() : 평균이 0이고 표준편차가 1인 가우시안 정규분포를 이용해 생성
4 ) torch.randn_like() :  사이즈를 튜플로 입력하지 않고 기존의 텐서로 정의
5 ) torch.randint() : 주어진 범위 내의 정수를 균등하게 생성, 자료형은 torch.float32
6 ) torch.randint_like() : 사이즈를 튜플로 입력하지 않고 기존의 텐서로 정의
7 ) torch.randperm() : 주어진 범위 내의 정수를 랜덤하게 생성

##### tensor 간 연산
방법1.

![2020-12-09-19-37-50](https://user-images.githubusercontent.com/34912004/102002907-8b5b0280-3d44-11eb-97e4-596175504801.png)

방법2.

![2020-12-09-19-37-59](https://user-images.githubusercontent.com/34912004/102002908-8c8c2f80-3d44-11eb-91ba-026e856abbfc.png)

##### tensor의 size나 shape 변경
![2020-12-09-19-56-15](https://user-images.githubusercontent.com/34912004/102002912-96ae2e00-3d44-11eb-811a-6e660f071f0d.png)

##### tensor to numpy, numpy to tensor
![2020-12-09-19-57-33](https://user-images.githubusercontent.com/34912004/102002917-9dd53c00-3d44-11eb-8868-8a4fe56c7982.png)

tensor와 numpy는 메모리를 공유하기 때문에 하나를 변경하면 다른 하나도 변경된다.

# 3.Autograd: 자동 미분
requires_grad = True : 이루어지는 연산들을 tracking
backward: gradient 전파
![2020-12-09-22-23-53](https://user-images.githubusercontent.com/34912004/102002931-bc3b3780-3d44-11eb-888d-550d7acdce28.png)
![2020-12-09-22-23-33](https://user-images.githubusercontent.com/34912004/102002924-aa599480-3d44-11eb-94b5-c23a5a9efd43.png)


##### 연산 tracking 정지
![2020-12-09-22-27-30](https://user-images.githubusercontent.com/34912004/102002948-d248f800-3d44-11eb-98a7-4251d44f5be1.png)

![2020-12-09-22-30-01](https://user-images.githubusercontent.com/34912004/102002949-d248f800-3d44-11eb-843b-de1c2454ed68.png)

##### detach vs no_grad
detach: grad가 필요하지 않는 tensor를 만든다. computational graph에서 output을 detach, 주어진 변수만 참조
no_grad: 모든 requires_grad를 false로 설정, gradient가 필요하지 않으므로 intermediary result가 필요없음 때문에 메모리를 덜 사용함
