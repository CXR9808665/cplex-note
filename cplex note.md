---
typora-root-url: picture
---

# cplex学习笔记

### Day 1

下载地址：https://www.ibm.com/cn-zh/analytics/cplex-optimizer

需要注册 IBM账号

是IBM公司开发的一个优化工具引擎，适合求解线性规划、二次规划、整数规划等问题。

可试用免费版，计算数据量不得超过1000个点。

学术版对于计算数量没有要求，但需要通过邮箱认证。





### Day 2

cplex作为一款比较简单的数值优化求解器，有自己的OPL语言，用于编制模型文件和数据文件。

#### cplex包含的文件类型

1.项目文件：组织模型和数据的文件，

2.模型文件：声明数据项目，但是不需要提供数据的初始化工作

3.设置文件：当你决定了一个或多个数学规划和其他的缺省值，该文件将保存用户定义的值

4.运行配置：为了运行的目的而根据项目进行的设置



模型文件包含：①数据②决策变量③目标函数④约束条件。 这也是数学建模的四个必备要素。



Cplex的编程步骤

1.正确地表达清楚数学规划模型。

2.确定模型中已知量、未知量（**类型**，**范围**等）。

3.编写程序。





### Day 3

#### OPL的主要关键字



| 数据类型           | 语法规则                                                     | 代码示例                                                     |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| string             | {string} 变量名 = {"字符串1", "字符串2","字符串3",...};      | {string} Products = {"gas", "chloride"};{string} Components = {"nitrogen", "hydrogen", "chloride"}; |
| float              | 用来定义浮点数据类型，也就是小数;float 变量名[对应字符串数组变量名] = [数值1, 数值2, ...]; | //浮点数<br/>float var = 3.13;<br/>//数组<br/>float Profit[Products] = [30,40];<br/>//二维数组<br/>float Demands[Products][Components] = [[1,3,0],[1,4,0]]; |
| int                | 用来定义整型数据类型，也就是整数<br/>int 变量名[对应关键字数组数组变量名] = [数值1, 数值2, ...]; | //整型<br/>int Fixed = 10;                                   |
| range              | 一段连续的整型数据<br/>range 变量名 = startInterge..endInterge | //Range类型<br/>range Rows = 1..10;<br/>int n=10;<br/>range rows = (n+1)..2*(n+1); |
| boolean            | 定义布尔型变量                                               |                                                              |
| dvar               | 定义决策变量<br/>dvar 数据类型函数 变量名;                   | dvar float+ Gas;<br/>dvar float+ Chloride;<br/>dvar float+ Production[Products]; |
| Maximize、Minimize | maximize或minimize 目标函数的表达式;                         | maximize 40 * Gas + 50 * Chloride;<br/>maximize sum(p in Products) Profit[p] * Production[p]; |
| subject to         | subject to{<br/>约束1名称：约束1;<br/>                       | subject to{<br/>ctMaxTotal: Gas + Chloride <= 50;<br/>ctMaxTotal2: 3 * Gas + 4 * Chloride <= 180;<br/>ctMaxChloride: Chloride <= 40;<br/>} |



eg 求解模型1  简单的线性规划问题

min  z = 2x+3y,

s.t. 2x+3y≥20，

x+y≥10，

x,y≥0且为整数。

```OPL
/*********************************************
 * OPL 12.10.0.0 Model
 * Author: CXR
 * Creation Date: 2021年8月4日 at 下午9:51:35
 *********************************************/
dvar int+ x;  /*int表示整型,int+表示正数*/
dvar int+ y;
minimize 2 * x + 3 * y;
subject to{
  2*x+3*y>=20;
  x+y>=0;
}
```

运行结果：

<img src="/picture/2x+3y.png" style="zoom:50%;" />

### Day 4

以上的变量声明方式没有使用**集合语言**，

下面介绍一些使用集合语言声明的方式（eg.sum,forall,...)

#### Cplex常用集合语言

1.多个式子求和：

a1x1+a2x2+...+anxn=Σajxj(j=1 to n).

代码  `sum(j in 1 .. n)a[j]*x[j];`



2.同种形式的多个约束条件：

x1≤b1

x2≤b2                         **xj≤bj，j=1，2...n**

...

xn≤bn

代码：     `forall(j in 1 .. n)   x[j]≤b[j];`





eg.2 求解模型 2     01背包问题

有n件物品和一个容量为C的背包，第i件物品消耗的容量为wi，价值为pi，求解放入哪些物品可以使得背包中总价值最大。

本问题中，n,C,wi,pi均为已知量。

定义**变量**  xj, 其中：

<img src="/picture/backpack problem.jpg" style="zoom:50%;" />

```OPL
/*********************************************
 * OPL 12.10.0.0 Model
 * Author: CXR
 * Creation Date: 2021年8月8日 at 上午10:51:10
 *********************************************/
int n = 4;
int C = 13;
int p[1..n] = [12,11,9,8];
int w[1..n] = [8,6,4,3];      /*定义已知量 */

dvar boolean x[1..n];    //定义布尔型变量xi（决策变量）

maximize sum(j in 1..n) p[j]* x[j];    //利用集合语言定义目标函数

subject to{
  sum(j in 1..n) w[j] * x[j] <=C;  //利用集合语言定义约束条件
}
```

运行结果：

<img src="/picture/01backpack_solve.png" style="zoom:50%;" />





### Day 5

