
<!--more-->

> 本文主要是介绍开源的地球物理相关的软件（地震学）及获取的方式

# **0. 网址汇总**
这部分主要是将一些网址进行汇总，里面包含了很多相关的代码，如果在下面的介绍中没有
提到可以在这里进行查找。

- [iLab - Software](http://www.iearth.edu.au/codes/)
- [Princeton University - Theroretical & Computational Seismology](https://tromp.princeton.edu/software)
- [SeisSol](http://www.seissol.org/)
- [NEXD](http://www.gmg.ruhr-uni-bochum.de/geophysik/seismology/nexd.html)
- [IRIS-dmc-software](https://ds.iris.edu/ds/nodes/dmc/software/downloads/)

# **1. 正演模拟算法及软件**

## **1.1 射线追踪正演模拟算法及软件**
这部分主要是介绍计算地震波射线路径及走时的射线追踪正演算法及软件。

## **1.2 地震波半解析正演模拟算法及软件**
这部分主要是介绍地震波传播过程的半解析数值模拟正演算法及软件。

### **1.2.1 FK**

## **1.3 地震波数值正演模拟算法及软件**
这部分主要是介绍地震波传播过程的纯数值模拟正演算法及软件。

### **1.3.1 SPECFEM**
SPECFEM 程序使用谱元法 (SEM) 来进行地震波传播的数值模拟，该方法目前是天
然地震中用的比较多的一种数值正演模拟算法，目
前 [Jeroen Tromp](https://tromp.princeton.edu/people/jeroen-tromp) 等
人发展出来的一系列的开源程序得到了广泛的使用。
如果需要了解更多可以进入 [Princeton University - Theoretical & 
Computational Seismology 主页](https://tromp.princeton.edu/software)进行
查看，其代码地址为：

- [SPECFEM1D](https://github.com/geodynamics/specfem1d) 一维地震波传
播正演模拟程序，主要是为了让大家学习如何来实现谱元法的代码。
- [SPECFEM2D](https://github.com/geodynamics/specfem2d) 二维地震波传播
正演模拟程序。
- [SPECFEM3D](https://github.com/geodynamics/specfem3d) 三维笛卡尔坐标系
地震波传播正演模拟程序。
- [SPECFEM3D_GLOBAL](https://github.com/geodynamics/specfem3d_globe) 三维
全球地震波传播正演模拟程序。

### **1.3.2 SeisSol**
[SeisSol](http://www.seissol.org/) 程序是使用间断加辽金法 (DG) 来进行三维的
地震波传播以及地震破裂过程的模拟，其可以显示时间和空间任意高阶，目前已经
比较多的用于地震破裂过程的模拟中。如果需要了解更多可以进 
入 [SeisSol 主页](http://www.seissol.org/)进行查看，其代码地址为：

- [SeisSol](https://github.com/SeisSol/SeisSol) 三维地震波传播和地震破裂正演模拟

### **1.3.3 NEXD**
[NEXD](http://www.gmg.ruhr-uni-bochum.de/geophysik/seismology/nexd.html) 程序
是使用间断加辽金法 (DG) 来进行地震波传播过程的模拟。该程序主要是德国波鸿鲁尔大学
的课题组来开发的，相关信息可以进
入 [NEXD 主页](http://www.gmg.ruhr-uni-bochum.de/geophysik/seismology/nexd.html)
进行查看，其代码地址为：

- [NEXD-2D](https://github.com/seismology-RUB/NEXD-2D) 二维地震波传播正演
模拟算法
- [NEXD-3D](https://github.com/seismology-RUB/NEXD-3D) 三维地震波传播正演
模拟算法


