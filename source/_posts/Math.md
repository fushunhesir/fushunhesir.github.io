---
title: 高等数学
tags: [数学,考研]
categories: [基础课程]
poster:
  topic: 标题上方的小字
  headline: 大标题
  caption: 标题下方的小字
  color: 标题颜色
date: 2023-05-01 14:01:34
description: 高等数学学习记录。
cover:
banner:
---

## 前导知识

* **三角函数公式**
  * **倒数关系**
    * $tan(\alpha)cot(\alpha)=1$
    * $sin(\alpha)csc(\alpha)=1$
    * $cos(\alpha)sec(\alpha)=1$
  * **商数关系**
    * $tan(\alpha)=\frac{sin(\alpha)}{cos(\alpha)}$
    * $cot(\alpha)=\frac{cos(\alpha)}{sin(\alpha)}$
  * **平方关系**
    * $sin^2(\alpha)+cos^2(\alpha)=1$
    * $1+cot^2(\alpha)=csc^2(\alpha)$
    * $1+tan^2(\alpha)=sec^2(\alpha)$
  * **诱导公式**
    * $sin(2k\pi+\alpha)=sin(\alpha)$
    * $cos(2k\pi+\alpha)=cos(\alpha)$
    * $tan(2k\pi+\alpha)=tan(\alpha)$
    * $cot(2k\pi+\alpha)=cot(\alpha)$
    * $sin(\pi+\alpha)=-sin(\alpha)$
    * $cos(\pi+\alpha)=-cos(\alpha)$
    * $tan(\pi+\alpha)=tan(\alpha)$
    * $sin(-\alpha)=-sin(\alpha)$
    * $cos(-\alpha)=cos(\alpha)$
    * $tan(-\alpha)=-tan(\alpha)$
    * **奇变偶不变，符号看象限**
  * **基本公式**
    1. **和差公式**
       * $cos(\alpha\pm\beta)=cos(\alpha)cos(\beta)\mp sin(\alpha)sin(\beta)$
       * $sin(\alpha\pm\beta)=sin(\alpha)cos(\beta)\pm cos(\alpha)sin(\beta)$
       * $tan(\alpha+\beta)=\frac{tan(\alpha)+tan(\beta)}{1-tan(\alpha)tan(\beta)}$
    2. **积化和差公式**
       * $sin(\alpha)cos(\beta)=\frac{sin(\alpha+\beta)+sin(\alpha-\beta)}{2}$
       * $cos(\alpha)cos(\beta)=\frac{cos(\alpha+\beta)+cos(\alpha-\beta)}{2}$
       * $sin(\alpha)sin(\beta)=\frac{cos(\alpha+\beta)-cos(\alpha-\beta)}{2}$
    3. **和差化积公式**
       * $sin(\alpha)+sin(\beta)=2sin\frac{\alpha+\beta}{2}cos\frac{\alpha-\beta}{2}$
       * $cos(\alpha)+cos(\beta)=2cos\frac{\alpha+\beta}{2}cos\frac{\alpha-\beta}{2}$
    4. **倍角公式**
       * $sin(2\alpha)=2sin(\alpha)cos(\alpha)$
       * $cos(2\alpha)=2cos^2(\alpha)-1=1-2sin^2(\alpha)$
       * $tan(2\alpha)=\frac{2tan(\alpha)}{1-tan^2(\alpha)}$

## 函数与极限

### 数列的极限

* **数列**：按一定**顺序排列**的数字
* **通项**：生成数列的法则
* **数列极限**：$|x_n-a|\textless\varepsilon$
  * **a**：极限值
  * $\varepsilon$：任意正数
* **收敛数列的性质**
  * **极限唯一性**：数列收敛，极限唯一
  * **有界性**：数列收敛必有界，数列有界不一定收敛
  * **保号性**：极限值符号可以确定此前一些数列项的符号
    * $a\textgreater 0(a\textless 0)\\ \exist N,当n\textgreater N时\\x_n\textgreater0(x_n\textless0)$
    * **推论**：如果数列{ $x_n$ }从某项起$x_n\ge0(x_n\le0)$,且$\lim_{n\to\infty}x_n=a$,那么$a\ge0(a\le0)$
  * **子序列收敛性**：原数列收敛于n，那么任意子数列也收敛于n
    * **子序列**：从原数列中**任意**抽取的**无限多项**数字，这些的数字的排列顺序与在原数列中**相同**

### 函数的极限

* **函数极限的定义**
  * **自变量趋于有限值**
    * $x\to x_0,\;\lim_{x\to x_0} f(x)=A$
    * 函数在$x_0$的去心邻域内有定义
    * **左极限**：从左侧靠近$x_0$
    * **右极限**：从右侧靠近$x_0$
    * *极限存在的充分比必要条件为左极限和右极限同时存在且相等，**不用关心函数在该点的取值***
  * 自变量趋近于无穷大时函数的极限
    * $x\to\infty,\;\lim_{x\to\infty}f(x)=A$
    * **也称$y=A$为水平渐近线**
* **函数极限的性质**
  * **唯一性**：如果极限存在，那么极限唯一
  * **局部有界性**：在极限的一个邻域内，满足有界
  * **局部保号性**：在极限的一个邻域内，满足符号和极限值相同
    * **推论**：如果极限存在，那么极限的符号和邻域内的符号相同
  * **与数列极限的关系**：如果$\lim_{x\to x_0}f(x)=A,\;lim_{n\to\infty}x_n=x_0$，那么数列$\lim_{n\to\infty}f(x_n)=A$

### 无穷小与无穷大

* **无穷小**
  * **无穷小是一个函数**，$x\to x_0,\;f(x)\to0$
  * **定理**：函数具有极限的充分必要条件：$x\to x_0,\;f(x)=A+\alpha$，其中$\alpha$是$x\to x_0$的无穷小
* **无穷大**
  * $x\to x_0,\;\forall M\textgreater0,\;|f(x)|\textgreater M$
  * **定理**：如果$f(x)$为无穷大，那么$\frac{1}{f(x)}$为无穷小；反之，如果$f(x)\ne0$，且$f(x)$为无穷小，那么$\frac{1}{f(x)}$为无穷大

### 极限运算法则

