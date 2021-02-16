# 

## 실험 protocol
|Protocol|Model|val_loss|특이사항|
|---|---|---|---|
|1|base_line(lstm)||lstm 1개사용|
|2|GRU4개+earlystopping|<span style="color:red">1.4644</span>|rmsprop사용, epoch22에서 stop|
|3|biGRU2개+earlystoppig|1.8694|units 모두 30으로 사용, epoch24에서 stop|
|4|
