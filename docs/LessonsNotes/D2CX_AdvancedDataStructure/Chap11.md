---

title: Chap 11 | “Approximation”

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---
<h1 id="欢迎">Chap 11 | “Approximation”</h1>
!!! note "章节启示录"
    本章节主要介绍了近似算法相关的问题。我们知道对于一些较难的问题，给出严谨的多项式时间（即在所有情况下都是多项式时间）是很困难的，因此我思考去寻找近似的算法对之进行替代。~~启示录写的真的是越来越简略了~~

## 1.目标与标准

* What for?
   对于某些问题，我们无法使用多项式时间解决。因此我们尝试寻找一种多项式算法，使得最后的结果在一个可接受的偏差范围内。
* 标准（Approximation Ratio）：近似比  
  
    $\large max(\frac{C}{C^*},\frac{C^*}{C}≤\rho(n))$

    优化问题的近似方案是一种近似算法，它不仅将问题的实例作为输入，而且还将值 $ \varepsilon> 0$ 作为输入，因此对于任何固定 $\varepsilon$ ，该方案是 $(1+ \varepsilon)$ 近似算法。  
    如果对于任何固定的 $\varepsilon > 0$，该方案在其输入实例的大小 n 中按时间多项式运行。我们就称近似方案是a polynomial-time approximation scheme（PTAS）（多项式时间近似范式）
!!! 近似范式
    近似范式(approximation scheme)指的是对于某个优化问题的一族相同模式的算法，它们满足对于确定的 $\varepsilon > 0$，算法的近似比为 $1+\varepsilon$。

    * 当 $f(n,\varepsilon)$ 关于 $n$ 是多项式时，**我们称其为多项式时间近似范式(polynomial-time approximation scheme, PTAS)**。
    * 当 $f(n,\varepsilon)$ 关于 $n$ 和 $1/\varepsilon$ 都是多项式时，我们称其为**<font color="red">完全</font>多项式时间近似范式(fully polynomial-time approximation scheme, FPTAS)**。
  
    ![](./img/166.png)
##  2.装箱问题
给定 $N$ 个大小为 $S_1 ， S_2 ， ...， S_N$ 的物品，使得 $0 < S_i ≤ 1$ 对于所有的 $1 ≤ i ≤ N$ . 将这些物品包装在最少数量的箱子中，每个箱子的最大容量都是 $1$。

!!! example "一个例子🌰"
    给定 7 个item ，size 分别是 0.2 , 0.5 , 0.4 , 0.7 , 0.1 , 0.3 , 0.8 ,那么最少需要 3 个箱子。

    * bin 1 : 0.2 + 0.8
    * bin 2 : 0.7 + 0.3
    * bin 3 : 0.4 + 0.1 + 0.5

!!! warning "注意"
    装箱问题是一个 NPH 问题，但装箱问题的判定问题是一个 NPC 问题。  
    Decision problem: given K bins, can we pack N items? 

在介绍具体的方式前，我们先对其进行一个分类：在线算法（online algorithm）和离线算法（offline algorithm）

* 在线算法：输入是一个一个知道的，也就是一开始只知道第一个，随着进程进入才知道后面的部分
* 离线算法：输入是一下子全部知道的，也就是一开始就知道所有物品的size，可以进行更加合理的安排布置。

### 2.1 Next Fit（online）
```c++
void NextFit ( )
{   read item1;
    while ( read item2 ) {
        if ( item2 can be packed in the same bin as item1 )
            place item2 in the bin;
        else
            create a new bin for item2;
        item1 = item2;
    } /* end-while */
}
```
NF 策略总是选择当前最后一个bin，若剩余量足够，则放入，否则就开一个新的。  
NF 策略的箱子个数不会超过 $2M-1$，其中 $M$ 表示准确解。
!!! success "证明"
    证明当 NF 的结果为需要 $2M$ 或 $2M+1$ 个 bin 时，准确解为至少需要 $M+1$ 个bin。

    ![](./img/167.png)

### 2.2 First Fit（online）
```c++
void FirstFit ( )
{   while ( read item ) {
        scan for the first bin that is large enough for item;
        if ( found )
    place item in that bin;
        else
    create a new bin for item;
    } /* end-while */
}
```
FF 策略总是选择第一个能放下当前item的箱子，如果所有箱子都装不下了，那么就新开一个箱子。  
FF 策略总是使用不超过 $\lfloor 1.7M \rfloor$ 个 bin，并且存在一组能对边界取等的输入

