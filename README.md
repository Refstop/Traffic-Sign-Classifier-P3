# Traffic-Sign-Classifier-P3
본 프로젝트는 Udacity Self-driving car nanodegree의 세 번째 프로젝트 교통 신호 분류기입니다.
프로젝트 목적은 다음과 같습니다.
- Train data, Validation data, Test data를 받아 학습, 유효 검증, 최종 테스트 정확도 보기
- CNN을 수정하여 93% 이상의 유효 검증 정확도 도출하기
- 계산된 모델로 새로운 사진 test하고 분류 결과와 정확도 검출

## 기본이 된 CNN 모델
![lenet](image/lenet.png)  
교통신호 분류기를 작성하는데 사용한 CNN은 LeNet-5 입니다. 기존의 LeNet-5의 구조는 그림과 같이 Convolution-Pooling layer 한 쌍과 3개의 Fully Connected Layer로 구성되어 있습니다.

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Input         		| 32x32x3 RGB image   							|
| Convolution 1     	| 1x1 stride, VALID padding, output = 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride, VALID padding, output = 14x14x6   |
| Convolution 2  	    | 1x1 stride, VALID padding, output = 10x10x16  |
| RELU					|												|
| Max pooling	      	| 2x2 stride, VALID padding, output = 5x5x16    |
| Flatten				| output = 400									|
| Fully connected		| input = 400, output = 120       	            |
| RELU					|												|
| Fully connected		| input = 120, output = 84       	            |
| RELU					|												|
| Fully connected		| input = 84, output = 10       	            |

하지만 이 모델을 그대로 사용했을 때, 유효 검증 정확도가 약 89% 정도 도출되었기에, 조금 수정해 주었습니다.

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Input         		| 32x32x3 RGB image   							|
| Convolution 1     	| 1x1 stride, VALID padding, output = 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride, VALID padding, output = 14x14x6   |
| Convolution 2     	| 1x1 filter, 1x1 stride, VALID padding, output = 14x14x16 	|
| RELU					|												|
| Convolution 3  	    | 1x1 stride, VALID padding, output = 10x10x48  |
| RELU					|												|
| Max pooling	      	| 2x2 stride, VALID padding, output = 5x5x48    |
| Flatten				| output = 1728									|
| Fully connected		| input = 1728, output = 256       	            |
| RELU					|												|
| Fully connected		| input = 256, output = 128       	            |
| RELU					|												|
| Fully connected		| input = 128, output = 84       	            |
| RELU					|												|
| Fully connected		| input = 84, output = 43       	            |

정확도를 올리기 위해서 사용해 본 방법은 다음과 같습니다.
1. Hyperparameter 수정
1. 1*1 convolution layer 추가
1. fully connected layer 추가
1. dropout 추가
1. 이미지 정규화 범위 수정
1. 가중치 초기화 다른 방법 사용

이 중 결론적으로 사용한 방법은 1, 2, 3번 입니다. EPOCHS를 10~200까지 증가시켜 보면서 가장 유효 검증 정확도가 높게 나온 EPOCHS = 50을 채택했습니다. 그리고 계산량을 줄이고 비선형성을 추가하는 1*1 convolution layer를 추가하였고, fully connected layer를 더 깊게 하여 비선형성을 추가했습니다.
```
여러번 실행해 본 결과 EPOCHS = 50일 때 가장 좋은 결과를 낼 수 있었습니다.
EPOCHS = 50  
BATCH_SIZE = 128  

1번째 실행
EPOCH 48 ...  
Validation Accuracy = 0.9566893424036281
EPOCH 49 ...  
Validation Accuracy = 0.9564625850340136
EPOCH 50 ...  
Validation Accuracy = 0.9562358276643991
2번째 실행
EPOCH 48 ...  
Validation Accuracy = 0.9578231290084164
EPOCH 49 ...  
Validation Accuracy = 0.9578231290084164
EPOCH 50 ...  
Validation Accuracy = 0.9578231290084164
```

## Test 데이터 정확도 측정
```
Test Accuracy = 94%
```
Test 데이터의 정확도는 약 94%가 나왔습니다. 다음으로 새로운 데이터에 대한 정확도를 보겠습니다.

## 새로운 데이터에 대한 정확도
```
INFO:tensorflow:Restoring parameters from ./traffic_classi
Prediction: 17
Prediction: 13
Prediction: 17
Prediction: 17
Prediction: 14
Prediction: 13
Prediction: 38
Test Accuracy = 86%
```
새로운 데이터에 대해서 약 86%의 정확도를 보여주고 있습니다. 7개 중 6개를 정답으로 예측하고 있습니다.
