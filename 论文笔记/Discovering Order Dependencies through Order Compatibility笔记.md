# Discover Order Dependencies through Order Compatibility笔记

## 重要公理

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927162558016.png" alt="image-20200927162558016" style="zoom:50%;" />

~~公理1～公理4会证明~~

<font color=#FF0000> ***公理5～公理6还不会*** </font>

## 重要定义

$p_A$：元组p在属性A上的投影

投影：从原有关系生产一个新的关系，包含原来关系的部分列

### 定义2.1 操作符号

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927162707854.png" alt="image-20200927162707854" style="zoom:50%;" />

该操作符与将数据按字典序排序效果一样，即将$X$按字典序进行排序

### 定义2.2 顺序依赖(OD)

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927174300921.png" alt="image-20200927174300921" style="zoom:50%;" />

即对于关系模式R中的每一个r，在对X属性列进行字典序排序的同时也会将Y属性列进行字典序排序。记为：
$$
X\mapsto Y
$$

### 定义2.3 函数依赖(FD)

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927174726713.png" alt="image-20200927174726713" style="zoom:50%;" />

即每一个X属性列的值相等时，Y属性列的值也相等。

表现为函数关系：一个X对应一个Y。

### ***定义2.4 函数一致性依赖(OCD)***

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927175033006.png" alt="image-20200927175033006" style="zoom:50%;" />

即对XY(YX)属于列进行字典序排序的同时也会对YX(XY)属性列进行字典序排序。

表达式为：$XY\mapsto YX \and YX\mapsto XY$，也记作$X～ Y$

***查看两列之间的OCD的另一种方法是，当将它们视为一对时，它们的值都是单调非递减的。***

***OCD编码为这样的事实：在一个关系中，两个属性列表显示相同的单调性。如果我们以非递减顺序对列表的任何一个进行排序，则它们最后都变为两个顺序排序，因此它们的值都是单调非递减的。***

### 定义2.5 Split											<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927191556369.png" alt="image-20200927191556369" style="zoom:50%;" />		

即对于(X,Y)这个属性列，两个元组"前等后不等"。

### 定义2.8 Swap

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927191430856.png" alt="image-20200927191430856" style="zoom:50%;" />

即对于(X,Y)这个属性列，两个元组"前小后大"。

### 定义3.1 闭包(Clouse)

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927191956459.png" alt="image-20200927191956459" style="zoom:50%;" />

一个ODs集合的闭包是该集合通过上述公理可以推导出来的所有ODs的集合。

### 定义3.2 等价ODs集合

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927192218574.png" alt="image-20200927192218574" style="zoom:50%;" />

当且仅当两个ODs集合的闭包相等时，这两个ODs集合等价

### 定义3.3 最小属性列

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927192413678.png" alt="image-20200927192413678" style="zoom:50%;" />

### 定义3.4 最小OCD

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927192529796.png" alt="image-20200927192529796" style="zoom:50%;" />



## 重要定理

### 定理2.6 OD包含FD

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927191726479.png" alt="image-20200927191726479" style="zoom:50%;" />

### 定理2.7 当且仅当OD$X\mapsto XY$，FD$X\to Y$成立

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927191746188.png" alt="image-20200927191746188" style="zoom:50%;" />

**尝试证明**

### 定理2.9 OD=FD + OCD

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927191844782.png" alt="image-20200927191844782" style="zoom:50%;" />

- FD $X\to Y$有效，表明没有split
- OCD $X ～ Y$有效，表明没有swap

### 定理3.5 有重复属性的OCD可由没有重复元素的OCD推导得出

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200927193801379.png" alt="image-20200927193801379" style="zoom:50%;" />

#### 证明<font color=#FF000>（2，3不会证）</font>：

分为三种情况：

1. 证明开头具有重复属性的属性列表是多余的：

   <img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928141044546.png" alt="image-20200928141044546" style="zoom:33%;" />

2. 证明结尾具有重复属性的属性列表是多余的：

   <img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928141108241.png" alt="image-20200928141108241" style="zoom:33%;" />

3. 证明中间具有重复属性的属性列表是多余的：

   <img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928141136625.png" alt="image-20200928141136625" style="zoom:33%;" />

   

   <font color=#FF0000>***下文中又说：有些具有重复属性的ODs不能由没有重复属性的ODs推导得出***  </font>   

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928133649051.png" alt="image-20200928133649051" style="zoom:50%;" />

### 定理3.6 OCD向下闭包

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928132412234.png" alt="image-20200928132412234" style="zoom:50%;" />

**不会证**

### 定理3.7 OCD的剪枝规则

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928132442685.png" alt="image-20200928132442685" style="zoom:50%;" />

3.6的逆命题

### 定理3.8 当且仅当$XY \mapsto Y$有效，$X ～ Y$有效

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928134155480.png" alt="image-20200928134155480" style="zoom:50%;" />

