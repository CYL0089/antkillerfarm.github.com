---
layout: post
title:  深度学习（二十八）——SOM, Group Normalization, MobileNet, 花式卷积进阶
category: DL 
---

# RBM & DBN & Deep Autoencoder（续）

## DBN

RBM不仅可以单独使用，也可以堆叠起来形成Deep Belief Nets(DBNs)，其中每个RBM层都与其前后的层进行通信。单个层中的节点之间不会横向通信。

深度置信网络可以直接用于处理无监督学习中的未标记数据聚类问题，也可以在RBM层的堆叠结构最后加上一个Softmax层来构成分类器。

除了第一个和最后一个层，深度置信网络中的每一层都扮演着双重角色：既是前一层节点的隐藏层，也是后一层节点的输入（或“可见”）层。深度置信网络是由多个单层网络组成的。

深度置信网络常用于图像、视频序列和动作捕捉数据的识别、聚类与生成。

参考：

https://mp.weixin.qq.com/s/7V0GSWcKZGyXeLeMPhE9fQ

神经网络简史：BP算法后的又一突破—信念网络

https://zhuanlan.zhihu.com/p/40100223

深度学习与TensorFlow：关于DBN的一些认识

## Deep Autoencoder

![](/images/img2/deep_autoencoder.png)

Deep Autoencoder由两个对称的DBN组成，其中一个DBN通常有四到五个浅层，构成负责编码的部分，另一个四到五层的网络则是解码部分。

让我们用以下的示例来描绘一个编码器的大致结构：

784 (输入) ----> 1000 ----> 500 ----> 250 ----> 100 -----> 30

假设进入网络的输入是784个像素（MNIST数据集中28x28像素的图像），那么深度自动编码器的第一层应当有1000个参数，即相对较大。

这可能会显得有违常理，因为参数多于输入往往会导致神经网络过拟合。

在这个例子当中， 增加参数从某种意义上来看也就是增加输入本身的特征，而这将使经过自动编码的数据最终能被解码。

其原因在于每个层中用于变换的sigmoid置信单元的表示能力。sigmoid置信单元无法表示与实数数据等量的信息和差异，而补偿方法之一就是扩张第一个层。

各个层将分别有1000、500、250、100个节点，直至网络最终生成一个30个数值长的向量。这一30个数值的向量是深度自动编码器负责预定型的前半部分的最后一层，由一个普通的RBM生成，而不是一个通常会出现在深度置信网络末端的Softmax或逻辑回归分类输出层。

解码的DBN是一个完全相反的结构。

相比Autoencoder，Deep Autoencoder显然能够“消化”更复杂的数据。

## 参考

https://deeplearning4j.org/cn/restrictedboltzmannmachine

受限玻尔兹曼机基础教程

http://txshi-mt.com/2018/02/04/UTNN-11-Hopfield-Nets-and-Boltzmann-Machines/

Hopfield网和玻尔兹曼机

http://txshi-mt.com/2018/02/10/UTNN-12-Restricted-Boltzmann-Machines/

受限玻尔兹曼机

https://mp.weixin.qq.com/s/pWhJR6_6lRtDBl1MnYSIiw

深度学习来一波，受限玻尔兹曼机原理及在推荐系统中的应用 

http://www.cnblogs.com/kemaswill/p/3269138.html

基于受限玻尔兹曼机(RBM)的协同过滤

http://www.cnblogs.com/kemaswill/p/3203605.html

受限波尔兹曼机简介

https://mp.weixin.qq.com/s/MqnJ39GPrzP4xJWDZi9gnQ

博士生开源深度学习C++库DLL：快速构建卷积受限玻尔兹曼机

http://www.cs.toronto.edu/~fritz/absps/cogscibm.pdf

A Learning Algorithm for Boltzmann Machines

http://www.cs.toronto.edu/~hinton/absps/dbm.pdf

Deep Boltzmann Machines

http://www.cs.toronto.edu/~hinton/absps/guideTR.pdf

A Practical Guide to Training Restricted Boltzmann Machines

http://www.taodocs.com/p-62206829.html

《受限波尔兹曼机简介》张春霞著

https://mp.weixin.qq.com/s/mXJthyETebtww5TvEljuoQ

