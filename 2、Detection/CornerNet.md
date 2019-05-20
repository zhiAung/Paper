# 一、论文

CornerNet: Detecting Objects as Paired Keypoints

论文：https://arxiv.org/abs/1808.01244

代码： https://github.com/princeton-vl/CornerNet

# 二、笔记
## 1、背景
a)使用anchors的one stage 方法有两个缺点：(1)大量的anchor box 降低了训练速度，并且存在正负样本不均衡的问题。(2)、另外使用anchor boxes会引入大量的超参数和设计选项

## 2、创新点
a）、预测一对角点来表示目标框（anchors free）

b）、设计了一个corner pooling layer

c）、修改更新了hourglass 结构，增加了新的focal loss 的变种

先用一个7*7的卷积 stride 2的步长降低四倍原图的分辨率（这好像是使用大尺度输入图片的通用思路，使用大size图片吗，然后在第一个卷进处，大幅度降低分辨率，即提高了图片的输入尺度，又缩小了运算量）
做了对比实验表明hourglass 作为corner net的backbone 相比使用 fpn 有很大的提升表明，基础网络对目标检测的重要性。


更多详细的其他内容可以参见这两篇博客，以及原论文。

https://blog.csdn.net/u014380165/article/details/83032273

https://blog.csdn.net/weixin_40414267/article/details/82379793
