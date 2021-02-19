# 데이콘 운동동작분류AI 경진대회
3축 가속도계(accelerometer)와 3축 자이로스코프(gyroscope)를 활용해 측정된 센서 데이터에 머신러닝 알고리즘을 적용해 운동 동작 인식 알고리즘 개발
## Data
### train
- train_x
  - ID : 사람 별 부여되는 ID (train - 총 3125개)
  - time : ID 1개당 600time의 data가 있음.
  - action_data : 행동데이터로 (acc_x, acc_y, acc_z, gy_x, gy_y, gy_z)로 구성됨.
  - id 3125 * time 600 = 총 1875000개의 데이터
- train_y
  - labels : ID하나당 label 1개 , 즉 600time의 동작data를 보아 이 사람은 이 운동을 하는 중.
### test
- 어떤 사람의(ID의) 600time 동작데이터를 보고 어떤운동(label)을 하는지 예측하는 문제.
- ID(782)별 600time의 동작(4)데이터
- id 782 * time 600 = 총 469200개의 데이터

## 실험 protocol
- defalut
  - batch_size = 32
  - epochs = 100
  - earlystopping = 10
  
|Protocol|Model|val_loss|특이사항|
|---|---|---|---|
|1|base_line(lstm)||lstm 1개사용|
|2|GRU4개+earlystopping|<span style="color:red">1.4644</span>|rmsprop사용, epoch22에서 stop|
|3|biGRU2개+earlystopping|1.8694|units 모두 30으로 사용, epoch24에서 stop|
|4|biGRU2개+earlystopping|1.7874|units32->64,epoch25에서 stop| 
|5|biGRU4개+earlystopping|1.7746|epoch17에서 stop|
|6|cnn-lstm|1.3957|filter=128, maxpooling, spatialDropout=0.4,LSTM unit32|
|7|cnn-lstm|1.4566|filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, epoch 1000으로 늘림|
|8|cnn2-lstm|1.5370 |filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, cnn 1층 늘림|
|9|cnn2-lstm|1.3853 |filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, cnn 1층 늘림, epoch1000|
|10|cnn2-lstm|1.3614 |filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, cnn 1층 늘림, epoch1000, adam -> rmsprop|
|11|cnn2-lstm|1.4994 |filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, cnn 1층 늘림, epoch1000, adam -> rmsprop, batch=16|
|12|cnn2-Bilstm|1.0629 |filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, cnn 1층 늘림, epoch1000, adam -> rmsprop|
|13|cnn2-Bilstm2|0.9169 |filter=128, maxpooling, spatialDropout=0.4,LSTM unit32, epoch1000, adam -> rmsprop|
|14|cnn2-Bilstm2|0.8615 |filter=128, maxpooling,LSTM unit32,64, epoch1000, adam -> rmsprop, Dropout뺌|
|15|aug2_cnn2-Bilstm3|**0.1643** |filter=128, maxpooling,LSTM unit32,64,128 epoch1000, adam -> rmsprop, Dropout뺌, **Data증강**|
|16|**aug1_cnn2-Bilstm3**|0.8511|filter=128, maxpooling,LSTM unit32,64,128 epoch1000, adam -> rmsprop, Dropout뺌, **Data증강**|
|17|aug1_cnn2-Bilstm3||filter=128, maxpooling,LSTM unit32,64,128 epoch1000, adam -> rmsprop, Dropout뺌, **Data증강**, DROPOUT추가|

- 일반GRU가 biGRU보다 성능우수
- unit개수는 일정한것 보다 확장되는것이 성능이 더 우수함.
- 배치는 16보다 32가 더 우수
- optimizer = adam? rmsprop?
- dropout ?
- batch_norm ?
- [15] : 데이터 증강한게 훨씬 성능 잘나오나 test낮음 , test log loss는 1.02
- 시작부터 train과 val 분리 후 데이터 증강 및 학습 


## Model
### cnn-lstm
- cnn-lstm의 구조\
![image](https://user-images.githubusercontent.com/70633080/108026447-b414b680-706b-11eb-9e99-d9612c719fb5.png)