预测电影偏好？如何利用自编码器实现协同过滤方法

https://blog.csdn.net/hanlin_tan/article/details/62078935

机器学习中的玻尔兹曼分布——最小代价和极大似然

# SOM

Self Organizing Maps (SOM)（也叫kohonen network）是一种无监督算法，主要用于聚类和可视化。

>Teuvo Kohonen，1934年生，芬兰科学家。Helsinki University of Technology博士和教授。

![](/images/img2/SOM.png)

上图是一个SOM可视化的案例。左边的六角形网格被称作SOM的map space。其中，越相似的国家，在map space上的颜色和位置越接近。

下面来说一下SOM的具体训练方法。

## 构建map space

首先，map space是有拓扑关系的。这个拓扑关系需要我们确定，如果想要一维的模型，那么隐藏节点依次连成一条线；如果想要二维的拓扑关系，那么就组成一个平面网格。换句话说，SOM的目标就是**将任意维度的输入离散化到一维或者二维(更高维度的不常见)的离散空间上。**

网格的形状一般是矩形或六角形。网格中的每个node关联一个weight vector，其中的值表示一个input space中的点，因此weight vector和input vector的维度是相同的。

## 初始化weight vector

![](/images/img2/SOM_2.png)

上图展示了SOM的训练过程。SOM训练的目的就是使map space尽量贴合样本分布。

因此，通常有两种初始化weight vector的方法：

1.用小的随机值初始化。

2.从最大的两个主特征向量上进行采样。

显然，第2种训练起来要快的多。因为它已经是SOM weights的一个很好的近似了。

## 训练SOM

1.对于每一个输入数据，找到与它最相配的node，这个node一般被称作best matching unit(BMU)。这里一般使用Euclidean distance作为相似度的度量。

2.更新BMU的weight，使之更接近输入数据。同时也要更新BMU周围的node的weight，离BMU越近，更新幅度越大。更新公式为：

$$W_{v}(s + 1) = W_{v}(s) + \theta(u, v, s) \cdot \alpha(s) \cdot (D(t) - W_{v}(s))$$

其中，D(t)是输入数据，u表示BMU的index，v是被更新node的index，s是迭代的次数，$$\alpha(s)$$是更新率，一般为一个单调递减函数。$$\theta(u, v, s)$$是时空衰减因子，公式通常为：

$$\theta(u, v, s)=\exp \left(-\frac{dist(u,v)}{2\sigma^2(s)}\right)$$

反复多次进行以上2步的迭代之后，SOM会趋于收敛。

## 与K-Means的比较

同样是无监督的聚类方法，SOM与K-Means有什么不同呢？

1.K-Means需要事先定下类的个数，也就是K的值。SOM则不用，隐藏层中的某些节点可以没有任何输入数据属于它。所以，K-Means受初始化的影响要比较大。

2.K-means为每个输入数据找到一个最相似的类后，只更新这个类的参数。SOM则会更新临近的节点。所以K-mean受noise data的影响比较大，SOM的准确性可能会比k-means低（因为也更新了临近节点）。

3.SOM的可视化比较好。优雅的拓扑关系图。

![](/images/img2/SOMsPCA.png)

上图中蓝线表示PCA的主轴，而红点是SOM的node结点，可见SOM对数据的贴合程度要高于PCA。

## 参考

http://chem-eng.utoronto.ca/~datamining/Presentations/SOM.pdf

Self-Organizing Maps

http://www.cnblogs.com/sylvanas2012/p/5117056.html

Self Organizing Maps (SOM): 一种基于神经网络的聚类算法

http://blog.csdn.net/Loyal2M/article/details/11225987

聚类算法实践（3）——PCCA、SOM、Affinity Propagation

http://www.ai-junkie.com/ann/som/som1.html

Kohonen's Self Organizing Feature Maps

# ESN

Echo State Network

https://blog.csdn.net/zwqhehe/article/details/77025035

回声状态网络(ESN)原理详解

# Group Normalization

论文：

《Group Normalization》

![](/images/img2/Group_Normalization.png)

参考：

https://mp.weixin.qq.com/s/H2GmqloNumttFlaSArjgUg

