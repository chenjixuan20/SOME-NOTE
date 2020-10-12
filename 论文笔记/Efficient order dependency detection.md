# Efficient order dependency detection**总结

## 符号含义

$C_l(X)$：在$l$层，包含所有的Y,使得$X\rightarrow_<Y$是否有效仍需要验证，且$|X| + |Y| = l$

$CS_l$：$l$层的所有候选集的集合

$X^{'}$：$X$的最长前缀，例：$X=ABCD$，则$X^{'}=ABC$

$\tau_X $：属性列X的排序分区，将元组根据属性列X的值分割成一些等价类，使得这些等价类的X值相等

$\tau_X^k$：表示X中值第k小的元组

$|\tau_X|$：表示$\tau_X$中等价类数量

> 因为使用$\tau_X$这种方法，所以没有必要去存储整个元组的数据值。只需要存储元组的标识符(行索引)

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010153403581.png" alt="image-20201010153403581" style="zoom:50%;" />

---

## 定义

![image-20201010145746281](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010145746281.png)

***（不太清楚）***

![image-20201010150129327](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010150129327.png)

***

## 引理

![image-20201010132849677](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010132849677.png)

![image-20201010133439082](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010133439082.png)

![image-20201010133513961](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010133513961.png)

![image-20201010135453238](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010135453238.png)

![image-20201010135505020](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010135505020.png)

![image-20201010140104156](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010140104156.png)

![image-20201010155640612](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010155640612.png)

> 排序分区和merge之的关系，若(X,Y)中存在merge，则元组s,t中存在后等前不等的情况，则在$\tau_X$中s,t位于不同的等价类中，但是 $\tau_Y$中s,t位于同一个等价类中。
>
> ***引理7的表述不太清楚***（$\tau_X \times\tau_Y$是什么意思，injective是什么意思），e表示一个等价类

***

## 定理

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010134320307.png" alt="image-20201010134320307" style="zoom:50%;" />

​	定理1表明，发现所有基于$<$操作符的n元OD，便可以发现所有基于$\leq$操作符的n元OD。

***

## 晶格网络

**用晶格网络来表示搜索空间**

![image-20201010141036771](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010141036771.png)

第K层的节点包含k个属性并且代表了k-1个候选ODs。

***

## checkForSwap算法

算法检查$\tau_X，\tau_Y$中是否至少存在一个swap

$e_X$表示$\tau_X$中的第i小的等价类（簇）

![image-20201010162501046](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010162501046.png)

### 代码解析：

​	line3: $i< |\tau_X|$...

​	考虑两种情况：

1. $e_X$标识符数量小于$e_Y$
   - 若没有$e_X \subseteq e_Y$，则返回swap
   - 否则$merge = true$，由于$|e_X|<|e_Y|$，$e_Y$中还会有一个元组没进行比较，所以取下一个$e_X$，然后把$e_Y$中没处理过的元组保留，进行下一次比较。
2. $|e_X|$等于或大于$|e_Y|$
   - 若没有$e_Y \subseteq e_X$，则返回swap
   - 否则取下一个$e_Y$，删除当前$e_X$中处理过的元组
     - 若$|e_X|=0$，则取下一个$e_X$
     - 否则继续处理当前$e_X$

若$merge=true$成立，则返回merge

否则返回valid

***

## **Product of sorted partitions**(分区的乘积)

### 简述思路

![image-20201010172800734](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010172800734.png)

![image-20201010173826293](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010173826293.png)

> 1. 等价类大小为1时
>    - 直接加入到$\tau_ {AB}$
> 2. 等价类大小大于1时
>    - 根据在$\tau_B$中出现的位置分成新的等价类再加入$\tau_ {AB}$中

> 使用类似哈希连接的过程实现两个排列分区的乘积
>
> 1. 创建哈希表$H_P$，将元组标识符t映射到t所在的$\tau_A$中的位置（只保存大小大于1的等价类）
> 2. 遍历$\tau_B$，建立哈希表$H_S$，将$\tau_A$中大小大于1的等价类的位置映射到$\tau_{AB}$的等价类列表中