### 2.3 Best Fit（online）
BF 策略总是选择能够容纳当前 item 且剩余空间最小的 bin（即 tightest），若所有 bin 都无法容纳当前 item，则新开一个 bin。
BF 策略也总是使用不超过 $\lfloor 1.7M \rfloor$ 个 bin，并且存在一组能对边界取等的输入。

### 2.4 First Fit Decreasing（offline）
离线做法的优势在于它能够获得所有 item 的信息以求统筹规划。这里给出的近似做法是，将 item 按照 size 降序排序，而后使用 FF（或 BF，由于单调性，两者等价）。

![](./img/168.png)

FFD 策略总是使用不超过 $\large \frac{11}{9}M+\frac{6}{9}$个 bin，并且存在一组能对边界取等的输入。

## 3.Knapsack Problem（背包问题）
问题描述：要打包容量为 $M$ 的背包。 给定 $N$ 个项目。 每个项目 $i$ 都有一个权重 $w_i$ 和一个利润 $p_i$。 如果 $x_i$ 是被打包的物料 $i$ 的百分比，则打包的利润将为 $p_ix_i$  。

### 3.1 Fractional Version（可以装0.1个物品，连续可分）
$x_i ∈ [0,1]$ ，我们可以很自然地想到用性价比（profit density）最优策略进行选择，即 $p_i/w_i$。这样得到的答案是准确解。

### 3.2 0-1 Version
$x_i∈{0,1}$，一个物品要么选要么不选。这是一个 NPH 问题，w我们可以近似算法来求较优解。