* **有限个无穷小之和为无穷小**
* **有界函数与无穷小 乘积为无穷小**
  * *常数与无穷小的乘积为无穷小*
  * *有限个无穷小的乘积为无穷小*
* 若$\lim f(x)=A,\;\lim g(x)=B$，那么
  * $\lim[f(x)\pm g(x)]=A\pm B$
  * $\lim[f(x)\cdot g(x)]=A\cdot B$
    * $\lim[cf(x)]=c\cdot\lim f(x)$
    * $\lim[f(x)^n]=[\lim f(x)]^n$
  * 若$B\ne0$，$\lim[\frac{f(x)}{g(x)}]=\frac{A}{B}$
  * **数列也满足以上规则**
  * 若$\varphi(x)\ge\psi(x)$，且$\lim\psi(x)=B,\;\lim\varphi(x)=A,\;$那么$A\ge B$
  * **复合函数**
    * $y=f(g(x))$
    * $\lim_{x\to x_0}g(x)=u_0$
    * $\lim_{x\to u_0}f(x)=A$
    * $lim_{x\to x_0}f(g(x))=A$

### 极限存在准则 两个重要极限

* **准则**

  * **数列**

    * 如果数列$\{x_n\},\{y_n\},\{z_n\}$，满足

      1. 当$n>n_0时\quad y_n\le x_n \le z_n$
      2. $\lim_{n\to \infty} y_n=a, \lim_{n\to \infty} z_n=a$

      那么$\lim_{n\to\infty}x_n=a$（***==夹逼准则==***）

    * *==单调有界函数必有极限==（**充分条件**）*

    * **柯西极限存在准则**

      * **思想**：越接近极限的数列项之间的差值会越来越小
      * **数学**：$\forall\varepsilon>0,\exist N,当n,m>N时，|x_m-x_n|<\varepsilon$
      * *极限存在的充分必要条件*

  * **函数**

    * 如果在$x_0$的去心邻域内或$|x|\textgreater M$时

      1. $g(x)\le f(x)\le h(x)$
      2. $\lim g(x)=a,\lim h(x)=a$

      那么$\lim f(x)=a$(***夹逼准则***)

    * 函数在$x_0$的左邻域内为单调有界，那么$f(x_0^-)$必存在

* **重要极限**

  * ==$\lim_{x\to 0} \frac{sinx}{x}=1$==
  * ==$\lim_{x\to \infty}(1+\frac{1}{x})^x=e$==

### 无穷小的比较

* **定义**
  * **高阶无穷小**：$\lim \frac{\beta}{\alpha}=0$，记为$\beta=o(\alpha)$
  * **低阶无穷小**：$\lim \frac{\beta}{\alpha}=\infty$
  * **同阶无穷小**：$\lim \frac{\beta}{\alpha}=c$
  * k**阶无穷小**：$\lim \frac{\beta}{\alpha ^k}=\infty$
  * **等价无穷小**：$\lim \frac{\beta}{\alpha}=1$，记为$\alpha\sim \beta$，==在求极限中本质为恒等变换==
* **定理**
  * $\beta与\alpha$**是等价无穷小的充分必要条件**
    * **思想**：两者相差很小很小，即差值为高阶无穷小
    * **数学**：$\beta=\alpha+o(\alpha)$
  * **两项之比的极限等于这两项的等价无穷小之比的极限**
    * **数学**：$\lim \frac{\alpha}{\beta}=\lim \frac{\tilde\alpha}{\tilde\beta}$

### 函数的连续性与间断点

* **连续性**
  *  *在该点的极限和该点的函数值相同*
  *  **左连续**
     * 在该点的左极限和函数值相同
  *  **右连续**
     * 在该点的右极限和函数值相同
* **间断点**
  * 在$x_0$没有定义
  * 在$x_0$有定义但是极限不存在
  * 在$x_0$有定义，且极限也存在，但是极限不等于函数值
  * **极限存在不一定连续，连续一定极限存在**
  * **分类**
    * **第一类间断点**：左极限和右极限存在
      1. 可去间断点
      2. 跳跃间断点
    * **第二类间断点**：不是第一类
      1. 无穷间断点
      2. 振荡间断点

### 连续函数的运算与初等函数的连续性

* **连续函数的和、差、积、商的连续性**
  * 如果两函数都在$x_0$连续，那么他们的和差积商都在$x_0$连续
* **反函数与复合函数的连续性**
  * 如果函数在区间上**单调增加(减少)且连续**，那么他的反函数也在该区间上**单调增加(减少)且连续**
  * $\lim_{x\to x_0}f(g(x))=\lim_{u\to u_0}f(u)=f(u_0)$
    * 如果函数$g(x)$在$x_0$连续，且$g(x_0)=u_0,f(x)$在$u_0$连续，那么复合函数也在$x_0$连续
* **初等函数的连续性**
  * **一切初等函数在定义域内都是连续的**
* **常用等价无穷小**
  * $ln(1+x)\sim x(x\to0)$
  * $e^x-1\sim x(x\to0)$
  * $(1+x)^{\alpha}-1\sim\alpha x$
  * $sin\;x\sim x$

* **幂指函数**：形如$u(x)^{v(x)}=a^b$，**一般采取取对数方式处理**

### 闭区间上连续函数的性质

* **有界性与最大值最小值定理**
  * **闭区间连续**的函数一定能取到最大最小值

* **零点定理和介值定理**

  * **零点定理**：在闭区间[a,b]上连续且$f(a),f(b)$异号，那么至少存在一点使$f(\xi)=0$
  * **介值定理**：在闭区间$[a,b]$上连续且$f(a)=A,f(b)=B$，那么至少存在一点使$f(\xi)=C,A\le C\le B$

* **一致连续性**
  * **定义**：两个点靠的很近的时候，它们的差值也很小很小
  * **定理**：如果在闭区间上连续，那么也是一致连续
    * **其他区间不一定，例如**$f(x)=\frac{1}{x},x\to 0$


## 导数与微分

### 导数概念