FAIR何恺明等人提出组归一化：替代批归一化，不受批量大小限制

https://mp.weixin.qq.com/s/44RvXEYYc5lebsHs_ooswg

全面解读Group Normalization

# MobileNet

论文：

《MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications》

代码：

https://github.com/Zehaos/MobileNet

![](/images/article/dwl_pwl.png)

参考：

https://mp.weixin.qq.com/s/f3bmtbCY5BfA4v3movwLVg

向手机端神经网络进发：MobileNet压缩指南

https://mp.weixin.qq.com/s/mcK8M6pnHiZZRAkYVdaYGQ

MobileNet在手机端上的速度评测：iPhone 8 Plus竟不如iPhone 7 Plus

https://mp.weixin.qq.com/s/2XqBeq3N4mvu05S1Jo2UwA

CNN模型之MobileNet

https://mp.weixin.qq.com/s/fdgaDoYm2sfjqO2esv7jyA

Google论文解读：轻量化卷积神经网络MobileNetV2

https://mp.weixin.qq.com/s/7vFxmvRZuM2DqSYN7C88SA

谷歌发布MobileNetV2：可做语义分割的下一代移动端计算机视觉架构

https://mp.weixin.qq.com/s/lu0GHCpWCmogkmHRKnJ8zQ

浅析两代MobileNet

# 花式卷积进阶

## 可变形卷积核

MSRA于2017年提出了可变形卷积核的概念。

论文：

《Deformable Convolutional Networks》

![](/images/article/Deformable_convolution.png)

(a) 所示的正常卷积规律的采样 9 个点（绿点），(b)(c)(d) 为可变形卷积，在正常的采样坐标上加上一个位移量（蓝色箭头），其中(c)(d) 作为 (b) 的特殊情况，展示了可变形卷积可以作为尺度变换，比例变换和旋转变换的特殊情况。

![](/images/img2/Deformable_convolution.png)

如上图所示，位移量也成为了网络中待学习的参数。

参考：

https://mp.weixin.qq.com/s/okI3MT3E2o2PKCeokE7Niw

MSRA视觉组最新研究：可变形卷积网络

http://mp.weixin.qq.com/s/dvuX3Ih_DZrv0kgqFn8-lg

卷积神经网络结构变化——可变形卷积网络deformable convolutional networks

https://mp.weixin.qq.com/s/iN2LDAQ2ee-rQnlD3N1yaw

变形卷积核、可分离卷积？CNN中十大拍案叫绝的操作！

http://www.msra.cn/zh-cn/news/features/deformable-convolutional-networks-20170609

可变形卷积网络：计算机新“视”界

https://www.jianshu.com/p/940d21c79aa3

Deformable Convolution Networks

https://mp.weixin.qq.com/s/aLvlLi97JTd_cCfCZfraIg

“不正经”的卷积神经网络

## 3D卷积

3D卷积一般用于视频（2D图像+1D时序）和医学影像（3D立体图像）的分析处理中。

![](/images/article/conv_3d.png)

如上图所示，3D卷积的kernel不再是2D的，而是3D的。

论文：

《A two-stage 3D Unet framework for multi-class segmentation on full resolution image》

![](/images/img2/UNet-3D.jpg)

处理大型高分辨率3D数据时的一个常见问题是，由于计算设备的存储容量有限，输入深度CNN的体积必须进行裁剪（crop）或降采样（downsample）。这些操作会导致输入数据 batches 中分辨率的降低和类不平衡的增加，从而降低分割算法的性能。

受到图像超分辨率CNN（SRCNN）和self-normalization（SNN）的架构的启发，我们开发了一个两阶段修改的Unet框架，它可以同时学习检测整个体积内的ROI并对体素进行分类而不会丢失原始图像解析度。对各种多模式音量的实验表明，当用简单加权的模子系数和我们定制的学习程序进行训练时，该框架显示比具有高级相似性度量标准的最先进的深CNN更好的分割性能。

参考：

https://zhuanlan.zhihu.com/p/21325913

3D卷积神经网络Note01

https://zhuanlan.zhihu.com/p/21331911

3D卷积神经网络Note02

https://zhuanlan.zhihu.com/p/31841353

3D CNN阅读笔记

