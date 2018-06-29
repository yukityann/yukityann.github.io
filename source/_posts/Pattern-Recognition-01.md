---
title: Pattern Recognition 01
date: 2017-12-20 19:14:04
tags:
	- Pattern Recognition
	- Note
categories: Pattern Recognition
---

模式识别系列

<!-- more -->

# 概念
模式：我们从事物获得的信息，往往表现为具有时间或空间分布的信息。

模式识别系统(统计模式识别)：
- 信息获取
- 预处理
- 特征提取和选择
- 分类决策

紧致集：同一类的哥哥模式在改空间组成一个紧致集，从这个紧致集中的任何一点可以均匀过渡到同一集中的另外一点，而在过度途中的所有个点仍然属于这个紧致集即属于同一个模式类。此外当紧致集中各点在任意方向有些不大的移动(相应于被观察对象有些微笑的变形)是它仍然属于这个集合。那么这样的集合被称为紧致集。

定理：只要各个模式类是可分的，总存在这样一个空间(特征空间)，使变换到这个空间的集合是满足紧致性要求的。

集合论表述：
>	对于一个集合M，定义关系R
	\\( \forall x \in M, xRx \\), 则称关系R是自反的。
	\\( \forall x,y \in M, xRy \Longrightarrow yRx \\), 则称关系R是对称的。
	\\( \forall x,y,z \in M, xRy,yRz \Longrightarrow xRz \\), 则称关系R是传递的。

同时满足自反和对称的关系成为相似关系
同时满足自反，对称和传递的关系成为等价关系
eg: 相等就是一种等价关系。

相似性度量——定义在空间中的某种距离：
&emsp;&emsp;给定一个输入样本集合\\(X\\), 用D维空间中的一个点表示某个样本，\\(x_i,x_j\\)支箭的相似性度量\\(\delta(x_i,x_j)\\)应满足一下要求：
1. 相似性度量应为非负值，即\\(\delta(x_i,x_j) \geq 0\\);
2. 样本本身之间相似性度量应为最大;
3. 相似性度量应满足对称性，即\\(\delta(x_i,x_j) = \delta(x_j,x_i)\\);
4. 在模式类满足紧致性条件下，相似性应是点间距离的单调函数。

分类是主观的，不存在纯客观的分类标准。

特征是决定相似性与分类的关键。
>考虑目测一把椅子的重量，想要从视网膜上椅子的图像直接计算其重量是极为困难的，但是我们可以把这个问题分解为若干层次的更简单的问题。例如，重量可以由比重和体积算出，比重可由器材质决定，材质可以从纹理、颜色等可知；体积可有形状算出，三维形状可有二位形状推测，二位形状可由边缘算出，边缘又可以从更为低层的特征——局部方向得到。

&emsp;&emsp;由此可知，为了通过学习获得新的特征，我们需要充分的低层次特征。如果低层次特征足够风度，通过选择和简单的运算就可以得到更高一层的新特征，新特征又可以称为更高层次特征的基础。

特征分为三个层次
1. 底层特征：直接地收到信息源物理特性的影响
2. 中层特征：信息源的变化中存在的某种不变性，建立稳定，可预测的客观世界图景，为高层特征做准备(常识背景)
3. 高层特征：具体化和专门化，服务于具体的目的与行为

# 贝叶斯决策理论
贝叶斯决策的基本要求
- 各类别总体的概率分布是已知的
- 要决策分类的类别数是一定的

要识别的对象有\\(d\\)种特征观察量\\(x_1,x_2,\cdots,x_d\\), 构成\\(d\\)维特征空间。有\\(c\\)个类别，各类别的状态用\\(\omega_i\\)来表示。分类的目的是根据特征向量得到样本\\(\boldsymbol{x}\\)到底属于哪一类。各个类别\\(\omega_i\\)出现的先验概率\\(P(\omega_i)\\)及类条件概率密度函数\\(p(\boldsymbol{x}|\omega_i)\\)是已知的（\\(p(\boldsymbol{x}|\omega_i)\\)表示在类别\\(\omega_i)\\)下观察到\\(\boldsymbol{x}\\)的类条件概率密度）。

## 几种常用的决策规则
### 基于最小错误率的贝叶斯决策
用后验概率代替先验概率(可迭代)，最后概率最大的就是决策结果
贝叶斯公式

{% math %}
\begin{align}
P(\omega_i|\boldsymbol{x})=\frac{p(\boldsymbol{x}|\omega_i)P(\omega_i)}{\sum\limits_{i=1}^{c}p(\boldsymbol{x}|\omega_j)P(\omega_j)}
\end{align}
{% endmath %}

得到的条件概率\\(P(\omega_i|\boldsymbol{x})\\)成为状态的后验概率。贝叶斯公式实质上是通过观察特征向量\\(\boldsymbol{x}\\)把状态的先验概率\\(P(\omega_i)\\)转化为后验概率\\(P(\omega_i|\boldsymbol{x})\\)
基于最小错误率的贝叶斯决策规则为：

{% math %}
\begin{align}
&P(\omega_i|\boldsymbol{x}) < \max P(\boldsymbol{\omega}|\boldsymbol{x}) & \Longrightarrow \boldsymbol{x}\in\omega_i \\
&p(\boldsymbol{x}|\omega_i)P(\omega_i) = \max p(\boldsymbol{x}|\boldsymbol{\omega})P(\omega) & \Longrightarrow \boldsymbol{x}\in\omega_i \\
&\begin{split}
P(e)=1-P(c)&=1-\sum\limits_{i=1}^cP(\boldsymbol{x}\in\boldsymbol{F}_i|\omega_i)P(\omega_i) \\
&=1-\sum\limits_{i=1}^c\int\limits_{\boldsymbol{F}_i}p(\boldsymbol{x}|\omega_i)P(\omega_i)dx
\end{split}
\end{align}
{% endmath %}

---------------
等待下次更新...
---------------