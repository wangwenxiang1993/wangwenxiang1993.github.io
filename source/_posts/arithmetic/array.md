---
layout: post
title: 根据上排给出十个数，在其下排填出对应的十个数
date: 2017/02/16
tags: [arithmetic, index]
---

## 题目要求：
##### 给你10分钟时间，根据上排给出十个数，在其下排填出对应的十个数,要求下排每个数都是先前上排那十个数在下排出现的次数。
##### 上排的十个数如下：【0，1，2，3，4，5，6，7，8，9】
<!--more-->

### 举个例子，
##### 上排数值: 0,1,2,3,4,5,6,7,8,9
##### 下排数值: 6,2,1,0,0,0,1,0,0,0
##### 0在下排出现了6次，1在下排出现了2次，
##### 2在下排出现了1次，3在下排出现了0次...

## 分析过程（不一定正确）：
##### 看到题目的时候觉得有一点绕，切没有太理解题目，以为是一直循环输出下排数组，于是有了下面的解题过程。
### 根据题目得到的一些条件：
1. 10个数，每个数都大于等于0；
2. 下排的数字为上排数字出现的次数；
3. 基于条件2，下排数字相加一定等于10；
4. 基于条件3，下排数字有0，要么下排全等于1，很明显全等于1就不符合条件2，所以下排数字一定有0；
5. 根据条件4，上排数字也有0；

#### 基于上面几个条件，做出下面的一些假设：
##### 上排的10个数中，有n个数字为0，则非0的数有10-n：
$$ \underbrace{0...0}_{\text{n个0}},\overbrace{v_1...v_i}^{\text{10-n个非0数字}} $$
##### 如果上排为0的下排也为0，显然不符合条件2，所以0的下排数字为非0，切所有0的下排数字相等，假设为非0数x,则上排一定有一个数字为x,对应的下排数字为n：
$$ 上排：0...0,v_1...v_i,x $$
$$ 下排：\underbrace{x...x}_{\text{n个x}},i_1...i_i,n $$
##### 上排的0在下排为x，则说明下排有x个0,进一步得出下面的组合：
$$ 上排：0...0,v_1...v_i,v_j...v_k,x $$
$$ 下排：\underbrace{x...x}_{\text{n个x}},\underbrace{0....0}_{\text{x个0}},i_i....i_k,n $$
##### 根据条件1，下排一共10个数，所以：
$$ n+x+1≤10 $$
##### 根据条件3，下排数字相加等于10，所以：
$$ nx+n≤10 $$
##### 当v<sub>j</sub>到v<sub>k</sub>不存在，也就是个数为0时，上面两个不等式的等号成立，所以有以下两个函数：
$$\begin{cases}
n+x+1=10 \\\\
nx+n=10 \\\\
x,n 为 正整数
\end{cases}
\implies
\begin{cases}
n+x+1<10 \\\\
nx+n<10 \\\\
x,n 为 正整数
\end{cases}
$$
##### 因为x，n均为正整数，所以枚举所有情况如下：
$$
n=\begin{cases}
[1,2,3,4] &x=1 \\\\
[1,2,3] &x=2 \\\\
[1,2,] &x=3 \\\\
[1] &x=4 \\\\
[1] &x=5 \\\\
[1] &x=6 \\\\
[1] &x=7 \\\\
[1] &x=8
\end{cases}
$$
##### 于是我基于这种组合，将6,2,1,0,0,0,1,0,0,0默认为上排函数，发现n=6,计算不出下排函数。百度以后发现，人家根本不是按照我的这种方式去计算。下面说一下正确的解题思路.

## 解题：
##### 它的原型跟八皇后有点类似，都是用回溯递归的方法去一次一次尝试，直到找出正确解。
##### 具体的想法是：不断的去从下排数组中捉取在上排数组中对应位置中出现的个数，如果个数不对就更新下排数组中对应的值，只到找到正确值。（下排数组先初始为任意值）
##### 如：
##### 上排数组A：0,1,2,3,4,5,6,7,8,9
##### 下排数组B：0,1,2,3,4,5,6,7,8,9
##### 从上牌数组Index = 0开始，A[0] = 0，0在下排数组中的个数为1，那么下排数组B此时就要更新为：1,1,2,3,4,5,6,7,8,9
##### Index = 1, A[1] = 1, 1在下排数组中的个数为2，那么下排数组B此时就要更新为：1,2,2,3,4,5,6,7,8,9，从此不断的往下进行，只要找不到正确值就一直往下进行，如果Index >= 数组长度时，那么重新恢复Index = 0再往下进行测试直到找出正确解。

## 最后再说一些：
##### 其实按照上面的这种解题思路，我的解题过程也不能算是错误的，只是排除了大部分的可能性。并且题目也理解有误，所以以后做题一定要看清楚题，理解题目的正真含义。