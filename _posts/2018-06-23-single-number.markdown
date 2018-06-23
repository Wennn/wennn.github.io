---
layout: post
title:  "只出现一次的数字"
date:   2018-06-23 22:15:00 +0800 
categories: exam
---
### 题目
&emsp;&emsp;
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>输入：[2,２,1]  
>输出：１

### 整体思路
&emsp;&emsp;
由于题目中已经说明了该数组中仅存在一个只出现了一次的元素。因此可以利用数学方式，也就是异或，来解决这一问题。基于异或的特性x ^ 0 = x且x ^ x = 0，只要将数组中的元素逐个进行异或，那么其它出现了两次的元素最终计算得到0，再与剩下的一个元素进行异或即可得到结果。


### 代码实现
```java
public int solution(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

### 结语
&emsp;&emsp;
这道题目同样是一道非常基础的题目。只要能想到异或这一技巧，编码就非常简单了。  
&emsp;&emsp;
如果您有任何建议或看法，十分欢迎您通过[Github Issues](https://github.com/Wennn/wennn.github.io/issues)为我指正。