* **导数定义**

  * **可导**：*极限存在*。**导数本身是一个特殊极限**，极限存在必须左极限和右极限都存在且相等，因此导数的左导数和右导数也必须存在且相等，同时**隐含该点的函数值存在**
    * **可导的充要条件**：左右导数存在且相等

  * **导数**：$f^\acute(x)=\lim_{\Delta x\to0}\frac{f(x_0+\Delta x)-f(x_0)}{\Delta x}$

  * **单侧导数**
    * 左导数
    * 右导数
* **导数的几何意义**
  * **函数图像中的切线斜率**
* **导数可导性与连续性的关系**
  * **可导一定连续**：因为导数里面已经蕴含了函数在该点存在函数值，而且左右导数相等
  * **连续不一定可导**：连续仅能保证在该点的函数值存在，并不能保证**左右导数相等**或者**极限值不为无穷**
* **奇偶函数与周期函数的导数的性质**
  * 奇函数的导数为偶函数
  * 偶函数的导数为奇函数
  * 周期函数的导函数也是周期函数，且周期相同

* **适合用定义求导数的情形**
  * *基本初等函数*
  * *求导法则不能应用的时候，比如不知道函数的可导性*
  * *分段函数*

### 函数的求导法则

* **函数的和、差、积、商求导法则**
  * ==**前提**：u，v均可微==
    * $u+v=\dot u+\dot v$
    * $uv=\dot uv+u\dot v$
    * $\frac{u}{v}=\frac{\dot uv+u\dot v}{v^2}$

* **反函数的求导法则**
  * $\dot f^{-1}(x)=\frac{1}{\dot f(y)}$
    * $x=f(y)$在区间单调、可导且$\dot f(y)\ne0$
* **复合函数的求导法则**
  * $\frac{dy}{dx}=\frac{dy}{du}\frac{du}{dx}$
* **基本求导法则与导数公式**
  * **常数和基本初等函数的导数公式**
    * $\dot C=0$
    * $(x^{\mu}\acute)=\mu x^{\mu-1}$
    * $sin(x\acute)=cos(x)$
    * $cos(x\acute)=-sinx$
    * $tan(x\acute)=sec^2(x)$
    * $cot(x\acute)=-csc^2(x)$
    * $sec(x\acute)=tan(x)sec(x)$
    * $csc(x\acute)=-cot(x)csc(x)$
    * $(a^x\acute)=a^xlna$
    * $log_a(x\acute)=\frac{1}{xlna}$
    * $ln(x\acute)=\frac{1}{x}$
    * $(arcsin\;x\acute)=\frac{1}{\sqrt{1-x^2}}$
    * $(arccos\;x\acute)=-\frac{1}{\sqrt{1-x^2}}$
    * $(arctan\;x\acute)=\frac{1}{\sqrt{1+x^2}}$
    * $(arccot\;x\acute)=-\frac{1}{1+x^2}$

### 高阶导数

* 二阶及二阶以上的倒数统称为高阶导数
* **重要的n阶导数**
  * $(e^x)^{(n)}=e^x$
  * $sin\;x^{(n)}=sin(x+n\cdot\frac{\pi}{2})$
  * $cos\;x^{(n)}=cos(x+n\cdot\frac{\pi}{2})$
  * $ln(x+1)^{(n)}=(-1)^{(n)}\cdot\frac{(n-1)!}{(1+x)^n}$
  * $(uv)^{(n)}=\sum_{i=0}^{k}C_n^ku^{(n-k)}v^{(k)}$

### 隐函数及由参数方程所确定的函数的导数

* **隐函数的导数**
  * **隐函数**：**本质是函数**所以必须保证每个自变量只能对应一个函数值，形式是自变量和因变量纠缠
  * **隐函数求导**：一般直接对方程两边同时求导即可，利用复合函数求导法则
    * **幂指函数**：*一般采取对数求导法*
* **参数方程所确定函数的导数**
  * $x=\varphi(t)$，$y=\psi(t)$
  * $\frac{dy}{dx}=\frac{\dot\psi(t)}{\dot\varphi(t)}$

### 函数的微分

* **微分的定义**
* **可微**：$\Delta y=A\Delta x+o(\Delta x)$

* **微分**：$dy=A\Delta x$

* **函数的微分**：$dy=f\acute(x)\Delta x$，与$x,\;\Delta x$有关

* **自变量的微分**：$dx=\Delta x$
  * *因此函数的微分与自变量的微分之商是导数*

* **微分的几何意义**
  * 用切线来近似代替一小段的函数增量
* **微分与可导之间的区别与联系**
  * 微分准确描述了一种**以直代曲**的概念
  * **在一元函数中**，**可微和可导是等价的**，在多元函数中则不是

* **一阶微分不变性**
  * **定义**：$dy=f'(u)du=f'(u)u'(x)dx$
  * **意义**：积分运算的基础

### 一元微分学的简单应用

* **平面曲线的切线与法线**
  * **切线方程**：$y-y_0=f^\acute(x_0)(x-x_0)$
  * **法线方程**：$y-y_0=-\frac{1}{f^\acute(x_0)}(x-x_0)$
* **平面曲线的曲率**

## 微分中值定理与导数的应用

### 微分中值定理

* **罗尔定理** 
  * **费马引理**：==在$x_0$的邻域内$f(x)$有定义且在$x_0$处可导==，始终$f(x)\le f(x_0)(或f(x)\ge f(x_0))$，若$f(x)$可导，那么$f\acute(x_0)=0$
    * **驻点**：若$f'(x_0)=0$，那么$x_0$为驻点。结合费马引理，$f(x)在x_0$处取极值且可导，该点就是驻点，反之不然
    * **证明思路**：利用$f(x)\le f(x_0)$来得到两个结论，取交集推知必为0
  * **罗尔定理**
    * **条件**：$[a,b]连续，(a,b)可导，f(a)=f(b)$
    * **结论**：$f\acute(\xi)=0$
    * **证明思路**：利用闭区间连续函数最值定理，联合开区间连续得到费马引理的条件，从而推出结论
* **拉格朗日中值定理**
  * **条件**：$[a,b]连续，(a,b)可导$
  * **结论**：存在$f(a)-f(b)=f\acute(\xi)(a-b)$
    * $f(x)=f(x_0)+f'(\xi)(x-x_0)$
    * $f(x+\Delta x)-f(x)=f'(x+\theta\Delta x)\Delta x$
  * **推论**：如果一个函数在区间上可导且导数为0，**那么该函数为常数函数**
  * **证明思路**：通过辅助函数，将条件加强为罗尔定理条件，从而推知结论
