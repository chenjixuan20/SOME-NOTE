# Efficient Bidirectional Order Dependency Discovery

## 相关概念

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201016154835753.png" alt="image-20201016154835753" style="zoom:50%;" />

**祖先ODs**

> $AX^´ \mapsto BY^´$是$AX \mapsto BY$的祖先ODs，当且仅当
>
> 1. $X^´$是$X$的前缀，$Y^´$是$Y$的前缀或者$Y$
> 2. $Y^´$是$Y$的前缀，$X^´$是$Y$的前缀或者$X$
>
> 意思就是短的是祖先，因为长的是由短的根据剪枝规则产生的



## 模型框架

使用以下框架发现关系实例r上的完整的最小有效ODs集合$\sum$：

1. 抽样，得到子集$r^´ \subseteq r$；
2. 发现$r^´$上的完整的最小有效ODs集合$\sum ^´$；
3. 如果所有$\sum ^´$上的ODs在r上都有效，则$\sum = \sum ^´$，且是r上的完整的最小有效ODs集合。否则选择一个非空子集$\Delta r \subseteq r / r′$，使得$r^´=r^´ \bigcup \Delta r $，继续第二步操作来更新$r^´$

该框架会在有限次运行后结束，因为$r^´$的大小单调增加，最坏情况为$r^´=r$。

以下论点证明改框架的正确性：

论点1:(图片之后上传)







