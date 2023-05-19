# CowDetect - yolov7과 deepsort기반 젖소 객체 추적 프로젝트 (2022 캡스톤 디자인 은상 수상)

영상 정보만 가지고 젖소의 활동량을 계산해 가임기와 발정기를 파악하고 예측하는 AI 모델

<br/>


## 수행 과정

<img src="https://github.com/Ji-Yeon-98/CowDetect/blob/master/img/%EA%B2%B0%EA%B3%BC%ED%91%9C.png">

<br/>

### 1. 데이터 전처리

<img src="https://github.com/Ji-Yeon-98/CowDetect/blob/master/img/%EB%9D%BC%EB%B2%A8%EB%A7%81.png">

- 저작권이 없는 젖소 이미지와 목장 이미지 데이터를 수집
- 로보 플로우를 이용하여 직접 데이터 라벨링
- 데이터 전처리(대비 스트레칭, HSV 변환, 회전, 반전 등)를 이용해 데이터 증강 

<br/>

### 2. 훈련 방법

- yolo에서 제공하는 사전학습된 모델 가중치를 프리징하여 일부 레이어만 훈련
  - 마지막 레이어 제외한 모든 레이어 프리징 후 훈련
  - 모델의 헤드만 프리징하여 훈련

- 모든 레이어 훈련


<br/>

------

## install

```
git clone https://github.com/Jang-jiwon/CowDetect.git
cd CowDetect
git submodule update --init --recursive
cd StrongSORT-YOLO
pip install -r requirements.txt
wget https://www.dropbox.com/s/x4x6fr88lbgb504/sample_video.mp4
wget https://github.com/WongKinYiu/yolov7/releases/download/v0.1/yolov7-e6e_training.pt -O /content/CowDetect/StrongSORT-YOLO/yolov7/yolov7-e6e_training.pt
```

<br/>

## train

1. 프리징하여 일부 레이어만 훈련하는 경우
```
$python train.py --workers 8 --device 0 --freeze 105 --batch-size 32 --data ../your_path/data.yaml --img 416 416 --cfg cfg/training/yolov7.yaml --weights 'yolov7_training.pt' --name yolov7-custom --hyp data/hyp.scratch.custom.yaml
```

2. 모든 레이어로 훈련하는 

```
$python train.py --workers 8 --device 0 --batch-size 32 --data ../your_path/data.yaml --img 416 416 --weights 'yolov7_training.pt' --name yolov7-custom 
```

<br/>

## tracking

```
$python track_v7.py --source sample_video.mp4 --save-vid --class 19 --yolo-weights ../your_path/yolov7-e6e_training.pt
```
