# **Efficient order dependency detection**总结

## 符号含义

$C_l(X)$：在$l$层，包含所有的Y,使得$X\rightarrow_<Y$是否有效仍需要验证，且$|X| + |Y| = l$

$CS_l$：$l$层的所有候选集的集合

$X^{'}$：$X$的最长前缀，例：$X=ABCD$，则$X^{'}=ABC$

---

## ***ORDER算法***

#### 代码

<img src="/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200919103535288.png" alt="image-20200919103535288" style="zoom:50%;" />

> 过程：自下而上的遍历整个晶格网络

---

### ***updateCandidateSets()函数***

> 由下一层的候选集集合$CS_{l-1}$生成$l$层候选集集合$CS_l$

#### 当$|X| = l-1$时，$C_l(X)$由$C_{l-1}(X^{'})$生成

> 当$B\in C_{l-1}(X^{'})$且$B$与$X$不相交，则$B \in C_l(X)$

#### 当$1 \leq |X| \leq l-1$时，$C_l(X)$由逐渐加入$Y \in C_{l-1}(X)$扩展而得到的$U$得到

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
                  5. $X \rightarrow_< T,T \in prefix(U)$,<u>***为什么？我感觉应该将$X$改为$X^{'}$***</u>
              -  1～4种情况不用将$U$加入候选集
              -  第5种需要<u>***（这不就与上面分析的推导矛盾了么？）***</u>
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

#### ***merge-pruning***

> 仅由于$merge$导致$X\nrightarrow_<Y$时，$XV\to _<YW$是否有效无法得知，可能有效也可能无效。只能推理出$XV\nrightarrow _<Y$，

文章中原话：

> Instead, assume we know in addition to $A \nrightarrow_<B$ being invalidated only by a *merge* that any $A\to_<BY,(Y\neq \empty)$ need not be checked. Then, we need not check any OD of the form  $AX \nrightarrow_<BY$

翻译翻译：

> 然而，假如除了知道$A \nrightarrow_<B$是由于***merge***造成的，我们还知道任何$A\to_<BY,(Y\neq \empty)$ 均不需要检查，于是，我们不需要检查形如$AX \nrightarrow_<BY$格式的OD

<u> 那么$A\to_<BY,(Y\neq \empty)$ 均不需要检查有两种形式</u>

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

$\because \nexists V \in C_{l}(x^{`}) ,且 V = YE$

$\because|V|+|X^{'}|=l,|Y|+|X|=l$

$\therefore |E|=1$

$\therefore X^{`}M \to _< VN$有效性已知

令$X^{`}M=XA,VN=YEN=YZ$时，上式可以表示为$XA\to _<YZ$,其有效性已知

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
    - 当由于$swap$导致$X \nrightarrow _<Y$时，从$C_l(X)$中删除Y。<u>因为可推$XV \nrightarrow YW$(仅V,W可为$\emptyset$)，从而删除Y
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

