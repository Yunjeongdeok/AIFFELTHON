## Faster R-CNN 선정이유

- 에이모의 과제 중, 모델 1개는 다른 팀(Task B)과 데이터셋만 다르고, 모델은 하이퍼 파라미터까지 같은 상태에서 실험을 하기를 원했다. Task B와 합의하에 Faster R-CNN을 하기로 했다.
- 비록 6년 정도된 모델이지만 Faster R-CNN 이후 R-CNN 계열은 형태는 비슷하고 backbone, neck 등의 개선이 이루어졌다.  그래서  backbone을 convNeXt 등의 최신모델로 바꾸면 성능 향상을 이룰 수 있다고 생각했다.

**모델링** 

1. MMdetection에는 Faster R-CNN이 구현되어 있다. 그래서 쓰고자 했다.
2. MMdetection은 아래와 같은 형식(format)으로 만들어야한다. 그래서 만들었다


<img width="596" alt="스크린샷 2022-12-12 오전 10 43 40" src="https://user-images.githubusercontent.com/66895161/206948206-d4cecdec-cee0-4b61-8cb0-ddb7efc914f2.png">

    
    

- 학습 과정  에이모 json ⇒ txt변환 ⇒ MMdetection  format변환 ⇒ train(학습)
    - json ⇒ txt변환
    
    
    - txt ⇒ MMdetection  format변환
        
        

- train(학습)
    
    

1.  학습을 진행했다.

## 실패 사례

1. 아래의 그림과 같이 학습을 실패를 했다.
    
<<<<<<< HEAD

=======
>>>>>>> d73a40f1e5d9d7b80abaa187b0138ad3c0326aaa
    

1. 원인 ⇒ img를 0.5로 resize했지만 label은 변환하지 않았다.
2. 해결 ⇒ label도 크기(숫자)를 0.5로 resize했다.
3. 아래의 사진은 학습결과가 아닌 cv2로 확인한 결과이다. img와 label이 잘 변환되었다.


## **학습 결과**

1. 하이퍼 파라미터는 SGD와 Adam으로 했고, lr을 변경하면서 실험을 했다.
2. 아래의 사진과 같이 학습이 잘 되었음을 알 수 있다.
    - 트래픽 사인(신호등 등)은 train 전에 제거했기에 detection하지 않는다.
        
        

1.  그림과 같이 큰 변화는 없었다. 사진도 비슷비슷했다.
- adam01
    - train 결과
    

    

- test 결과
    
    <img width="380" alt="test_평가지표con0 3" src="https://user-images.githubusercontent.com/66895161/206948723-4d0d3bf5-40b9-4c05-ab73-ea62927bbb88.png">

    

- SGD 01
    - train 결과
        

        

- test 결과
    

    

## **회고**

- Faster R-CNN의 장점
    - YOLOv7보다 높은 정확도
    - backbone 등을 개선하면 정확도 좀 더 향상 가능
    
- Faster R-CNN의 단점
    - 탐색 속도 느림
    - 지금처럼 사진들을 하는데에는 적합하나 cctv같이 실시간 동영상에 적용하기에는 무리이다.
    
- 개선시도
    - SGD에서 Adam으로 optimizer 변경을 했다.
    - lr 변경 기본 값에서 2배로 올리기
- 아쉬움
    - MMdetection을 처음 다뤄봐서 배우는데 시간이 너무 많이 걸렸다.
    -  6년 전 모델인데도 YOLOv7과 비등비등한 결과를 얻었다.
        - backbone을 ConvNeXt, Swin실험 등으로 바꿔서 성능향상을 이루지 못해 아쉬웠다.
        - Sparse R-CNN, Double-Head R-CNN 등 다른 최신 R-CNN 계열이라면 어떤 결과였을지 궁금하다.

- 앞으로의 다짐
    - MMdetection 운용이나 모델 쌓는 능력이 미숙했다. 계속 공부를 해서 다양한 모델을 사용하고 익숙해지도록 노력하겠다.
    - 나중에 MMdetection이 어느정도 익숙해지면 모델로 비교하면 하면서 특징을 알아보고 싶다
    - R-CNN 계열을 맡았지만 관련 논문을 많이 읽지 못했다. R-CNN부터 Dynamic R-CNN 등 여러가지 R-CNN 계열 논문을 읽고, github 등을 참고하여 논문 구현을 하고 싶다. 
