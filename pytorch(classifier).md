# Clasiifier

## CIFAR10
- ‘비행기(airplane)’, ‘자동차(automobile)’, ‘새(bird)’, ‘고양이(cat)’, ‘사슴(deer)’, ‘개(dog)’, ‘개구리(frog)’, ‘말(horse)’, ‘배(ship)’, ‘트럭(truck)’의 이미지 (3x32x32) 
- 32x32 픽셀 크기의 이미지가 3개 채널의 색상으로 이루어짐

## Step
1. torchvision을 사용하여 train/test set을 load하고 nomarlizing
2. Convolution Neural Network 정의
3. Loss function 정의
4. train set을 이용하여 neural network train
5. test set을 이용하여 neural network test

##### 1. torchvision을 사용하여 train/test set을 load하고 nomarlizing
* nomarlizing(정규화) : 표준 정규분포로 만들기(x-평균)/표준편차
  
![image](https://user-images.githubusercontent.com/34912004/102006618-abe78480-3d65-11eb-84c3-ecd01004e72e.png)

torchvision을 사용하여 CIFAR10 load

torchvision dataset의 output은 [0,1]의 범위를 가진 PILImage임. 이를 [-1, 1]의 범위로 normalizing한 tensor로 변환
* download: 경로에 data가 없을 시 다운로드
* num_workes: 몇 개의 코어를 사용할 것인지

##### 2. image 확인
![image](https://user-images.githubusercontent.com/34912004/102058835-74470e00-3e33-11eb-908b-aef1845e492c.png)


##### 3. Convolution Neural Network 정의
![image](https://user-images.githubusercontent.com/34912004/102058948-9e003500-3e33-11eb-9828-578b4ed96ea9.png)


*** pooling을 하는 이유?
- CNN에서 특징을 뽑아내는 과정
Max pooling: filter안에서 가장 큰 값만 뽑아냄
Average Pooling: filter 값들의 평균을 뽑아냄
- 목적: input size를 줄임으로써(특징만 뽑아냄으로써) 효율적인 학습을 할 수 있고 쓸모없는 parameter가 줄어드니까 overfitting을 방지할 수 있다.

##### 4. Loss function과 Optimizer 정의
![image](https://user-images.githubusercontent.com/34912004/102059036-b5d7b900-3e33-11eb-87ef-4bf46af055ef.png)

##### 5. Train
![image](https://user-images.githubusercontent.com/34912004/102059084-c4be6b80-3e33-11eb-9066-20be47a1b3f1.png)
![image](https://user-images.githubusercontent.com/34912004/102059108-c9831f80-3e33-11eb-98f5-d98c90659fd6.png)

* zero_grad를 하는 이유: Pytorch에서는 backpropragation 전에 gradient를 0으로 설정해야 한다. backward passes에서 gradient를 누적시키기 때문임. 0으로 설정하지 않으면 gradient는 최적과는 다른 방향을 가리킴.
* criterion(outputs, labels) : criterion 파라미터 값을 기준으로 예측과 정답값을 비교
* loss.backward(): 구한 loss로부터 back propagation을 통해 각 변수마다 loss에 대한 gradient를 구함.
* step()이란 함수를 실행시키면 우리가 미리 선언할 때 지정해 준 model의 파라미터들이 업데이트 된다.
  
##### 6. 학습 모델 저장 및 train set 확인
![image](https://user-images.githubusercontent.com/34912004/102059959-ec620380-3e34-11eb-808f-5bc9bc96f16a.png)

##### 7. 학습 모델 load 후 예측 결과 확인
![image](https://user-images.githubusercontent.com/34912004/102059471-47dfc180-3e34-11eb-989b-b745d2caada5.png)

##### 8. Test set에서의 Accuracy
![image](https://user-images.githubusercontent.com/34912004/102059623-7a89ba00-3e34-11eb-987a-928f34d26508.png)

##### 9. Class 별 accruacy 확인
![image](https://user-images.githubusercontent.com/34912004/102059710-99884c00-3e34-11eb-9cfe-dd8ab20e1bd4.png)

##### 10. 추가 작업(성능 향상)
* optimizer를 Adam으로 바꾸고 적절하게 amsgrad 설정하면 61%까지 향상함.