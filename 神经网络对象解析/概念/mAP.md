mAP,即mean Average Precision,平均精度的均值.指的是,对于要预测的每个类计算出ROC回归曲线导出的均值,计算这个均值的平均值.

#### TP,FP,FN
- TP:即一个正确的检测,得到的IOU\>threshold.
- FP:即一个错误的检测,得到的IOU\<threshold
- FN,即target种物体存在,但并未被检测出来,一般认为FN的数量等于???

- Pricision:准确率/精确率,准确率模型指的是模型只找到相关目标的能力,等于$$Pre = \frac{TP}{TP+FP}$$即模型给出的所有预测结果中命中真实目标的比例.
	
- Recall:召回率是指模型找到所有相关目标的能力,等于:$$Recall = \frac{TP}{TP+FN}$$即模型给出的预测结果最多能覆盖多少真实目标.

- score,confidence:每个预测边界框的分数/置信度,不同的论文种表达方式不同.

- RP曲线:即precision-recall曲线,

#### ap的两种计算方法.
VOC是物体检测的重要项目,似乎在07和10年采用不同的map计算方法.

###### 07年
07年,map采用11插值法.