![image-20201010184616744](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201010184616744.png)

$tide_{eB}$：等价类$e_B$中的元组标识符

Visited：记录$\tau _B$中的等价类$e_B$中的元组$tid_{eB}$(在$\tau_A$中所属$e_A$大小大于1)，在$\tau_A$中的位置i（$H_p(tid_{eB})$）的集合。

$H_p$：记录大小大于1的等价类中的元组标识符，$H_p(3)=2$表示元组标识符3在$\tau_A$的第二个等价类中

$H_S$：根据逐一遍历$\tau_B$在中的等价类中的元组标识符的结果，记录由$\tau_A$中大小大于1的等价类经过$\tau_B$处理得到的$\tau _{AB}$中的等价类的所有标识符，$H_S(2)=\{ \{ 2,4\} ,\{3\}\}$表示在$\tau_A$中的等价类$\{2,3,4\}$，由于$\tau_B$在$\tau_{AB}$中分为2个等价类$\{2,4\},\{3\}$。

<img src="/Users/chenjixuan/Downloads/IMG_0004.PNG" alt="IMG_0004" style="zoom:50%;" />

### 代码解析：

​	Line5：如果$H_p$的$tid_{eB}$位置为空，表示该元组的等价类大小为1，则继续遍历。

​	Line7：若不为空，表示等价类大小大于1，先记录下该元组在$\tau _A$中的位置pos。

​	Line8：将pos加入visited。

​	Line9：将该元组的标识符加入$H_S$中pos位置的最后一个等价类中。

​	Line10~11：遍历完$\tau _B$中一个等价类中所有元组后，在$H_S$中pos出现过的位置加入一个空集合。来确保$\tau _A$中一个等价类中的元组标识符（$H_p$中不为空且值相同的位置）能继续添加到$H_S$中。

​	Line12~17：在遍历完$\tau_B$后，$\tau_{AB}$中的总体顺序应该与$\tau_A$相同（将表按A排序和将表按AB排序大致相同），再遍历$e_A$，对$\tau_A$中对等价类$e_A$处理得到$e_{AB}$。

- 若$|\tau_A^i|=1,i.e.|e_A|=1$，则直接将$e_A$加入$\tau_{AB}$
- 否则将$H_S(i)$中的非空等价类$e_{AB}$依次加入$\tau_{AB}$

***

## 剪枝原则

![image-20201012111855818](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201012111855818.png)

> (A,CD)中存在merge，则(AB,CD)中依然存在merge
>
> (A,CD)中存在swap，则(AB,CD)中依然存在swap

![image-20201012112532873](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201012112532873.png)

> (A,BC)中没有merge或swap，则(A,BCD)中肯定也没有merge或swap

![image-20201012111928803](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201012111928803.png)

> (A,C)中存在swap，则(AB,CD)中肯定也存在swap，因为在A，C右侧加入属性并没有改变元组在A,C中原先的顺序

![image-20201012112229232](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20201012112229232.png)

> 由Lemma6得

由于自下而上的生成候选集，晶格网络中$l$层的节点均是$l+1$层节点的前缀，在生成候选集的时候使用剪枝规则可以有效降低时间复杂度

***

## ORDER算法

#### 代码

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200919103535288.png" alt="image-20200919103535288" style="zoom:50%;" />

> 过程：自下而上的遍历整个晶格网络

---

### ***updateCandidateSets()函数***

> 由下一层的候选集集合$CS_{l-1}$生成$l$层候选集集合$CS_l$

