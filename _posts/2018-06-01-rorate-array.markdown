---
layout: post
title:  "旋转数组"
date:   2018-06-21 23:00:00 +0800
categories: exam
---
## 题目
&emsp;&emsp;
给定一个数组，将数组中的元素向右移动k个位置，其中k是非负数。例如：
>输入：[1,2,3,4,5]，k=2  
>输出：[4,5,1,2,3]

## 整体思路
&emsp;&emsp;
关于这道题目，主要有四种解决方法。  
&emsp;&emsp;
最简单的是通过暴力解决，即将数组中的每一个元素依次向右移动一位，执行k次。这样的时间复杂度为O(n * k)，空间复杂度为O(1)。虽然可以通过（k = k % 数组长度）这样的操作来降低k的数值，但是当n、k都比较大时，这样的时间很可能是无法接受的。  
&emsp;&emsp;
第二种方法是借助额外的空间，即创建一个与输入数组等长的数组，并遍历输入数组，依次将元素放置在新数组中移动后的位置上。这样的时间复杂度为O(n)，空间复杂度也为O(n)。  
&emsp;&emsp;
第三种方法是使用了循环替代。简单的说就是从第一个元素开始，按照k的步长依次执行向后替换过程，直到回到起始位置，此时以该元素开启的这一链条上的元素都被置于正确的位置上；执行完一轮之后，如果替换次数小于数组长度，则再从第二个元素开始……以此类推，直至每一个元素都替换过，即替换次数等于数组长度。这一思路的时间复杂度为O(n)，空间复杂度为O(1)。对于这一思路的证明过程可以参考[Rotate Array](https://leetcode.com/articles/rotate-array/#)一文中的相应段落。  
&emsp;&emsp;
第四种方法使用了三次反转实现。据说在旋转字符串中也有类似的解法（[参考](https://blog.csdn.net/geekmanong/article/details/50913518)）。  
&emsp;&emsp;
对于左旋转，其反转过程为：
1. 反转前k个元素
2. 反转后面n-k个元素
3. 反转所有元素

&emsp;&emsp;
而对于本题要求的右旋转，反转过程则刚好相反：
1. 反转所有元素
2. 反转前k个元素
3. 反转后n-k个元素

&emsp;&emsp;
这一思路的时间复杂度依然为O(n)，空间复杂度为O(1)。
## 代码实现
&emsp;&emsp;
这里主要给出后面两种思路的代码实现。  
&emsp;&emsp;
第三种思路：
```java
public void solution(int[] nums, int k) {
    k = k % nums.length;
    int count = 0;

    for (int start = 0; count < nums.length; start++) {
        int current = start;
        int prev = nums[current];
        do {
            int next = (current + k) % nums.length;
            int temp = nums[next];
            nums[next] = prev;
            prev = temp;

            current = next;
            count++;
        } while (current != start);
    }
}
```
&emsp;&emsp;
第四种思路：
```java
private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
public void solution(int[] nums, int k) {
    k = k % nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}
```
## 结语
&emsp;&emsp;
数组旋转是数组的一种基础操作。考虑到时间复杂度以及数组考察中常见的原地算法的要求，后面两种思路就显得更加值得思考。  
&emsp;&emsp;
如果您有任何建议或看法，十分欢迎您通过[Github Issues](https://github.com/Wennn/wennn.github.io/issues)为我指正。