**反向证明应该为：$XY \mapsto Y$，由(AX5)$XY\leftrightarrow YXY$，再由(AX3)得$XY \leftrightarrow YX$，即$X ～ Y$**

**这意味着ODs：$XY \mapsto Y$，与OCDs：$X～Y$等价**

### 定理3.9

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928135133055.png" alt="image-20200928135133055" style="zoom:50%;" />

**不懂**

### 定理4.1 当且仅当X~Y，$XY\mapsto YX$成立

​											<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928170616549.png" alt="image-20200928170616549" style="zoom:50%;" />											<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928170640900.png" alt="image-20200928170640900" style="zoom:50%;" />

证明见草稿（以后上传）

## THE OCDDISCOVER ALGORITHM

### 减少搜索空间的策略：

- 移除常量列
  - 因为常量列可以被任意列X排序，能产生大量ODs
- 移除order-equivalent列
  - 若$A\leftrightarrow B$，则当属性A出现在的ODs中，我们都可以用属性B去代替属性A，会产生大量ODs
  - 给所有的order-equivalent属性建立一个等价类，并选取一个代表，移除其他所有的列，然后储存这些删除掉的列的信息。

### 搜索树

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928143624781.png" alt="image-20200928143624781" style="zoom:50%;" />

使用广度优先策略，使较短的最小依赖项在较长的最小依赖项之前被发现。

每个OCD候选集会被检查是否有效，会出现下面2种情况：

1. 不有效，则不从该候选集节点产生任何别的候选集了。
2. 有效，则按照以下方式生成新候选集
   - 在一定条件下（下面解释），生成$XA ～ Y$
   - 在一定条件下（下面解释），生成$X～YA$

### 剪枝原则

如果发现了新OCD $X～Y$,我们进一步检查OD$X \mapsto Y$和$Y\mapsto X$的有效性。

- 如果$X \mapsto Y$，则不产生形如$XZ～Y$的候选集，即$X～Y$的左儿子被剪掉

- 如果$Y\mapsto X$，则不产生形如$X～YZ$的候选集，即$X～Y$的右儿子被剪掉

- 如果上述两个都有效，则删除该候选集节点的所有字树

### 索引

> 计算一个OD候选集是否有效，先将候选集左侧属性进行排序，建立一个只包含各元组在该排序中的位置，然后依次检查右侧属性是否违反排序规则。



### OCDDISCOVER

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928171150559.png" alt="image-20200928171150559" style="zoom:50%;" />

Line 3：删除$u$中的常量属性，删除等价属性，并选取一个作为代表保留

Line 4：<font color=#FF000>B>A不太懂是什么意思</font>。（4.2节黄色标记，B>A大概就是在关系中B属性排在A属性后）

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201014161027769.png" alt="image-20201014161027769" style="zoom:50%;" />

总体思想：层次遍历搜索树，检查每个OCD候选集节点的有效性（无效则不从该节点产生新子节点候选集；有效则根据剪枝规则继续判断是否产生子节点候选集，产生某种候选集）；再将该节点产生的新的子节点候选集加入下一层。直到某层为空，无候选集则算法结束。

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928171314193.png" alt="image-20200928171314193" style="zoom:50%;" />

Line 2：<u>LEN(r)代表r表中的元组数？或者理解为二维数组的大小？</u>

Line3：<font color=#FF000>重要函数，产生位置索引，还不太清楚怎么编写，需要继续研究</font>

Line5～12：已经对X进行字典序排序，检查Y是否也是顺序一致，若存在违规(Swap)，则X～Y无效，终止循环返回false。

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200928171339046.png" alt="image-20200928171339046" style="zoom:50%;" />

Line 3：$A^+$新候选集补充的属性A（在$u^`$中，不在X、Y中的集合）的集合

Line4～17：根据剪枝规则产生下一层的候选集节点

## 总结

### 思想是通过发现OCD顺道来发现OD

发现的OCD是完整的，且最小的。

使用了广度优先来遍历搜索树，并且证明了有开头，中间，结尾重复属性的OCD是多余的。所以只用在下一层生成形如XZ～Y，X~YZ的OCDs。再利用剪枝策略，验证$X \to Y，Y \to X$并根据结果进行剪枝。从而得到该关系实例中所有的最小OCD，以及OD。

### 发现的OD是完整的么，是最小的么

形如$X \to Y$的ODs（X与Y无交集），在广度优先遍历的时候产生。(文章中是如何保证最小的？OCD无效的节点就不会再产生子节点了，这样会漏掉一些ODs么？)

LHS与RHS有交集的ODs，由OCDs X～Y产生（$XY \leftrightarrow YX$）。OCDs是最小的且是完整的，所以该形式的ODs应该也是完整的。



### 

