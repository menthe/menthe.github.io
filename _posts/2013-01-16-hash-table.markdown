---
layout: post
title: "哈希表"
date: 2013-01-16 14:27
comments: true
categories: 数据结构
---
###基本概念
哈希表，也叫做散列表，是根据关键码值（key value）进行随机访问的数据结构。也就说，它通过把关键码值映射到表中的一个位置来访问记录，以加快查找的速度。使用的映射函数是Hash函数，存放记录的数组叫做哈希表。

**散列函数**若结构中存在关键字和K相等的记录，则必定存储在f(K)的位置上，由此不需要比较便可以直接取得所要的记录，整个对应关系f就是散列函数，按照整个思想建立的表就是散列表。

**冲突**对于不同的关键字可能得到同一个散列地址，例如K1!=K2,但是f(k1)=f(k2),这种现象叫做冲突。具有相同散列值的关键字对于该散列函数来说叫做**同义词**。
<!--more-->
根据散列函数和处理冲突的方法将一组关键字映射到一个连续的地址集上，并以关键字在地址集中的映射作为这条记录的存放位置，这种表就叫做散列表，这一过程成为散列造表或者散列，所得存储位置成为散列地址。散列表中，只能通过顺序的方式来查找，一般三次就可以找到。

**均匀散列函数**若对于关键字集合中的任一个关键字，经散列函数映射到地址集合中的任一地址的概率是相等的，则称此类散列函数为均匀散列函数，能使得关键字经过散列函数得到一个随机的地址，减少冲突。
###哈希函数的构造方法
散列函数能使对一个数据序列的访问更加迅速，尽可能的具有更好的随机性，减少冲突。构建Hash函数需要考虑的因素有：

-  计算Hash函数所需时间
-  关键字的长度
-  哈希表的大小
-  关键字的分布情况
-  记录的查找频率

**1)直接寻址法：**取关键字或关键字的某个线性函数值为散列地址。即H(key)=key或H(key) = a·key + b，其中a和b为常数（这种散列函数叫做自身函数）。若其中H(key）中已经有值了，就往下一个找，直到H(key）中没有值了，就放进去。

**2)除留余数法：**取关键字k除以哈希表长度m所得余数作为哈希函数地址的方法。即：
H(k)=k％m
这是一种较简单、也是较常见的构造方法。
这种方法的关键是选择好哈希表的长度m。使得数据集合中的每一个关键字通过该函数转化后映射到哈希表的任意地址上的概率相等。
理论研究表明，在m取值为素数（质数）时，冲突可能性相对较少。

**3)平方取中法：** 取关键字平方后的中间几位作为哈希函数地址（若超出范围时，可再取模）。
设有一组关键字ABC，BCD,CDE，DEF，……其对应的机内码如表所示。假定地址空间的大小为1000，编号为0-999。现按平方取中法构造哈希函数，则可取关键字机内码平方后的中间三位作为存储位置。

**4)折叠法：**这种方法适合在关键字的位数较多，而地址区间较小的情况。
将关键字分隔成位数相同的几部分。然后将这几部分的叠加和作为哈希地址（若超出范围，可再取模）。
例如，假设关键字为某人身份证号码430104681015355，则可以用4位为一组进行叠加。即有5355+8101+1046+430=14932，舍去高位。 则有H(430104681015355)=4932 为该身份证关键字的哈希函数地址。

**5)数值分析法：**若事先知道所有可能的关键字的取值时，可通过对这些关键字进行分析，发现其变化规律，构造出相应的哈希函数。
例：对如下一组关键字通过分析可知：每个关键字从左到右的第l，2，3位和第6位取值较集中，不宜作哈希地址。 剩余的第4，5，7和8位取值较分散，可根据实际需要取其中的若干位作为哈希地址。

**6)随机数法：**选择一个随机函数，取关键字的随机函数值为它的哈希地址，即H(key)＝random(key)，其中random为随机函数。

###冲突的解决方法
假设哈希表的地址范围为0～m-l，当对给定的关键字k，由哈希函数H(k)算出的哈希地址为i（0≤i≤m-1）的位置上已存有记录，这种情况就是冲突现象。 处理冲突就是为该关键字的记录找到另一个“空”的哈希地址。即通过一个新的哈希函数得到一个新的哈希地址。如果仍然发生冲突，则再求下一个，依次类推。直至新的哈希地址不再发生冲突为止。

常用的处理冲突的方法有开放地址法、链地址法两大类：

####开放地址法
用开放定址法处理冲突就是当冲突发生时，形成一个地址序列。沿着这个序列逐个探测，直到找出一个“空”的开放地址。将发生冲突的关键字值存放到该地址中去。
如 Hi=(H(k)+d（i）) % m, i=1，2，…k (k 其中H(k)为哈希函数，m为哈希表长，d为增量函数，d(i)=dl，d2…dn-l。
增量序列的取法不同，可得到不同的开放地址处理冲突探测方法。

**1)线性探测法：**线性探测法是从发生冲突的地址（设为d）开始，依次探查d+l，d+2，…m-1（当达到表尾m-1时，又从0开始探查）等地址，直到找到一个空闲位置来存放冲突处的关键字。
若整个地址都找遍仍无空地址，则产生溢出。
线性探查法的数学递推描述公式为：
	d0=H(k)
	di=(di-1+1)% m (1≤i≤m-1)

**平方查找法：** 设发生冲突的地址为d，则平方探查法的探查序列为：d+12，d+22，…直到找到一个空闲位置为止。
平方探查法的数学描述公式为：
	d0=H(k)
	di=(d0+i2) % m (1≤i≤m-1)

####链地址法
用链地址法解决冲突的方法是：把所有关键字为同义词的记录存储在一个线性链表中，这个链表称为同义词链表。并将这些链表的表头指针放在数组中（下标从0到m-1）。这类似于图中的邻接表和树中孩子链表的结构。
