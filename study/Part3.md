# 1. Convolution

![image](https://user-images.githubusercontent.com/34912004/126892600-815b2f64-7d9e-48ac-bb90-9b08aafb0fbc.png)

- stride : filter를 한 번에 얼마나 이동할 건지
- padding : input 주위를 0으로 둘러싸는 것 (padding = 2 : 0으로 2번 둘러싸는 것)
- padding 하는 이유
    - Convolution filter를 통과하면 이미지가 작아지지만 padding을 이용하면 그대로 유지 가능
    - Edge 부분의 정보가 중요할 경우, padding 하면 그 정보를 활용할 수 있음
- dilation : convolution kernel 간의 간격, 넓은 시야가 필요하고 여러 convolution이나 큰 커널을 사용할 여유가 없는 경우 사용
![image](https://user-images.githubusercontent.com/34912004/126892702-83f411d8-3beb-4b15-bf6f-0931fb01fc45.png)

- 참고
https://github.com/vdumoulin/conv_arithmetic/blob/master/README.md
- Output 계산   
![image](https://user-images.githubusercontent.com/34912004/126892714-3d648da6-c508-4f7b-a40e-5a7eb02d6c52.png)

- Pooling : 특징을 뽑아내는 과정   
![image](https://user-images.githubusercontent.com/34912004/126892720-a215bf04-735f-4dd4-8ae8-fc1a4d77355f.png)

- 보통 Max pooling을 더 많이 함.
- pooling 목적
    - input size를 줄임
        - 필요하지 않은 자료까지 전부를 다 분석할 필요없이 특징만 뽑아내서 학습함
    - overfitting을 조절
        - input size가 줄어드는 것은 그만큼 쓸데없는 parameter의 수가 줄어드는 것이라고 생각할 수 있음
    - 특징을 잘 뽑아냄.
        - 특정한 모양을 더 잘 인식할 수 있음
# 2. VGGNet
![image](https://user-images.githubusercontent.com/34912004/126892728-5fdb4715-89c4-4301-9518-72cda96a07f0.png)   
가. ILSVRC 2014에 등장 (ImageNet Large Scale Visual Recognition Challenge)    
나. 강조 키워드   
- 3x3 filter를 통해 layer 개수를 deep하게 늘렸고 이것이 large-scale image recognition에서도 좋은 결과를 얻었다.

다. 구조   
![image](https://user-images.githubusercontent.com/34912004/126892741-9623c64c-5ff7-4cd2-a61b-ee7ea17a945f.png)
- input image는 224x224로 고정
- train set에 대한 input image preprocessing에서 mean RGB value를 빼준다.
- 모든 layer에 3x3 conv filter (모델에 따라 1x1 filter가 추가된 것도 있음)
- Pooling layer는 5개이며, 2x2이고 stride = 2
- FC layer는 4096 channel
- hidden layer는 ReLU
- D와 E가 각각 VGG16, VGG19

라. Discussion
- 3x3 filter를 사용한 이유
    - receptive field
        - 특정 영역에 치중된 이미지 파악   
            ![image](https://user-images.githubusercontent.com/34912004/126892751-749add53-c80e-4459-a143-fdfdfd513db2.png)

    - parameter
        - 연산시 3x3 filter 2개가 7x7 filter 1개보다 parameter수가 적다.
            -> overfitting을 줄여준다.

마. Training
- hyperparameters
- cost function = Crossentropy
- batch size = 256
- optimizer = Momentum 0.9
- Regularization = L2
    - weight initialization
        - VGG A 먼저 학습 후, B~E 구성할 때 A의 layer를 가져다 씀(conv layer 4개, FC layer 3개) 
    - image size
        - image size를 VGG의 input size에 맞게 rescale
        - 만약 S = 256이라면, image size의 넓이와 높이 중 더 작은 쪽을 256으로 맞춰주고 ratio를 유지해야하기 때문에 다른 한 쪽도 비율에 맞게 rescale
        ex) 1024x512 => 512x256
        - S값 설정 방법
            - S를 256 or 384 고정
            - S가 256~512 중 random한 값을 갖도록 설정

바. Test
- Test에서 recaling이 필요 
    - training에서의 rescaling 기준값은 S, test에서는 Q
    - Q=S일 필요는 없고, 각 S마다 다른 Q값을 적용하면 performance가 향상된다고 함.
    - 첫번째 FC layer를 7x7로 바꾸고 두번째, 세번째 FC layer는 1x1로 변경
        - FC layer를 바꿔줌으로써 전체 이미지에 적용시킬 수 있다고 설명


# 3. ResNet
가. 사전 연구
- 딥러닝은 기본적으로 망이 깊어질수록 성능이 더 좋아진다고 생각을 함.
- VGG 이전의 layer들은 8이었고 VGG가 19까지 쌓았지만 결과적으로 16과 19의 성능 차는 크지 않음.
![image](https://user-images.githubusercontent.com/34912004/126892757-986b85e8-fcfa-4d49-94ab-25a686e58f11.png)
      
- ResNet 연구진이 20 layer와 56 layer 비교를 통해 망이 깊다고 다 좋은 게 아닌 것을 확인 (geadient descent)   

![image](https://user-images.githubusercontent.com/34912004/126892786-0638c247-35f7-4295-ada4-d997317001b1.png)

나. 구조   
- 기존 neural network   
![image](https://user-images.githubusercontent.com/34912004/126892800-44dae549-8b6e-4a1a-805e-e098f135cd54.png)
    - input x를 target y에 mapping시키는 함수 H(x)를 찾는 것
    - 즉, 출력이 x가 되도록 H(x) - x = F(x)를 최소화하는 방향으로 학습 진행
    
- skip / short connection   
![image](https://user-images.githubusercontent.com/34912004/126892805-4506537a-1401-45c1-a513-1153db57baad.png)
    - H(x) = x가 되도록 학습
    - F(x)는 0이 되도록 학습
    - F(x) + x = H(x) = x이 되도록 학습시킨다면 F(x) + x의 미분값은 F'(x) + 1로 최소 1 이상
    - 모든 layer에서의 gradient가 1 이상이므로 gradient vanishing 해결
![image](https://user-images.githubusercontent.com/34912004/126892816-bd7845ec-4a98-47f9-a5d0-34a7ad6fff21.png)
- 기본 구조는 VGG 19 기반이고 conv layer를 추가한 후 short connection을 추가한 것

다. 실험
- 저자는 layer 18과 34에서의 plain network와 ResNet의 성능 비교
![](2021-07-25-01-30-44.png)
     - 왼쪽은 34 layer가 더 error가 높지만 오른쪽은 더 낮은 것을 확인
     
![image](https://user-images.githubusercontent.com/34912004/126892831-85185e36-c825-4316-9496-58bcb502e25a.png)
- ImageNet validation testing에서 layer가 더 깊을수록 error가 더 낮은 것을 확인
