# 

## 실험 protocol
|Protocol|Model|val_loss|특이사항|
|---|---|---|---|
|1|base_line(lstm)||lstm 1개사용|
|2|GRU4개+earlystopping|<span style="color:red">1.4644</span>|rmsprop사용, epoch22에서 stop|
|3|biGRU2개+earlystopping|1.8694|units 모두 30으로 사용, epoch24에서 stop|
|4|biGRU2개+earlystopping|1.7874|units32->64,epoch25에서 stop| 
|5|biGRU4개+earlystopping|1.7746|epoch17에서 stop|

- 일반GRU가 biGRU보다 성능우수
- unit개수는 일정한것 보다 확장되는것이 성능이 더 우수함.
