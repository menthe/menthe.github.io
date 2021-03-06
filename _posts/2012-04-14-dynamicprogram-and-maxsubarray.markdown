---
layout: post
title: "动态规划与最大子序列"
date: 2012-04-14 00:57
comments: true
categories: 算法
---
今天有两个同学去参加腾讯的笔试，回来和我说有一个动态规划的问题。大学四年学得最少的就是算法了，马上要考研了，突然对这个算法很感兴趣，于是就网上看了一点资料。基本的资料都来自于<a href="http://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92" target="_blank">维基百科</a>，总结之后大致是动态规划是用来求解含有重叠子问题的最优化的问题。它的基本思想是把原问题分解为相似的子问题，通过求解子问题还原出原问题的答案。

一般要经历如下的步骤：
<ol>
	<li>最优子结构性质。如果问题的最优解所包含的子问题的解也是最优的，我们就称该问题具有最优子结构性质（即满足最优化原理）。最优子结构性质为动态规划算法解决问题提供了重要线索。</li>
	<li>子问题重叠性质。子问题重叠性质是指在用递归算法自顶向下对问题进行求解时，每次产生的子问题并不总是新问题，有些子问题会被重复计算多次。动态规划算法正是利用了这种子问题的重叠性质，对每一个子问题只计算一次，然后将其计算结果保存在一个表格中，当再次需要计算已经计算过的子问题时，只是在表格中简单地查看一下结果，从而获得较高的效率。</li>
</ol>
下面就来说说最大子序列的问题：所谓的最大子序列就是说在一个即存在正数又存在负数的序列里面，求出一段连续的且是和最大的子序列。这个问题可以可以动态规划的思想解决:设 b[i]表示以第 i 个元素 a[i]结尾的最大子序列,那么显然b[i+1]=b[i]&gt;0?b[i]+a[i+1]。说白了就要是要判断当前子串的下一个字符是正是负，如果是负的话与前面子串的和相加之后是正是否，如果是正的话，当然可以继续往后继续判断的，这个子串也是有用的，如果是负的话，显然要抛弃。

Java代码实现如下：
<pre>package com.softrayn.maxsubarray;

public class MaxSubArray {
    public int maxSubArr(int[] arr) {
        int length = arr.length;
        int max = arr[0];
        int b = arr[0];
        for (int i = 0; i &lt; length; i++) {
            if (b &gt; 0) {
                b += arr[i];
            } else {
                b = arr[i];
            }
            if (b &gt; max) {
                max = b;
            }
        }
        return max;
    }
}</pre>
在main方法里面调用一下就好了。代码就不贴出来了。