* **柯西中值定理**
  * **条件**：$[a,b]连续，(a,b)可导，F\acute(x)\ne 0$
  * **结论：**那么存在$\frac{f(a)-f(b)}{F(a)-F(b)}=\frac{f\acute(\xi)}{F\acute(\xi)}$
  * **证明思路**：利用拉格朗日中值定理的辅助函数，用参数方程替换原函数，即可推知
* **微分中值定理的重要作用**：建立了函数值增量，自变量增量和导数之间的联系。*是研究函数性态的重要工具*

### 利用导数研究函数的性态

* ==**总结**：==
  * ==其中单调性，凹凸性对应性，拐点，极值点对应态。态是性的临界状态之一，当性和态都确定后，函数图像基本才能确定。==
  * ==凹凸性分析本质可以视为对一阶导数的单调性分析==

* **基本概念**
  * **极值**：函数某一个点周围的值都小于(大于)该点的值，那么这就是极值点
  * **凹凸性**
    * **凹**：$f(x_0)+f'(x_0)(x-x_0)<f(x),\quad x\in(a,b)$
    * **凸**：$f(x_0)+f'(x_0)(x-x_0)>f(x),\quad x\in(a,b)$
    * **凹**：$f(\frac{a+b}{2})\le\frac{f(a)+f(b)}{2}$
    * **凸**：$f(\frac{a+b}{2})\ge\frac{f(a)+f(b)}{2}$
  * **拐点**：凹凸性发生变化的点，*该点一定是连续点*

* **函数为常数的条件与函数恒等式的证明**

  * **函数为常数的充要条件**：在区间任意一点导数为0

  * **两函数差值为常数的充要条件**：两函数在定义域内任意一点的导数相等
  * **两函数差值为0的充要条件**：两函数在定义域内任意一点的导数相等，且$f(x)=g(x)$

* **函数单调性充要判别法**

  * **前提条件**
    * 设$f(x)$在$[a,b]上连续，(a,b)上可导$
  * **结论**
    * $f'(x)≥0\Leftrightarrow f(x)$在$(a,b)$上单调不减
    * $f'(x)≥0$且在$(a,b)$内$f'(x)\not\equiv 0$$\Leftrightarrow f(x)$在$(a,b)$上单调递增
  * **求单调区间一般方法**
    * 根据极值点和不可导点，分区间讨论

* **极值点的必要条件和充分判别法**

  * **必要条件**：$x_0$为极值点$\Rightarrow f'(x_0)=0$或该点不可导
  * **极值第一充分判别定理**
    * **条件**：函数在区间连续，函数在该点外可导
    * **结论**：该点左右两侧导数符号发生改变，那么该点为极值点。==并不需要该点可导==
  * **极值第二充分判别定理**
    * **条件**：函数在该点二阶可导
    * **结论**：$f'(x)=0,f''(x)\ne 0\Rightarrow$，该点为极值点
    * **本质**：还是该点左右导数符号发生改变，**不同在于，需要对该点性质进行强化(二阶可导)**
  * **求极值点的一般求法**
    * 找不可导点和$f'(x)=0$的点
    * **根据充分判别定理筛选，注意两个定理适应不同情况**

* **凹凸性定义与充要判别法**

  * **定理1**
    * **条件**：$f(x)$在$[a,b]$连续，$(a,b)$可导
    * **结论**：$f'(x)$在$(a,b)$上单调递增(减)$\Leftrightarrow$该函数在该区间是凹(凸)的
  * **定理2**
    * **条件**：$f(x)$在$[a,b]$连续，$(a,b)$二阶可导
    * **结论**：$f''(x)≥(≤)0,f''(x)\not\equiv 0\Leftrightarrow$该函数是凹(凸)的
  * **函数凹凸性区间的求法**
    * **本质**：求$f'(x)$单调区间

