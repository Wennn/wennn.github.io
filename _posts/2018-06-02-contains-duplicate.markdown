---
layout: post
title:  "存在重复"
date:   2018-06-22 23:55:00 +0800
categories: exam
---
### 题目
&emsp;&emsp;
给定一个整数数组，判断是否存在重复元素。例如：
>输入：[1,2,3,1]  
>输出：true

### 整体思路
&emsp;&emsp;
最简单的思路依然是通过暴力求解，即将数组中所有元素两两比较，判断是否存在重复。在一个有n个元素的数组中，两两成对的组合共有(n-1)*n/2种可能，因此这一思路的时间复杂度即为O(n<sup>2</sup>)。  
&emsp;&emsp;
第二种思路则是将数组进行排序，排序后相同的元素便会相邻。这样只需要再遍历一遍数组即可找到是否存在重复元素。这一思路时间复杂度即排序的时间复杂度，也就是O(n log n)。  
&emsp;&emsp;
第三种思路则更为直接，借助Hash的方式即可快速定位到数据结构中存储的元素。因此，只需要将数组中的元素依次加入到一个实现了Hash的动态数据结构（例如HashSet）中，每次加入元素之前判断该元素是否已经存在，即可判断数组中是否存在重复元素。由于Hash数据结构的检索可以视为常数，因此这一思路的时间复杂度就是遍历一遍数组元素的时间复杂度，即O(n)。

### 代码实现
&emsp;&emsp;
由于暴力求解极易超时，因此本文主要考察后两种思路的实现方式。  
&emsp;&emsp;
借助Java语言的库函数，第二种思路的实现为：
```java
public boolean solution(int[] nums) {
    Arrays.sort(nums);
    for (int i = 0; i < nums.length-1; i++) {
        if (nums[i] == nums[i+1]) {
            return true;
        }
    }

    return false;
}
```
&emsp;&emsp;
需要说明的是，jdk 1.8中Arrays.sort使用了所谓的双轴快速排序（DualPivotQuicksort），具体实现暂时不再展开，有兴趣的可以参考[相关内容](https://stackoverflow.com/questions/20917617/whats-the-difference-of-dual-pivot-quick-sort-and-quick-sort)。  
&emsp;&emsp;
同样，借助Java提供的HashSet数据结构，第三种思路的实现为：
```java
public boolean solution(int[] nums) {
    Set<Integer> container = new HashSet<>();
    for (int num : nums) {
        if (container.contains(num)) {
            return true;
        }
        container.add(num);
    }
    return false;
}
```
&emsp;&emsp;
jdk 1.8中的HashSet实质上是利用HashMap实现的。在HashSet中持有一个静态Object对象PRESENT，每当有新元素add时，便向HashMap中put一个key为新元素、value为PRESENT的对象，借此来保证数据结构中元素的唯一性以及O(1)的检索时间。
### 结语
&emsp;&emsp;
这道题目是一道非常基础的题目。在这样的场景下，能够灵活地使用数据结构是解决问题的关键。  
&emsp;&emsp;
如果您有任何建议或看法，十分欢迎您通过[Github Issues](https://github.com/Wennn/wennn.github.io/issues)为我指正。