!!! tip "拓展：背包判定问题与背包问题"
    * 先给出结论，背包判定问题是NPC问题，但背包问题是NPH问题。[证明](https://zhuanlan.zhihu.com/p/102362515#:~:text=%E6%96%87%E7%AB%A0%E5%BC%80%E5%A4%B4%E7%BB%99%E5%87%BA%E7%BB%93%E8%AE%BA%EF%BC%9A%200%2F1%E8%83%8C%E5%8C%85%E5%88%A4%E5%AE%9A%E9%97%AE%E9%A2%98%E6%98%AFNPC%E9%97%AE%E9%A2%98%EF%BC%9B%200%2F1%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E6%98%AFNPH%E9%97%AE%E9%A2%98%E4%BD%86%E4%B8%8D%E6%98%AFNPC%E9%97%AE%E9%A2%98%E3%80%82%20%EF%BC%88%E4%B8%8B%E6%96%87%E4%BC%9A%E7%BB%99%E5%87%BA%E5%85%B7%E4%BD%93%E8%AF%81%E6%98%8E%E8%BF%87%E7%A8%8B%EF%BC%89,%E5%AF%B9P%E9%97%AE%E9%A2%98%E3%80%81NP%E9%97%AE%E9%A2%98%E3%80%81NPC%E9%97%AE%E9%A2%98%E3%80%81NPH%E9%97%AE%E9%A2%98%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB%E8%BF%98%E4%B8%8D%E5%A4%AA%E4%BA%86%E8%A7%A3%E7%9A%84%E5%90%8C%E5%AD%A6%EF%BC%8C%E5%8F%AF%E4%BB%A5%E5%85%88%E8%8A%B1%E5%87%A0%E5%88%86%E9%92%9F%E7%9C%8B%E7%9C%8B%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%AE%9A%E4%B9%89%E5%92%8C%E5%85%B3%E7%B3%BB%E5%9B%BE%EF%BC%8C%E7%84%B6%E5%90%8E%E5%86%8D%E7%9C%8B%E5%90%8E%E9%9D%A2%E7%9A%84%E5%85%B7%E4%BD%93%E8%AF%81%E6%98%8E%E8%BF%87%E7%A8%8B%20P%E9%97%AE%E9%A2%98%EF%BC%9A%E5%A6%82%E6%9E%9C%E4%B8%80%E4%B8%AA%E9%97%AE%E9%A2%98%E8%83%BD%E6%89%BE%E5%88%B0%E5%9C%A8%E5%A4%9A%E9%A1%B9%E5%BC%8F%E6%97%B6%E9%97%B4%E5%86%85%E8%A7%A3%E5%86%B3%E5%AE%83%E7%9A%84%E7%AE%97%E6%B3%95%EF%BC%8C%E9%82%A3%E4%B9%88%E8%AF%A5%E9%97%AE%E9%A2%98%E6%98%AFP%E9%97%AE%E9%A2%98%E3%80%82%20NP%E9%97%AE%E9%A2%98%EF%BC%9A%E5%A6%82%E6%9E%9C%E5%8F%AF%E4%BB%A5%E5%9C%A8%E5%A4%9A%E9%A1%B9%E5%BC%8F%E6%97%B6%E9%97%B4%E5%86%85%E9%AA%8C%E8%AF%81%E4%B8%80%E4%B8%AA%E9%97%AE%E9%A2%98%E7%9A%84%E8%A7%A3%E7%9A%84%E6%AD%A3%E7%A1%AE%E6%80%A7%EF%BC%8C%E9%82%A3%E4%B9%88%E8%AF%A5%E9%97%AE%E9%A2%98%E6%98%AFNP%E9%97%AE%E9%A2%98%E3%80%82%20NPC%E9%97%AE%E9%A2%98%EF%BC%9A%E4%B8%80%E4%B8%AANPC%E9%97%AE%E9%A2%98%E9%9C%80%E8%A6%81%E5%90%8C%E6%97%B6%E6%BB%A1%E8%B6%B3%E4%B8%A4%E4%B8%AA%E6%9D%A1%E4%BB%B6%EF%BC%9A%EF%BC%881%EF%BC%89%E8%AF%A5%E9%97%AE%E9%A2%98%E6%98%AFNP%E9%97%AE%E9%A2%98%EF%BC%9B%EF%BC%882%EF%BC%89NP%E9%87%8C%E6%89%80%E6%9C%89%E9%97%AE%E9%A2%98%E5%8F%AF%E4%BB%A5%E5%9C%A8%E5%A4%9A%E9%A1%B9%E5%BC%8F%E6%97%B6%E9%97%B4%E5%86%85%E5%BD%92%E7%BA%A6%E5%88%B0%E8%AF%A5%E9%97%AE%E9%A2%98%E3%80%82)  
    
    **0/1背包判定问题:**
    给定一个有限集合 $X$ ，对每一个 $x∈X$ ，对应一个值 $w(x)∈Z^+$ ，和相应的值 $p(x)∈Z^+$ 。另外，还有一个容量约束 $M∈Z^+$ 和一个价值目标 $K∈Z^+$。  
    问：是否存在 $X$ 的一个子集 $X^\prime$ ，使得 $\large\sum_{x∈X^\prime} w(x)≤M$ 而且 $\large\sum_{x∈X^\prime} p(x)≥K$

    ![](./img/169.png)  
    ![](./img/170.png)
#### 贪心算法    
我们可以使用贪心算法，贪心策略可以是总是选可以放得下的、还没放入中的，利润最大的或$p_i/w_i$最大的。这些做法的近似比都是 2。  
![](./img/171.png)

#### 动态规划算法
$W_{i,p}$ 为总利润正好是 $p$ 的最小权重集合。
$$
     W_{i,p} =
    \begin{cases}
        ∞  &(i=0)\\\\
        W_{i-1,p}  &(p_i>p)\\\\
        min\\{{W_{i-1},w_i+W_{i-1,p-p_i}\\}} &(otherwise)\\\\
    \end{cases}
$$

$$\large
i=1,…，n;p=1,…,n \;\;p_{max} \Rightarrow O(n^2\;p_{max})
$$
* 注意：input size包括 $\large p_{max}$ 的二进制编码长度 $d$，所以 $\large p_{max}=O(2^d)$ 是指数级的复杂度。
!!! abstract "伪多项式时间算法"
    * 多项式时间算法：根据输入的**数量**
    * 伪多项式时间算法：根据输入中最大数的**数量级**
        1.  当一个算法的最坏时间复杂度是依据输入的**数量级**的时候，我们就称算法的时间复杂偶是伪多项式时间。
        2. 给一个wiki上的解释可能更好理解：若一个数值算法的时间复杂度可以表示为输入数值规模N的多项式，但其运行时间与输入数值规模N的二进制位数呈指数增长关系，则称其时间复杂度为伪多项式时间。这是由于，N的值是N的位数的幂，故该算法的时间复杂度实际上应视为输入数值N的位数的幂。  
    

    背包问题实例的规模是它的参数所占的存储空间。那么动态规划中的 $\large p_{max}$ 所占用的空间便是 $d$ 位。因此动态规划算法也是一个伪多项式时间算法。
    [相关链接](https://www.zhihu.com/question/20686504)  
因此，如果 $\large p_{max}$ 很大，可以考虑将他们除以一个较大的数（比如十万），然后向上取整，再进行运算。但是这个带来了精度的损失，我们用 $\varepsilon$ 来表示。也就如下图所示：  

![](./img/172.png)