* **拐点的充分判别法**

  * **必要条件**：$f''(x)=0$或者$f''(x)$不存在
  * **第一充分判别定理**
    * **条件**：$f'(x)$在区间连续，$f'(x)$在该点外可导
    * **结论**：该点左右两侧$f''(x)$符号发生改变，那么该点为拐点。==并不需要该点二阶可导==
  * **第二充分判别定理**
    * **条件**：函数在该点三阶可导
    * **结论**：$f''(x)=0,f'''(x)\ne 0\Rightarrow$，该点为极值点
    * **本质**：还是该点左右导数符号发生改变，**不同在于，需要对该点性质进行强化(三阶可导)**

* **利用性态分析做函数图像**

  * 求定义域、间断点
  * 分析奇偶性、周期性等
  * 单调性分析
  * 凹凸性分析
  * 列表
  * ==**求渐近线**==
    * **垂直渐近线**：$\lim_{x\to a}f(x)=\infty$
    * **水平渐近线**：$\lim_{x\to\infty}f(x)=a$
    * **斜渐近线**$y=kx+b$：   $\lim_{x\to\infty}\frac{f(x)}{x}=k,\quad \lim_{x\to\infty}(f(x)-kx)=b$
  * 特殊点
  * 绘图

### 洛必达法则

* **未定式**：$\lim_{x\to a}\frac{f(x)}{F(x)}=\frac{0}{0},\frac{\infty}{\infty}$
* **洛必达法则**：求导再求极限，==单侧极限也适用==
* **导数之比极限不存在，不能推断极限不存在，需要使用其他方法**

### 泰勒公式

* **思想**：通过一个**已知点和曲线的公式**，用多项式去拟合该函数，多项式的阶越大，拟合越准确，但总有一个误差(皮亚诺余项/拉格朗日余项)
* **拉格朗日余项**：$R_n(x)=f(x)-P(x)=\frac{f^{(n+1)}(\xi)}{(n+1)!}(x-x_0)^{(n+1)}$

### 函数最大值最小值

* **闭区间$[a,b]$上连续函数$f(x)$最大值最小值问题**
  * 找出驻点、不可导点、两段点的函数值
  * 找出其中的最大最小值即可
* **$f(x)$在区间I上可导且仅有两个单调区间的最值问题**
  * 找到$f'(x)=0$的点
  * 判断左右导数符号
  * 若二阶导数存在，可以根据二阶导符号判断
* **归结为求$f(x)$在$(a,b)$的驻点的最值问题**
  * 若$\lim_{x\to a^+}f(x)=+\infty(-\infty),\lim_{x\to b^-}f(x)=+\infty(-\infty)$
  * $f(x)$的在区间上存在最小(大)值，通过求驻点方法求得
* **连续函数$f(x)$的极值点唯一的最值问题**

### 曲率

* **弧微分**
  * **概念**：有向弧段进行微分，即弧沿x方向的长度增长率。
  * **公式**：$ds=\sqrt{(1+y'^2)}dx$
* **曲率及计算公式**
  * **曲率**：曲线弯曲程度的度量
    * 角度：正比
    * 弧长：反比
  * **曲率公式**：$K=|\frac{\Delta\alpha}{\Delta s}|=\frac{|y''|}{(1+y'^2)^{3/2}}$
    * **特殊**：圆的曲率为$\frac{1}{r}$
  * **参数方程曲率公式**
    * $K=\frac{|\varphi'(t)\psi''(x)-\varphi''(t)\psi'(x)|}{[\varphi'^2(t)+\psi'^2(x)]^{3/2}}$
* **曲率圆与曲率半径**
  * **曲率圆**：在函数一点凹侧所作的与该点曲率相同的圆
  * **曲率中心**：曲率圆的圆心
  * **曲率半径**：曲率圆的半径

## 不定积分

### 不定积分的概念与性质

* **原函数与不定积分的概念**

  * **原函数**：$F'(x)=f(x)$，那么$F(x)$就是$f(x)$的原函数
    * ==内涵：$F(x)$在该区间是可导的，$f(x)$在该区间是连续的==
  * **原函数存在定理**：连续函数一定有原函数
    * ***原函数要么没有，要么有无数多个***
    * ***任意两个原函数的差值是常数***
  * **不定积分**：==$f(x)$的原函数的集合称为不定积分==，记为$\int f(x)dx$

* **基本积分表**

  <img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E7%A7%AF%E5%88%86%E8%A1%A8(1).png" alt="image-20230305164632323" style="zoom:30%;" />

​																						<img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E7%A7%AF%E5%88%86%E8%A1%A8(2).png" alt="image-20230305164734495" style="zoom:30%;" />

* **不定积分的线性性质**
  * ==前提：$f(x),g(x)$在区间上存在原函数==
    * 若$f(x),g(x)$的原函数存在，那么$\int [f(x)+g(x)]dx=\int f(x)dx+\int g(x)dx$
    * $\int kf(x)dx=k\int f(x)dx$

### 换元积分法

* **第一类换元法**
  * **思想**：凑微分，$t=\varphi(x)$，再由t来替换被积变量
  * **凑微分技巧**
    * 分母尽量简洁
    * **三角不等式技巧**
      1. $sin^{2k+1}cos^n,sin^ncos^{2k+1}\to u=cos\;x,u=sin\;x$
      2. $sin^{2k}cos^{2n}\to cos\;2x=1-2sin^2\;x=2cos^2\;x-1$
      3. $sec^{2k}tan^n,tan^{2k-1}sec^{n}\to u=tan\;x,u=sec\;x$
* **第二类换元法**
  * **思想**：$x=\varphi(t)$，$\varphi(t)$需要单调可导，从而保证反函数存在且可导

### 分部积分法

* **思想**：$uv=u'v+uv'$

### 有理函数的积分

* **有理函数的积分**

  * **有理函数**：形如$\frac{P(x)}{Q(x)}$

  * **有理函数的分解**
    * 根据分母是否可分来判断有理函数是否可以进行分解

* **可化为有理函数的积分**

  * $^n\sqrt{ax+b},\;^n\sqrt{\frac{ax+b}{cx+d}}$

### 积分表的使用

* 题目训练

## 定积分

### 定积分的概念与性质

* **概念**：和的极限，$\lim_{\lambda\to0}\sum_{i=1}^nf(\xi_i)\Delta x_i$

  * ==积分区间有限，被积函数有界==

* **可积性**

  * **充分条件**

    1. $f(x)$**在闭区间连续就可积分**

    2. **在闭区间有界，且只有有限个间断点就可积分**

  * **必要条件**

  1. 可积$\to f(x)$在$[a,b]$上有界

* **定积分的性质**

  * **线性性质**：$\int_a^b [\alpha\cdot f(x)+\beta\cdot g(x)]dx=\alpha\int_a^b f(x)dx+\beta\int_a^bg(x)dx$
  * **区间可加性**：$\int_a^b f(x)dx= \int_a^cf(x)dx+\int_c^bg(x)dx$
    * ==c可以在$[a,b]$内，也可以在$[a,b]$外==
  * $f(x)=1,\;\int_a^bf(x)=b-a$
  * **比较定理**：$f(x)≥0,\;\int_a^bf(x)dx≥0$
    * $f(x)≥g(x),\;\int_a^bf(x)dx≥\int_a^bg(x)dx$
    * $|\int_a^bf(x)dx|≤\int_a^b|f(x)|dx$
  * **估值定理**：$m(a-b)≤\int_a^bf(x)dx≤M(a-b)$
  * **定积分中值定理**：若函数在闭区间上连续，$\int_a^bf(x)dx=f(\xi)(b-a)$

### 微积分基本公式

* **积分上限函数及其导数**
  * **积分上限函数**：$\phi(x)=\int_a^xf(t)dt$
  * **积分上限函数的导数**：$\phi'(x)=f(x)$
  * **结论一**
    * **如果函数在闭区间$[a,b]$可积，那么$\phi(x)$是$[a,b]$上的连续函数**
    * **若$f(x)在[a,b]$连续，则积分上限函数$\phi(x)=\int_{x_0}^{x}f(t)dt是[a,b]$上的可导函数**
  * **结论二**
    * 若$f(x)$在$[a,b]$上连续，那么积分上限函数$\phi(x)=\int_{x_0}^{x}f(t)dt是f(x)在[a,b]上的一个原函数$
  * **不定积分与变限积分的关系**
    * 不定积分等于变限积分加一个不定常量
* **牛顿-莱布尼茨公式**
  * $\int_a^bf(x)=F(b)-F(a)$
  * ==**意义**：联系了定积分，原函数与不定积分，并通过原函数联系了微分学==
  * **推论**
    * 设$f(x),F(x)$在$[a,b]$连续，$F(x)$是$f(x)$在$(a,b)$上的一个原函数则：$\int_a^bf(x)dx=F(x)|_a^b=F(b)-F(a)$
    * 设$f(x)$在$[a,b]$连续，$F(x)$是$f(x)$在$(a,b)$上的一个原函数，且$F(a+0)=\lim_{x\to a+0}F(x),F(b-0)=\lim_{x\to b-0}F(x)$则：$\int_a^bf(x)dx=F(x)|_a^b=F(b-0)-F(a+0)$
* **奇偶函数与周期函数的积分性质**
  * **奇偶函数**
    * 奇函数在对称区间的积分值为0
    * 偶函数在对称区间的积分值为2倍
    * 若$f(x)为奇函数，则F(x)$为偶函数
    * 若$f(x)为偶函数，则F(x)$为奇函数

  * 周期函数
    * $\int_a^{a+T}f(x)dx=\int_0^Tf(x)dx$
    * $\int_0^{x}f(x)dx为周期函数的充要条件：\int_0^Tf(x)dx=0$

### 定积分换元法和分部积分法

* 题目训练

### 反常积分

* **本质**：极限

* **无穷限反常积分**
  * $\int_a^{+\infty}f(x)dx=\lim_{t\to+\infty}\int_a^tf(x)dx$
  * $\int_{-\infty}^{a}f(x)dx=\lim_{t\to-\infty}\int_t^af(x)dx$
  * $\int_{-\infty}^{+\infty}f(x)dx=\lim_{t\to+\infty}\int_0^tf(x)dx+\lim_{t\to-\infty}\int_t^0f(x)dx$
    * **发散**：极限不存在
    * **收敛**：极限存在
* **无界函数反常积分**
  * **瑕点**：邻域内无界的点
  * $\int_a^{b}f(x)dx=\lim_{t\to b^-}\int_a^tf(x)dx$
  * $\int_a^{b}f(x)dx=\lim_{t\to a^+}\int_t^bf(x)dx$

### 反常积分的审敛法     $\Gamma$函数

* **无穷限反常积分的审敛法**
  * 区间连续，$f(x)≥0,\;若F(x)=\int_a^{+\infty}f(x)$有上界，则反常积分收敛
  * **比较审敛原理**：区间连续，$0≤f(x)≤g(x),\;若\int_a^{+\infty} g(x)dx收敛,\;那么\int_a^{+\infty}f(x)dx收敛$
  * **比较审敛法I**
    * 区间连续，$f(x)≥0,\;\exist M>0,\;p>1$，使得$f(x)≤\frac{M}{x^p}\to$收敛
    * 区间连续，$f(x)≥0,\;\exist N>0$，使得$f(x)≥\frac{N}{x}\to$发散
  * **绝对收敛的反常积分必定收敛**
* **无界反常积分的审敛法**
  * **比较审敛法II**
    * 区间连续，$f(x)≥0,\;\exist M>0,\;q<1$，使得$f(x)≤\frac{M}{(x-a)^q}\to$收敛
    * 区间连续，$f(x)≥0,\;\exist N>0$，使得$f(x)≥\frac{N}{x-a}\to$发散
  * **极限审敛法则**
    * 区间连续，$f(x)≥0,\;x=a为f(x)瑕点，若\exist q,\;0<q<1,\;st\;\lim_{x\to a^+}(x-a)^qf(x)$存在，那么收敛，反之发散
* $\Gamma 函数$
  * $\Gamma (t)=\int_0^{+\infty}e^{-x}x^{t-1}dx\quad(t>0)$
  * **性质**
    * $\Gamma (t+1)=t\cdot\Gamma(t)$

## 定积分的应用

### 定积分在几何学上的应用

* **平面图形的面积**
  * **直角坐标系**
    * $A=\int_a^bf(x)dx$
  * **极坐标系**
    * $A=\int_a^b\frac{1}{2}\cdot \rho^2\cdot d\theta$
* **体积**
  * **旋转体的体积**
    * $V=\int_a^b\pi [f(x)]^2dx$
  * **平行截面面积已知的立体的体积**
    * $V=\int_a^bA(x)dx$
* **平面曲线的弧长**
  * **定理**：*光滑曲线弧是可求长的*
  * **公式**
    * **参数方程**：$ds=\sqrt{[\varphi'(t)]^2+[\psi'(t)]^2}dt$
    * **直角坐标系**：$ds=\sqrt{1+[y']^2}dx$
    * **极坐标系**：$ds=\sqrt{[\rho]^2+[\rho']^2}d\theta$

### 定积分在物理学上应用

* **变力沿直线作功**
  * **公式**：$dW=F(x)dx$
* **水压力**
  * **公式**：$dF=(\rho gh)dS$

## 微分方程

*微分方程是寻求问题函数表达式的工具。微分方程表示了目标函数及其导数之间的关系，通过解微分方程就能够找出目标函数表达式*

### 微分方程基本概念

* **微分方程**：凡是表示未知函数、未知函数的导数与自变量之间的关系的方程，叫做微分方程
  * **微分方程的阶**：最高阶导数的阶数
  * **微分方程的通解**：解得的微分方程中**任意常数**的个数等于**微分方程的阶数1**
  * **初值条件**：在通解基础上得到特解的条件

### 可分离变量的微分方程

* **一阶微分方程的对称形式**：$P(x,y)dx+Q(x,y)dy=0$
* **可分离变量的微分方程**：$g(y)dy=f(x)dx$，即能够将变量分离至一边只含有x，另一边只含有y

### 齐次方程

* **齐次方程**：能够化为$\frac{dy}{dx}=\varphi(\frac{y}{x})$形式的一阶微分方程
  * **主要思想**：通过对齐次方程进行变量替换，转化为可分离变量的微分方程
* **可化为齐次的方程**：通过变量替换将非齐次方程的常数项消去从而转化为齐次方程即可用齐次方程解法求解

### 一阶线性微分方程

* 

## 向量代数与空间解析几何

### 向量及其线性运算

* **向量相关概念**
  * **向量**：由大小和方向两个因素唯一确定的量
  * **向量夹角**：$[0,\pi]$
  * **向量的模**：向量的长度
    * 单位向量：模=1
    * 零向量：模=0
  * **向量共面**：*向量的公共起点和各个终点在同一个平面*
* **向量的线性运算**
  * **向量加减法**：*平行四边形法则，三角形法则*
    * *交换律*
    * *结合律*
  * **向量数乘**：*向量的伸缩变换*
    * *结合律*
    * *分配率*
  * **定理I**：非零向量a，向量b平行于a的充分必要条件$\to$存在唯一的实数$\lambda$，使得$b=\lambda a$
  * **向量不等式**
    * $|a+b|\le |a|+|b|$
    * $|a-b|\le|a|+|b|$
* **空间直角坐标系**
  * **思想**：*空间直角坐标系将向量、点和坐标联系起来，从而量化向量的计算*
  * **一般坐标系**：符合右手规则
  * **卦限**：逆时针
* **利用坐标作向量的线性运算**
  * 由**定理I结合坐标系**推出：$两向量平行\Leftrightarrow \frac{b_x}{a_x}=\frac{b_y}{a_y}=\frac{b_z}{a_z}$
* **向量的模、方向角、投影**
  * **向量的模与两点间的距离公式**
    * $|\vec{AB}|=\sqrt{(x_B-x_A)^2+(y_B-y_A)^2+(z_B-z_A)^2}$
  * **方向角与方向余弦**
    * **方向角**：与三条坐标轴的夹角，$(cos\;\alpha,cos\;\beta,cos\;\gamma)=\frac{\vec r}{|\vec r|}=\vec e_r$
    * **方向余弦**：上面向量对应的三个值
  * **向量在轴上的投影**
    * **本质**：数量，记为$Prj_ur或(r)_u$
    * **性质**
      1. $Prj_ua=|a|cos\;\varphi$
      2. $Prj_u(a+b)=Prj_ua+Prj_ub$
      3. $Prj_u(\lambda a)=\lambda\cdot Prj_ua$

### 数量积 向量积 混合积

* **两向量的数量积**
  * **点乘**：$a\cdot b=|a||b|cos\;\theta=|b|Prj_ba$
    * 交换律：$a\cdot b = b\cdot a$
    * 分配律：$(a+b)\cdot c=a\cdot c+b\cdot c$
    * 结合律：$(\lambda a)\cdot b=\lambda(a\cdot b)$
  * **结论**
    * **向量垂直的充分必要条件为$a\cdot b=0$**
    * **两向量夹角余弦**：$cos\;\theta=\frac{a\cdot b}{|a||b|}$
* **两向量的向量积**
  * **物理意义**：力矩，物体产生转动的根本原因
  * **叉乘**：$|a\times b|=|a||b|sin\;\theta$
    * **反交换律**：$b\times a=-a\times b$
    * **分配律**：$(a+b)\times c=a\times c+b\times c$
    * **结合律**：$(\lambda a)\times b=\lambda (a\times b)$
* **向量的混合积**
  * **定义**：$[abc]=(a\times b)\cdot c$
  * **几何意义**
    * *若混合积为0，那么三向量共面，反之不共面*。
    * *混合积的绝对值表示构成平行六面体的体积*

### 平面及其方程

* **曲面方程与空间曲线方程的概念**
  * **曲面方程**：$F(x,y,z)=0$
  * **曲线方程**：两个曲面的交线，因此用一个方程组表示
* **平面的点法式方程**
  * **思想**：法向量加上平面上一个点能唯一确定该平面
  * **方程**：$A(x-x_0)+B(y-y_0)+C(z-z_0)=0$
  * **推导关键**：数量积结论
* **平面的一般方程**
  * **方程**：$Ax+By+Cz+D=0$
  * **特殊平面**
    * D=0，表示通过原点的一个平面
    * A=0，表示平行于x轴的一个平面，**其余同理**
    * A=B=0，表示平行于xOy面的一个平面
* **两平面的夹角**
  * **定义**：用两平面的法向量夹角，**取锐角或直角**
  * **公式**：$cos\;\theta=\frac{|a\cdot b|}{|a||b|}$
  * **推论**
    * **两平面垂直**：$A_1A_2+B_1B_2+C_1C_2=0$
    * **两平面平行**：$\frac{A_1}{A_2}=\frac{B_1}{B_2}=\frac{C_1}{C_2}$
    * **点到平面的距离公式**：$d=\frac{|Ax_0+By_0+Cz_0|}{\sqrt{A^2+B^2+C^2}}$

### 空间直线及其方程

* 空间直线的一般方程

## 多元函数微分学

### 基础概念

#### 多元函数极限与连续性

**二元函数的极限**

* **二元函数极限判断**：$|f(x,y)-A|<\varepsilon$，从任何路径趋近

* **极限与无穷小的关系**：$\lim f(x)=A\Leftrightarrow f(x)=A+\alpha(x,y)$

* **求二元函数极限常用方法**：极限运算法则，放缩，变量替换

* **证明二元函数极限存在性**：找出一条路径的极限不存在、找出两条路径的极限不同

**二元函数的连续性**

* **二元函数的连续性判断**：$\lim_{(x,y)\to(x_0,y_0)} f(x,y) = f(x_0,y_0)$
* **二元初等函数在定义域连续**：x的初等函数与y的初等函数经过有限次四则运算，复合运算形成的函数
* **二元连续函数的性质**
  * **局部保号性**
  * **最大值最小值定理**
  * **介值定理**

#### 偏导数与全微分

**偏导数概念与计算**

* **定义**：$\frac{\partial f(x_0, y_0)}{\partial x}=\lim_{x\to x_0}\frac{f(x,y_0)-f(x_0,y_0)}{x-x_0}$
* **偏导数的计算**：分段函数一般按定义或者按照一元函数求导数方法，此时可以将其余变量视为常量，并且带入常值以简化计算。注意**混合偏导数**在第一步不能直接代入常值，**非混合二阶偏导数**直接带入值是没有问题的。

**可微性与全微分**

* **可微性**$\Leftrightarrow\Delta z=A\cdot\Delta x + B\cdot\Delta y + \rho$
* **全微分**：$dz=A\cdot\Delta x + B\cdot\Delta y$
* **充分条件**：偏导数存在且连续$\Rightarrow$可微。
* **必要条件**：可微$\Rightarrow$偏导数存在，$A=f'_x(x,y),B=f'_y(x,y)$。

* **性质之间关系**

  <img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E5%A4%9A%E5%85%83%E5%87%BD%E6%95%B0%E6%80%A7%E8%B4%A8%E9%97%B4%E5%85%B3%E7%B3%BB%E5%9B%BE.png" alt="image-20230424154940543" style="zoom: 33%;" />

* **偏导次序**：若混合偏导数都在定义域上连续，**那么二阶混合偏导数的次序与结果无关**

* **多元函数为常数的条件**：全微分恒等于0，或者说偏导数均为0

#### 多元函数的微分法则

* **全微分四则运算**：若$u=u(x,y),v=v(x,y)$，均可微，则

  $d(uv)=vdu+udv$

  $d(\frac{u}{v})=\frac{1}{v^2}\cdot(vdu-udv)$，

  $d(u+v)=du+dv$，

  $d(cu)=cdu$

* **多元复合函数微分法则**
  
  *多元函数与一元函数复合*：$\frac{du}{dt}=\frac{\partial u}{\partial x}\frac{dx}{dt}+\frac{\partial u}{\partial y}\frac{dy}{dt}$
  
  *多元函数与多元函数复合*：$\frac{\partial f}{\partial x}=\frac{\partial f}{\partial u}\frac{\partial u}{\partial x}+\frac{\partial f}{\partial v}\frac{\partial v}{\partial x}$
  
  *一阶全微分的形式不变性*：$d f=\frac{\partial f}{\partial u}du+\frac{\partial f}{\partial v}dv=\frac{\partial f}{\partial x}dx+\frac{\partial f}{\partial y}dy$

#### 隐函数微分法

* **方程式确定的隐函数求导**

  一元隐函数存在定理

  * **条件**：函数在$(x_0,y_0)$邻域内连续，且$F(x_0,y_0)=0，F'(x_0,y_0)\neq 0$
  * **结论**：在该邻域内，$F(x,y)=0\to y=f(x)，且y'=-\frac{F_x}{F_y}$
  * **计算方法**：复合函数求导法则

  二元隐函数存在定理

  * **条件**：函数$F(x,y,z)$在$(x_0,y_0,z_0)$附近有连续偏导数，且$F(x_0,y_0,z_0)=0,F'(x_0,y_0,z_0)\neq 0$
  * **结论**：在该邻域内，$F(x,y,z)=0\to z=f(x,y)$，且$z_x=-\frac{F'_x}{F'_z}，z_y=-\frac{F'_y}{F'_z}$
  * **隐函数一阶偏导计算方法**：复合函数求导法则、一阶全微分形式不变性、带入上述公式
  * **隐函数二阶偏导计算方法**：复合函数求导法则、对上述一阶结果继续求偏导

* **方程组确定的隐函数求导法则**：两边同时求导，再利用克莱姆法则，求得结果。

#### 复合函数求导法则的应用

* **作变量替换时，求函数在新变量下的偏导数，有时可以通过变量替换将某些偏微分方程化简**

  <img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E6%9E%81%E5%9D%90%E6%A0%87%E5%A4%8D%E5%90%88%E5%87%BD%E6%95%B0%E6%B1%82%E5%AF%BC%E5%8F%98%E6%8D%A2.png" alt="image-20230425162958095" style="zoom:50%;" />

#### 多元函数极值问题

* **驻点概念**：二元函数的驻点需要偏导数同时为0。**极值点一定是驻点，驻点不一定是极值点**。

* **多元函数取极值充分条件**

  $f'_x=0,f'_y=0,$令$f''_{xx}=A,f''_{xy}=B,f''_{yy}=C$

  1. $AC-B^2>0$，取极值且当$A>0$时取极小值，$A<0$时取极大值。
  2. $AC-B^2<0$，不是极值点。
  3. $AC-B^2=0$，可能取极值也可能不，需要用极值定义确定。

* **求二元函数极值点的一般步骤**

  1. 求出所有驻点
  2. 求出每个驻点的二阶导数的值，然后带入上述式子判断。
  3. **注意**：多元函数闭区间内的唯一极值点并不一定是最值点，马鞍面可以知道。部分方向的极值点并不是全局极值点，但是也可以改变多元函数图像的走向。

* **条件极值点的必要条件**

#### 多元函数的最大值与最小值问题

* **简单极值问题**

  在有界闭区域上连续的函数

  1. 求出所有偏导数为0以及偏导不存在的点的函数值
  2. 求出边界上的最大最小值
  3. 从所有值中找出最大最小值

* **条件极值问题**：一种解法是从条件函数解出$y(x),x(y)$，代入到原函数中，化为简单极值问题。另外一种解法是拉格朗日乘数法。拉格朗日乘数法本质上是通过函数条件极值点的梯度向量平行构建新函数，求新函数的简单极值问题。

#### 方向导数与梯度

* **方向导数**：通过对偏导数线性变换得到，线性变换的系数为方向向量。
* **梯度向量**：指向方向导数值最大的方向，模长等于最大方向导数的值。

#### 多元函数微分学的几何应用



### 基础题型

连续性问题，首先带入特殊情况检验极限是否相等。然后再确定极限存在性，然后才是应用一元函数方法求极限。

利用一阶全微分形式不变性，可以简化求一阶偏导数的计算。

**多元函数偏导数与全微分问题**

* 讨论函数在某点的可微性，一般有三种方法。第一种是定义法，第二种是利用可微分的必要条件否定可微，第三种是利用可微分的充分条件证明可微分。
* 求全微分的时候，最开始尽量利用一阶全微分不变性来简化后续计算。如例8.19
* 

**方向导数和梯度**

* 计算方向导数，一般需要先求出方向向量和梯度，然后将这两向量点乘得到方向导数。
