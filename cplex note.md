# cplex学习笔记

### Day 1

下载地址：https://www.ibm.com/cn-zh/analytics/cplex-optimizer

需要注册 IBM账号

是IBM公司开发的一个优化工具引擎，适合求解线性规划、二次规划、整数规划等问题。

可试用免费版，计算数据量不得超过1000个点。

学术版对于计算数量没有要求，但需要通过邮箱认证。





### Day 2

cplex作为一款比较简单的数值优化求解器，有自己的OPL语言，用于编制模型文件和数据文件。

包含的文件类型：

1.项目文件：组织模型和数据的文件，

2.模型文件：声明数据项目，但是不需要提供数据的初始化工作

3.设置文件：当你决定了一个或多个数学规划和其他的缺省值，该文件将保存用户定义的值

4.运行配置：为了运行的目的而根据项目进行的设置



模型文件包含：①数据②决策变量③目标函数④约束条件。 这也是数学建模的四个必备要素。





### Day 3

#### OPL的主要关键字



| 数据类型           | 语法规则                                                     | 代码示例                                                     |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| string             | {string} 变量名 = {"字符串1", "字符串2","字符串3",...};      | {string} Products = {"gas", "chloride"};{string} Components = {"nitrogen", "hydrogen", "chloride"}; |
| float              | 用来定义浮点数据类型，也就是小数;float 变量名[对应字符串数组变量名] = [数值1, 数值2, ...]; | //浮点数<br/>float var = 3.13;<br/>//数组<br/>float Profit[Products] = [30,40];<br/>//二维数组<br/>float Demands[Products][Components] = [[1,3,0],[1,4,0]]; |
| int                | 用来定义整型数据类型，也就是整数<br/>int 变量名[对应关键字数组数组变量名] = [数值1, 数值2, ...]; | //整型<br/>int Fixed = 10;                                   |
| range              | 一段连续的整型数据<br/>range 变量名 = startInterge..endInterge | //Range类型<br/>range Rows = 1..10;<br/>int n=10;<br/>range rows = (n+1)..2*(n+1); |
| dvar               | 定义决策变量<br/>dvar 数据类型函数 变量名;                   | dvar float+ Gas;<br/>dvar float+ Chloride;<br/>dvar float+ Production[Products]; |
| Maximize、Minimize | maximize或minimize 目标函数的表达式;                         | maximize 40 * Gas + 50 * Chloride;<br/>maximize sum(p in Products) Profit[p] * Production[p]; |
| subject to         | subject to{<br/>约束1名称：约束1;<br/>                       | subject to{<br/>ctMaxTotal: Gas + Chloride <= 50;<br/>ctMaxTotal2: 3 * Gas + 4 * Chloride <= 180;<br/>ctMaxChloride: Chloride <= 40;<br/>} |









