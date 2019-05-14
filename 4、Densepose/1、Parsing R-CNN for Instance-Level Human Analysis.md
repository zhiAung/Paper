
一、论文

Parsing R-CNN for Instance-Level Human Analysis
https://arxiv.org/abs/1811.12596


二、论文笔记

1、分析以前的工作的问题

实例级的人体分析还存在的问题

(1)、mask 分支给出的是一个类无关的mask，但是实例分析需要更多的细节特征

(2)、实例分析需要人体各个部分的几何和语义关系（mask没有）

2、创新性：

（1）、(PPS)使用特征金字塔的结构分别提取feature map：
bbox分支和自己定义的分支单独提取feature map，bbox 使用原有的FPN策略（p2-p5）,而自己的分支只使用p2的feature(尺寸更大)，因为作者分析了coco的数据集发现coco数据里边小的人物占比非常多。

（2）、增大ROI的分辨率：
之前的工作使用 7 *7 或者14 *14的ROIpooling ,对于物体检测或者物体分割这些不需要太多细节的任务可能适用，但是对于人体部位分割或者关键点定位这些需要细节的任务，则原来的分辨率损失了很多的信息。把自己定义的分支的ROI分辨率变为32 * 32，这样会增加计算复杂度，以及内存开销。减小了批处理大小来解决以上问题

（3）、提出了一个几何和上下文编码的模块
分支头使用较浅FCN效果不会（a）人体不同部位的比例差别很大（b）每个部分存在几何联系，这里使用non-local（高斯版本）结构可能会有益（c）32 * 32的ROI 需要更深的卷积进行消化。
ASPP在语义分割领域非常有效
基于以上两点，提出来了一个ASPP和non-local结合的模块Geometric and Context Encoding (GCE)，并且在后面加入了bn

（4）、纵向解耦，将自己提出来的分支，分为三个小分支（a）semantic space transformation（b）GCE（c）converts semantic features to specific tasks  三个分支的关系 before GCE, GCE module and after GCE

3、实验

在其他两个人体解析的数据集（CIHP、MHP v2.0）上的实验：

分别验证了每一个创新点的有效性。前面提到到措施，都有一定的提升，但是在COCO关键点数据集上预训练模型并且使用姿态估计的权重来初始化自定义的分支，这部分有很大的提升。增加训练的轮数也带来了一定的提升。

在DensePose-COCO上的实验：

计算指标没有改变，只是损失函数改成了fcn里边的像素级的softmax
获得第一名。。。


4、问题？

1、ASPP

2、像素级的softmax 见FCN 代替了原来的densepose的损失
