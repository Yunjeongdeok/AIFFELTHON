## Faster R-CNN 선정이유

- 에이모의 과제 중, 모델 1개는 다른 팀(Task B)과 데이터셋만 다르고, 모델은 하이퍼 파라미터까지 같은 상태에서 실험을 하기를 원했다. Task B와 합의하에 Faster R-CNN을 하기로 했다.
- 비록 6년 정도된 모델이지만 Faster R-CNN 이후 R-CNN 계열은 형태는 비슷하고 backbone, neck 등의 개선이 이루어졌다.  그래서  backbone을 convNeXt 등의 최신모델로 바꾸면 성능 향상을 이룰 수 있다고 생각했다.

**모델링** 

1. MMdetection에는 Faster R-CNN이 구현되어 있다. 그래서 쓰고자 했다.
2. MMdetection은 아래와 같은 형식(format)으로 만들어야한다. 그래서 만들었다


<img width="596" alt="스크린샷 2022-12-12 오전 10 43 40" src="https://user-images.githubusercontent.com/66895161/206948206-d4cecdec-cee0-4b61-8cb0-ddb7efc914f2.png">

    
    

- 학습 과정  에이모 json ⇒ txt변환 ⇒ MMdetection  format변환 ⇒ train(학습)
    - json ⇒ txt변환
    
<img width="1151" alt="스크린샷 2022-12-12 오전 11 15 41" src="https://user-images.githubusercontent.com/66895161/206948277-10721649-5ea8-4ca4-b79b-623200949b2e.png">
    
    - txt ⇒ MMdetection  format변환
        
<img width="510" alt="스크린샷 2022-12-12 오전 11 17 32" src="https://user-images.githubusercontent.com/66895161/206948307-53de695b-ceb1-4ee6-95e6-3c0269c8ecac.png">
        

- train(학습)
    
<img width="307" alt="스크린샷 2022-12-12 오전 11 18 36" src="https://user-images.githubusercontent.com/66895161/206948381-852c16ce-e6a8-4a6e-be80-ef84d16b08a7.png">
    

1.  학습을 진행했다.

## 실패 사례

1. 아래의 그림과 같이 학습을 실패를 했다.
    
![Untitled](https://user-images.githubusercontent.com/66895161/206948474-bf406c85-dfde-46d0-8a01-17a410b2c437.png)
    

1. 원인 ⇒ img를 0.5로 resize했지만 label은 변환하지 않았다.
2. 해결 ⇒ label도 크기(숫자)를 0.5로 resize했다.
3. 아래의 사진은 학습결과가 아닌 cv2로 확인한 결과이다. img와 label이 잘 변환되었다.

![Untitled](https://user-images.githubusercontent.com/66895161/206948645-08376c39-7d2b-46f9-bde3-95c0c80c0d69.png)

## **학습 결과**

1. 하이퍼 파라미터는 SGD와 Adam으로 했고, lr을 변경하면서 실험을 했다.
2. 아래의 사진과 같이 학습이 잘 되었음을 알 수 있다.
    - 트래픽 사인(신호등 등)은 train 전에 제거했기에 detection하지 않는다.
        
![Untitled](https://user-images.githubusercontent.com/66895161/206948674-5439c5cb-ff1d-47bc-8490-783858ed06ff.png)
        

1.  그림과 같이 큰 변화는 없었다. 사진도 비슷비슷했다.
- adam01
    - train 결과
    
    <img width="212" alt="스크린샷 2022-12-08 오전 11 04 39" src="https://user-images.githubusercontent.com/66895161/206948718-3117fbcd-9469-4106-8592-e10b717976e3.png">

    

- test 결과
    
    <img width="380" alt="test_평가지표con0 3" src="https://user-images.githubusercontent.com/66895161/206948723-4d0d3bf5-40b9-4c05-ab73-ea62927bbb88.png">

    

- SGD 01
    - train 결과
        
        <img width="211" alt="스크린샷 2022-12-10 오후 6 36 27" src="https://user-images.githubusercontent.com/66895161/206948782-732edb02-94fc-4b6f-ad19-1b2bcb72fde4.png">

        

- test 결과
    
    <img width="387" alt="test_평가지표_con0 3" src="https://user-images.githubusercontent.com/66895161/206948786-0150d394-ef22-4b59-acab-f1de997d4b63.png">

    

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
    - backbone을 ConvNeXt, Swin실험 등으로 바꿔서 하고 싶었으나 못했다.
    - Sparse R-CNN, Double-Head R-CNN 등 다른 R-CNN 계열을 하지 못했다.
