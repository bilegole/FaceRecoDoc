首先,Yolo模型于2016年被提出,
DOI为:[10.1109 / CVPR.2016.91](https://doi.org/10.1109/CVPR.2016.91)

首先,要描述和计算损失函数,我们就要知道,被用于计算损失函数的两个输入值,即targets和outputs.

##### targets:
从题目本身来讲,targets应该是一个框,将物体圈在里面,包括框的坐标,框的长宽,方框中物体的种类.

但出于和yolo的输出比较,用于计算loss的角度来讲,我们将其转化为了以下几个参数
一个及以上的框,被用于描述图中需要定位的物体.主要有以下几个参数:
1. 整个方框中心点所在格子
2. 方框中心点在格子中的位置暗含了x和y
3. 高度h和宽度w
4. 物体的种类

##### output
即Yolo网络的输出.这个是非常明确的.
首先,在初始化网络的时候,我们就将图片分割为SxS个方块,同时每个方块提出两个预测框,每个框包含5个数据,即(中心xy,长宽wh,以及框选置信度),同时包含对C个种类的预测,每个预测给一个种类置信度.

即,我们最终的输出参数数量为:SxS(Bx5+C)
以论文中的举例为参考,他设:
- S=7	将图片划分为了7x7个网格,
- B=2	每个网格需要提出2个框选
- C=20 共需识别20中物品.

那么最终,他的输出为:$7\times7\times(2*5+20)=1470$

---
最终的loss也分为三个部分:
- location预测误差
$$\displaystyle\lambda_{coord}\sum_{i=0}^{S^2}\sum_{j=0}^{B}1_{ij}^{obj}\left[(x_{i}-\hat{x_i})^2+(y_i-\hat{y_i})^2+(\sqrt{w_i}-\sqrt{\hat{w_i}})^2+(\sqrt{h_i}+\sqrt{\hat{h_i}})^2\right]$$
- 信息框预测误差
$$\displaystyle\sum_{i=0}^{S^2}\sum_{j=0}^B1_{ij}^{obj}(C_i-\hat{C_i})^2+\lambda_{noobj}\sum_{i=0}^{S^2}\sum_{j=0}^B1_{ij}^{noobj}(C_i-\hat{C_i})^2$$
	这个公式中,前面一项是含有object的方块,其中$c_i$为1.
	后一项为不含有object的方块,其中$c_i$为0.
- 种类预测误差
$$\displaystyle\sum_{i=0}^{S^2}1_{i}^{obj}\sum_{C\in classes}(p_i(c)-\hat{p_i}(c))$$

---
特定字段解释:
- IoU:
IoU就是,Intersection over Union,即交集与并集之比.在此处被用于衡量预测框的准确程度.
- best_n:
在yolo模型中,我们预先提出了若干个预选框,其中只有一个是真正被考虑的,而其他的仅仅是陪跑而已(理论上有机会转正,但仅是理论上.)anchors有单独的一个维度,通常是第二维.best_n就是用于指出,在第二维中,哪个才是真正有价值的预测框,也就是"best"的含义.
- output的形状:
此处的output指的是,\[样本数量,网格横坐标,网格纵坐标,预测框,各项数据输出\]
- targets的形状:
在torch的dataset,VOC数据集的target的形式是:
{
	'object_1':{"xmin","ymin","xmax","ymax","class",},
	'object_2':{"xmin","ymin","xmax","ymax","class"},
	....
}
我们传参的时候显然不能这样传,或者说这样传会很麻烦.按照示例项目,传进去的是一个二维矩阵,具体形状如下:
\[对应的图片的id,object的标签,x,y,w,h\]
\[对应的图片的id,object的标签,x,y,w,h\]
\[对应的图片的id,object的标签,x,y,w,h\]
\[对应的图片的id,object的标签,x,y,w,h\]

