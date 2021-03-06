# 实验计划
>注意：全部实验log使用统一的命名规则，如logNer.base.a1-2.a2-1.c3-2

实验出发点：
+ 字概率特征
+ LSTM网络
+ 局部聚焦
+ CRF层融合（决策层的修改）
+ 特征的引入时机与方法

## a# 基线模型调参实验
> 保持基本结构不变，调整超参,初步确认不同参数的影响力。  
实验基础模型为base。

+ a1# iteration=300
+ a2# lstm_layer=2、4、8
+ a3# dropout=0、0.2
+ a4# batchsize=32
+ a5# char_hidden_dim=100
+ a6# trans_hidden_dim=400
+ a7# hidden_dim=400
+ a8# learning_rate=0.03
+ a9# lr_decay=0.1
+ a10# l2=0
+ a11# clip=5(需要修改代码)

## b# 增加特征
> 扩展基线模型所使用的特征，初步确认不同特征对基线模型的影响力

+ b1# 更大英语trains文件
+ b2# 日语trains文件
+ b3# POS
+ b4# 分词位置
+ b5# 字形特征
+ b6# 多gram的idf概率

## c# 字概率信息
> 在字级别上引入不同特征的统计概率  
中文中是字，英文中是BPE字词

+ c1# 输入层硬拼接
+ c2# 输入层软拼接
+ c3# 中间层软拼接
+ c4# 决策层硬拼接
+ c5# 引入实体类型概率：统计方法、用词向量相似度学
+ c6# 引入POS概率
+ c7# 引入分词概率

## d# 局部聚焦
> 仅关注局部字符串而非全句

+ d1#方式=局部emb、LM判断
+ d2# 局部emb融入方式
+ d3# 各类型单独训练LM判断子串是否含实体
+ d4# 多类型交叉训练LM判断=2组合、全组合
+ d5#判断方式=ppl、分类器、emb拼接
+ d6# embedding拼接方式=加和、LSTM
+ d7# 评价标准=ppl、相似度、分类器
+ d8# 标签向量生成方式=加权平均、与多个分类器一起学
+ d9# 子串长度=1、2、3、4、5
+ d10# 多判断结果的综合方法
+ d11# 使用该特征的方法：输入层硬拼接、决策层硬拼接

## e# 多层lstm
> 动态链接的多层lstm，以实现任两词的直连

+ e1# 纵向的门控单元
+ e2# attention
+ e3# self-attention
+ e4# 离散结构的链接
+ e5# 多个不同起始位置水平层+纵向综合层
+ e6# 跨层残差连接

## f# 决策层
> 在决策层上的修改对结果的影响最为直接

+ f1# 取前n条分数较高的路径做Ens
+ f2# 记录多次CRF结果做Ens
+ f3# loss中加入对g与x有关的惩罚
+ f4# CRF中加入x做输入
+ f5# 取消B\I标签