- 当$|X| = l-1$时，$C_l(X)$由$C_{l-1}(X^{'})$生成

> 当$B\in C_{l-1}(X^{'})$且$B$与$X$不相交，则$B \in C_l(X)$

- 当$1 \leq |X| \leq l-1$时，$C_l(X)$由逐渐加入$Y \in C_{l-1}(X)$扩展而得到的$U$得到

> $|U|=|Y|+1$
>
> $Y \in C_{l-1}(X),E \in R,U = YE$，其中 $Y,E,X$两两不相交

#### 代码

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200919110446247.png" alt="image-20200919110446247" style="zoom:50%;" />

#### 代码解读:

- 当$|X| != l-1$时
  - 若$X \rightarrow _< Y \in vaild$，则不用扩展$Y$，因为$X \rightarrow _< YW \in valid$，且$YW$不是最小,不用加入$C_l(X)$
    - $EXTEND(X,Y)$函数将$Y$拓展成$U$，$U=YE,E \in R$，需要$X$参数的原因是控制$X,Y,E$均不相交，该$U$会经过后续一系列的判断后再决定是不是要加入$C_l(X)$
       -  <u>**按10到13行的规则进行筛选，但是我没太弄懂**</u>
          	-  大致分析分析
           -  若$U \notin C_{l-1}(X^{'})$,则$X^{'}V \rightarrow UW$的有效性已知，当$W = \emptyset ,X^{'}V = X$时，$X \rightarrow _< U$有效性已知，不用将$U$加入$C_l(X)$
               -  $U \notin C_{l-1}(X^{'})$有共5种情况
                  1. $U$非最小
                  2. $U$由于***swap***被剪枝
                  3. $U$由于***uniqune***被剪枝
                  4. $U$由于***merge-pruning***被剪枝
                  5. $X \rightarrow_< T,T \in prefix(U)$（<u>**为什么？**</u>）
              -  1～4种情况不用将$U$加入候选集
              -  第5种需要。当第5种情况时，必定存在$X^{'} \rightarrow Q，Q \in prefix(U)$，（<u>**为什么?**</u>）
              -  Line12：表示若不是第5种情况，则进行下一次循环，不把U加入候选集
    	-  最小的$U$才加入候选集$C_l(X)$
  
- 当$|X| = l-1$时
  - $C_l(X)$由$C_{l-1}(X^{'})$生成，当$B\in C_{l-1}(X^{'})$且$B$与$X$不相交，则$B \in C_l(X)$
- 最后将生成的$C_l(X)$加入到$CS_l$中​，知道该层候选集集合填充完毕

> ***updateCandidateSets***函数生成的下一层的候选集集合$CS_l$中的每个$C_l(X)$，均是根据$C_l(X)$的准则而生成，还需要对其中的OD进行验证，以及对其中的$C_l(X)$中的$Y$进行修剪。

---

### ***computeDependencies()函数***

> 计算$l$层的节点X的候选集$C_l(X)$中有效OD,并对其中的$Y$进行删减

#### ***obtainCandidates()函数***

​	**前提知识**：一个第$n$层的结点，可以产生$n-1$个候选OD

​		例：在第三层，结点ABC，可以产生$A \rightarrow _<BC,AB \rightarrow_< C$ 这2个候选OD

> obtainCandidates()函数将结点node,分为候选OD的左侧LHS和右侧RHS

#### 从$C_l(X)$删除Y的规则

<img src="/Users/chenjixuan/Downloads/IMG_0005.PNG" alt="IMG_0005" style="zoom:50%;" />

#### ***merge-pruning***

> 仅由于$merge$导致$X\nrightarrow_<Y$时，$XV\to _<YW$是否有效无法得知，可能有效也可能无效。只能推理出$XV\nrightarrow _<Y$，

文章中原话：

> Instead, assume we know in addition to $A \nrightarrow_<B$ being invalidated only by a *merge* that any $A\to_<BY,(Y\neq \empty)$ need not be checked. Then, we need not check any OD of the form  $AX \nrightarrow_<BY$

翻译翻译：

> 然而，假如除了知道$A \nrightarrow_<B$是由于***merge***造成的，我们还知道任何$A\to_<BY,(Y\neq \empty)$ 均不需要检查，于是，我们不需要检查形如$AX \nrightarrow_<BY$格式的OD

<u> 那么$A\to_<BY,(Y\neq \empty)$ 均不需要检查有两种情况</u>

1. $A\to_<BY,(Y\neq \empty)$ 

   $\because A \nrightarrow _<B$

   $\therefore AX \nrightarrow B$

   又$\because A \to _< BY$

   <u>**不会证了**</u>

2. $A\nrightarrow_<BY,(Y\neq \empty)$ 

   $\because A \nrightarrow _<B$，

   $ \therefore AX \nrightarrow _< B$

   $又\because A \nrightarrow_< BY$

   $\therefore AV \nrightarrow _< BY$

   **不需要检测上述格式得证**

> If the OD $maxPrefix(X) \to _< Y $was invalidated only by a *merge* (this is knowledge gained in level $l − 1$), we know that $X \nrightarrow _<Y$(and also,$XV\nrightarrow _<Y$ for any $V$).Then,if we cannot find in $C_l (maxPrefix(X)) $any $ V$ that starts with $Y$, we need not examine any $X \to_< YZ$, because it is either invalid or not minimal. 

证：

$X^{'}\nrightarrow _<Y $已知，$X^{'}\rightarrow _<YE(V)$的有效性已知（$\because \nexists V \in C_{l}(X^{'}) ,且 V = YE$）

> $\because|V|+|X^{'}|=l,|Y|+|X|=l$
>
> $\therefore |E|=1$

$\therefore X^{'}M \to _< VN$有效性已知（由于$ \nexists V \in C_{l}(X^{'})$）

令$X^{'}M=XA,VN=YEN=YZ(V=YE,Z=EN)$时，上式可以表示为$XA\to _<YZ$,其有效性已知

$\therefore$可当满足上述条件，可以将$Y$从$C_l(X)$中删除

同时当$A=\empty$时，上面的那段英文得证：

- 要么$X\nrightarrow _<YZ$
- 要么$X\to _<YZ$，但非最小OD。最小OD为$X^{'} \to V,X^{'}\subseteq X,V\subset YZ$

#### 伪代码：

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200919145048975.png" alt="image-20200919145048975" style="zoom:50%;" />

#### 代码解析：

- 遍历该层所有当结点，以得到所有的X，Y
  - 当$Y \notin C_l(X)$时，不进行检查
  - 当$X \to _< Y$有效时，将$X\to_<Y$加入$valid$集合
    - 当X时$unique$时，从$C_l(X)$中删除Y。<u>因为可推$XV\to_<YW$(仅V，W可为$\emptyset$)必定有效，根据从$C_l(X)$中的删除规则，将Y删除</u>
    - 当由于$swap$导致$X \nrightarrow _<Y$时，从$C_l(X)$中删除Y。<u>因为可推$XV \nrightarrow YW$(仅V,W可为$\emptyset$)，从而删除Y</u>
- 遍历完后，再对整层的结点进行***merge-pruning***(因为merge-pruing过程中需要用到整层的$CS_l$，比如在进行$Y \in C_l(X)$遍历的时候需要用到$C_l（X^{'})$中的元素进行判断)。

---

### ***prune()函数***

> 从晶格网络中删除结点的规则：只有确认该结点不会再生成包含还不知道有效性的OD候选集的结点
>
> 结点n的$｜n｜-1$个候选集全为空，表示以n作为前缀的更大的结点不会产生了，我们则将其从晶格网络中删掉

#### 代码：

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200920102235428.png" alt="image-20200920102235428" style="zoom:50%;" />

---

### ***generateNextLevel()函数***

> 1. 将两个大小为$l$的结点分成2部分$(X,Y)$,其中每个结点的$|X|=l-1$,且相同，$Y$则不同
> 2. 再将这2个结点结合在一起生成大小为$l+1$的结点
> 3. 为下一层产生空的候选集

#### ***prefixBlocks()函数***

> 将大小为$l$的结点分成有着相同的大小为$l-1$的前缀的结点列表$prefixBlock$，每个$list$中所有节点的$l-1$前缀相同

#### 代码：

![image-20200920110100608](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200920110100608.png)

