
<!--more-->

<!--
-->
> 在进行地震波传播过程的数值模拟时，主要使用有限差分或者有限元类的方法来求解弹性
波动方程，最近几年，间断加辽金的方法也越来越广泛的应用在地震波传播过程的数值模拟中，
这里主要是介绍 1D 情况下该方法的理论，以方便来理解该方法。

# **1. 一维弹性波一阶速度应力方程**
弹性波动方程可以有多种的表示形式，主要可以分为微分表示（强形式）和积分表示（弱形式）
两种，这里分别介绍这两种表示下的一维弹性波一阶速度应力方程组。
## **1.1 微分表示（强形式）**
关于一维弹性波一阶速度应力的具体推导，这里不进行介绍，其形式为：
\begin{align}
\partial_t v - \frac{1}{\rho}\partial_x \tau = 0 
\label{equ:1d-elastic-der-1}
\end{align}
\begin{align}
\partial_t \tau - \mu\partial_x v = 0 
\label{equ:1d-elastic-der-2}
\end{align}
公式 (\ref{equ:1d-elastic-der-1}) 和公式 (\ref{equ:1d-elastic-der-2}) 一起构成了
一维弹性波一阶速度应力方程组，其也可以改写成矩阵表示：
\begin{align}
\partial_t \mathbf{Q} + \mathbf{A}\partial_x \mathbf{Q} = \mathbf{f}
\label{equ:1d-elastic-der-matrix}
\end{align}
其中$\mathbf{Q}=\begin{bmatrix} \tau & v \end{bmatrix}^T$, 
$\mathbf{A} = \begin{bmatrix} 0 & -\mu \\\\ -\frac{1}{\rho} & 0 \end{bmatrix}$,
$\mathbf{f} = \begin{bmatrix} 0 & 0 \end{bmatrix}^T$。这里的 $\mathbf{f}$ 表示的是
震源项，当其不为0时，就表示加上对应的震源（可以理解成初始条件）。

## **1.2 积分表示（弱形式）**
<div>
积分表示的一维弹性波一阶速度应力方程可以使用微分表示的方程来推导得到，这里简单介绍一下
其推导的过程。

对公式 (\ref{equ:1d-elastic-der-matrix}) 等式两边分别使用将测试函数 $\phi_j$ 对整个物理
空间 $D_k$ 做积分，可以得到

\begin{align}
\int_{D_k} \partial_t \mathbf{Q}(x,t)\phi_j(x)dx 
+ \int_{D_k}\mathbf{A} \partial_x\mathbf{Q}(x,t)\phi_j(x)dx 
= 0.
\end{align}
对等号左边第二项做分部积分，可以得到：
\begin{align}
\int_{D_k} \partial_t \mathbf{Q}(x,t)\phi_j(x)dx 
- \int_{D_k}\mathbf{A}\mathbf{Q}\partial_x\phi_j(x)dx
+ \int_{\partial D_k}\mathbf{A}\mathbf{Q}\phi_j(x)\mathbf{n}dx
= 0.
\end{align}

我们将通量定义为：
\begin{align}
\mathbf{Flux} = \int_{\partial D_k} \mathbf{A}\mathbf{Q}\ell_j(\xi)\mathbf{n}d\xi
\label{equ:flux-def}
\end{align}

整理后，可以得到：
\begin{align}
\sum_{i=1}^{N_p}\partial_t \mathbf{Q}(\xi_i,t)\int_{D_k}\ell_i(\xi)\ell_j(\xi)Jd\xi
- \mathbf{A}\mathbf{Q}(\xi_i,t) \int_{D_k}\ell_i(\xi)\partial_{\xi}\ell_j(\xi)d\xi
= -\mathbf{Flux}
\end{align}

其中 $J$ 是雅克比矩阵 $J=dx/d\xi$，质量矩阵 $\mathbf{M}$ 和刚度矩阵 $\mathbf{K}$ 可
以表示为：
\begin{align}
\mathbf{M} = \int_{D_k}\ell_i(\xi)\ell_j(\xi)Jd\xi
\end{align}
\begin{align}
\mathbf{K} = \int_{D_k}\ell_i(\xi)\partial_{\xi}\ell_j(\xi)d\xi
\end{align}

最终，可以得到矩阵形式表示的积分形式的一阶速度应力方程：
\begin{align}
\mathbf{M}\partial_t\mathbf{Q} = \mathbf{AKQ} - \mathbf{Flux}
\end{align}

</div>

# **2. 间断加辽金正演模拟求解流程**

## **2.1 特征值与特征向量及其物理含义**
根据公式 (\ref{equ:flux-def}) 的通量的定义可以看到，我们如果可以将 $\mathbf{A}$ 表示
成一个对角矩阵与另一个矩阵的乘积后可以很容易的来计算对应的通量。通过特征值和特征向量
可以来实现，下面介绍一下对应的变换关系。

根据之前定义的矩阵 $\mathbf{A}$，可以看到其可以使用特征值来表示成如下的形式
\begin{align}
\mathbf{A}\mathbf{x}_p = \lambda_p\mathbf{x}_p
\end{align}
如果特征值矩阵可以表示为：
\begin{align}
\mathbf{\Lambda} = 
\begin{pmatrix}
\lambda_1 & & \\\\ & \ddots & \\\\ & & \lambda_m
\end{pmatrix}
\end{align}
对应的矩阵 $\mathbf{R}$ 是由特征向量组成的矩阵，其中 $\mathbf{x}$_m 表示每列的特征
向量：
\begin{align}
\mathbf{R} = \left( \mathbf{x}_1 | \mathbf{x}_2| ... | \mathbf{x}_p \right)
\end{align}

这样，矩阵 $\mathbf{A}$ 和 $\mathbf{\Lambda}$ 可以表示为：
\begin{align}
\begin{split}
\mathbf{A=R\Lambda R^{-1}} \\\\ 
\mathbf{\Lambda=R^{-1}A R} 
\end{split}
\end{align}

对于矩阵 $\mathbf{A}$，可以很容易得到其特征
值 $\lambda_{1,2} = \mp\sqrt{\mu/\rho} = \mp c$，根据特征值计算得到特征向量，
最后得到：
\begin{align}
\mathbf{\Lambda} =
\begin{pmatrix}
-c & 0 \\\\ 0 & c
\end{pmatrix}
\end{align}

\begin{align}
\mathbf{R} =
\begin{pmatrix}
Z & -Z \\\\ 1 & 1
\end{pmatrix}
\end{align}

我们把 $\mathbf{\Lambda}$ 分成正负两个特征值可以得到 $\mathbf{A}^{\pm}$:
\begin{align}
\begin{split}
\mathbf{A}^{-} & = \mathbf{R}\mathbf{\Lambda}^{-}\mathbf{R}^{-1}
= \frac{1}{2}
\begin{pmatrix}
-c & -cZ \\\\ -\frac{c}{Z} & -c
\end{pmatrix} \\\\ \mathbf{A}^{+} &
= \mathbf{R}\mathbf{\Lambda}^{+}\mathbf{R}^{-1} 
= \frac{1}{2}
\begin{pmatrix}
c & -cZ \\\\ -\frac{c}{Z} & c
\end{pmatrix}
\end{split}
\end{align}




