---
layout: post
title:  "字符串-菜鸟级题目"
date:   2018-06-26 20:11:00 +0800 
categories: exam
---
- 这是一个目录
{:toc}

### 反转字符串
#### 题目
请编写一个函数，其功能是将输入的字符串反转过来。例如：
>输入：s = "hello"，返回："olleh"  

#### 整体思路
这道题目可以说是很基础的一道题目了。考虑到Java中String的特殊性，可以先转成char array再进行操作。

#### 代码实现
```java
public String solution(String s) {
    if (s == null) return null;
    char[] ss = s.toCharArray();
    for (int i = 0; i < ss.length / 2; i++) {
        char c = ss[i];
        ss[i] = ss[ss.length - 1 - i];
        ss[ss.length - 1 - i] = c;
    }
    return new String(ss);
}
```
#### 小结
由于Java中String的不可变性，可以先调用toCharArray方法再进行正常的反转操作。

### 颠倒整数
#### 题目
给定一个32位有符号整数，将整数中的数字进行反转。例如：
>输入：123，输出：321  
>输入：-123，输出：-321  
>输入：130，输出：31  

假设我们的环境只能存储32位有符号整数，其数值范围是 [−2<sup>31</sup>,  2<sup>31</sup> − 1]。根据这个假设，如果反转后的整数溢出，则返回0。
#### 整体思路
单纯的整数反转并不复杂，但是题目中明示了会出现的溢出的情况，且在出现溢出时需要返回0，因此我们需要在迭代的过程中加入溢出的判断。根据整数的最大最小值除以10与当前结果的比较可以判断是否即将出现溢出的情况。而在这里，我们将采用另外一种方式判断，也就是当前结果是否能还原至上一步的结果。很显然，如果发生了溢出，那么当前结果经过相反操作与上一步的结果是不相等的，此时返回0即可。
#### 代码实现
```java
public int solution(int x) {
    int result = 0, new_result = 0;
    while (x != 0) {
        new_result = result * 10 + x % 10;
        if ((new_result - x % 10) / 10 != result) {
            return 0;
        }
        result = new_result;
        x /= 10;
    }

    return new_result;
}
```
#### 小结
这一题目尽管被放在了字符串目录下面，但是似乎与字符串关系并不大，更多的是通过整数计算完成的。

### 字符串中的第一个唯一字符
#### 题目
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回-1。注意事项：您可以假定该字符串只包含小写字母。例如：
>s = "leetcode"，返回0  
>s = "loveleetcode"，返回2

#### 整体思路
最简单的做法就是遍历两遍，首先第一遍获取每一个字母出现的次数，并在第二遍时检查每一个元素出现的次数是否为1，如果是则直接返回index。
#### 代码实现
```java
public int solution(String s) {
    int[] freq = new int[26];
    char[] ss = s.toCharArray();

    for (char c : ss) {
        freq[c - 'a']++;
    }

    for (int i = 0; i < ss.length; i++) {
        if (freq[ss[i] - 'a'] == 1) {
            return i;
        }
    }
    return -1;
}
```
#### 小结
在这里利用了题干中提到的“仅包含小写字母”的特点。如果字符的范围扩大的话，HashMap可能就会变成一个更优的数据结构。
### 有效的字母异位词
#### 题目
给定两个字符串s和t，编写一个函数来判断t是否是s的一个字母异位词。说明:你可以假设字符串只包含小写字母。例如：
>输入：s = "anagram"、t = "nagaram"，输出：true  
>输入：s = "rat"、t = "car"，输出：false

#### 整体思路
这一题目的本质就是验证两个字符串所包含的元素是否完全一致。那么显然有两种思路：其一是进行排序，然后比较排序后的两个字符数组对应位置上的元素是否一致；另一种则是使用数据结构保存下每个元素的出现次数，再进行对比。为了不局限在“小写字母”这一假设当中，本文给出了基于HashMap的第二种思路的代码实现。
#### 代码实现
```java
public boolean solution(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    HashMap<Character, Integer> charMap = new HashMap<>();
    for (char c : s.toCharArray()) {
        if (charMap.containsKey(c)) {
            charMap.put(c, charMap.get(c)+1);
        } else {
            charMap.put(c, 1);
        }
    }

    for (char c : t.toCharArray()) {
        if (charMap.containsKey(c)) {
            if (charMap.get(c) == 0) {
                return false;
            }
            charMap.put(c, charMap.get(c)-1);
        } else {
            return false;
        }
    }

    for (Character k : charMap.keySet()) {
        if (charMap.get(k) != 0) {
            return false;
        }
    }

    return true;
}
```
#### 小结
这一题目虽然比较的是两个字符串，但是其实现思路依然是基于数组的操作。

### 验证回文字符串
#### 题目
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。说明：本题中，我们将空字符串定义为有效的回文串。例如：
>输入: "A man, a plan, a canal: Panama"，输出: true  
>输入: "race a car"，输出: false  

#### 整体思路
只要将字母与数字的元素抽出来，然后模仿字符串反转的步骤即可判断。本文给出了最为朴素的一种实现方式。
#### 代码实现
```java
public boolean solution(String s) {
    if (s.length() < 2) {
        return true;
    }

    ArrayList<Character> charArr = new ArrayList<>();
    for (char c : s.toLowerCase().toCharArray()) {
        if ((c >= 'a' && c <= 'z') || (c >= '0' && c <= '9')) {
            charArr.add(c);
        }
    }

    for (int i = 0; i < charArr.size() / 2; i++) {
        if (!charArr.get(i).equals(charArr.get(charArr.size()-i-1))) {
            return false;
        }
    }

    return true;
}
```
#### 小结
依然是基于数组的操作。当然也可以考虑原地方法，即声明头尾两指针，分别移动至下一个数字/字母元素处进行比较。

### 字符串转整数（atoi）
#### 题目
实现atoi，将字符串转为整数。在找到第一个非空字符之前，需要移除掉字符串中的空格字符。如果第一个非空字符是正号或负号，选取该符号，并将其与后面尽可能多的连续的数字组合起来，这部分字符即为整数的值。如果第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。字符串可以在形成整数的字符后面包括多余的字符，这些字符可以被忽略，它们对于函数没有影响。当字符串中的第一个非空字符序列不是个有效的整数；或字符串为空；或字符串仅包含空白字符时，则不进行转换。若函数不能执行有效的转换，返回0。说明：假设我们的环境只能存储32位有符号整数，其数值范围是[−2<sup>31</sup>,2<sup>31</sup>−1]。如果数值超过可表示的范围，则返回INT_MAX(2<sup>31</sup>−1)或INT_MIN(−2<sup>31</sup>)。例如：
>输入:"42"，输出:42  
>输入:"   -42"，输出:-42  
>输入:"4193 with words"，输出:4193  
>输入:"words and 987"，输出:0  
>输入:"-91283472332"，输出:-2147483648  

#### 整体思路
atoi应该是字符串的一道经典题目。对于本题的要求，其关键点就在于正确的找到起始位置、正确地处理正负号以及溢出情况的结果返回。总体思路就是耐心地依次访问字符元素，逐个进行处理。

#### 代码实现
```java
public int solution(String str) {
    char[] s = str.toCharArray();
    int i = 0, ret = 0, sig = 1;

    while (i < s.length && s[i] == ' ') {
        i++;
    }

    if (i >= s.length) {
        return 0;
    }

    if (s[i] == '+' || s[i] == '-') {
        sig = s[i] == '-' ? -1 : 1;
        i++;
    }

    for (; i < s.length; i++) {
        if (s[i] < '0' || s[i] > '9') {
            break;
        }

        if (ret > Integer.MAX_VALUE / 10 || (ret == Integer.MAX_VALUE / 10 && s[i] - '0' > 7)) {
            return sig < 0 ? Integer.MIN_VALUE : Integer.MAX_VALUE;
        }

        ret = ret * 10 + (s[i] - '0');
    }

    return sig * ret;
}
```
#### 小结
思路并不复杂，只要想清楚对于数组边界判断、溢出、空格以及正负号的处理即可。

### 实现strStr()
#### 题目
实现strStr()函数。给定一个haystack字符串和一个needle字符串，在haystack字符串中找出needle字符串出现的第一个位置(从0开始)。如果不存在，则返回-1。当needle是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。对于本题而言，当needle是空字符串时我们应当返回0。这与C语言的strstr()以及Java的indexOf()定义相符。例如：
>输入：haystack = "hello", needle = "ll"  
>输出：2  

#### 整体思路
最简单的思路就是在haystack中找到needle字符串的首字母所在的位置，之后再依次向后匹配。除此之外，还有一种字符串匹配的高级方法，KMP算法。这一算法过程较为晦涩，本文不再展开阐述。下面给出的就是基于最朴素思路的代码实现。
#### 代码实现
```java
public int solution(String haystack, String needle) {
    for (int i = 0; ; i++) {
        for (int j = 0; ; j++) {
            if (j == needle.length()) {
                return i;
            }
            if (i + j == haystack.length()) {
                return -1;
            }
            if (needle.charAt(j) != haystack.charAt(i + j)) {
                break;
            }
        }
    }
}
```

#### 小结
在高级方法不容易掌握的情况下，朴素的思路也可以完成相应的操作。

### 数数并说
#### 题目
报数序列是指一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：
1. 1
2. 11
3. 21
4. 1211
5. 111221

1被读作"one 1"("一个一"),即11。
11被读作"two 1s"("两个一"）,即21。
21被读作"one 2","one 1"（"一个二","一个一"),即1211。  
给定一个正整数n，输出报数序列的第n项。注意：整数顺序将表示为一个字符串。例如：
>输入：3，输出：21

#### 整体思路
似乎没有什么取巧的办法，只能是依次算出来。
#### 代码实现
```java
public String solution(int n) {
    String res = "1";

    while (n > 1) {
        String cur = "";
        for (int i = 0; i < res.length(); i++) {
            int count = 1;
            while (i + 1 < res.length() && res.charAt(i) == res.charAt(i + 1)) {
                i++;
                count++;
            }
            cur += String.valueOf(count) + res.charAt(i);
        }
        res = cur;
        n--;
    }

    return res;
}
```
#### 小结
对于这种没有什么计算规律的序列，最朴素的思路，也就是依次推导，往往是解决问题的思路。

### 最长公共前缀
#### 题目
编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串""。例如：
>输入：["flower","flow","flight"]，输出："fl"  
>输入：["dog","racecar","car"]，输出：""  

#### 整体思路
关于此题有很多思路（可[参考](https://leetcode.com/problems/longest-common-prefix/solution/#)）。本文采用了一个最直观的思路，也就是纵向比较。简单地说就是依次比较所有字符串元素的第N个元素，直到该位置上的元素不全都相同，则返回前面的元素作为公共前缀。

#### 代码实现
```java
public String solution(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }

    for (int i = 0; i < strs[0].length(); i++) {
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j++) {
            if (i == strs[j].length() || c != strs[j].charAt(i)) {
                return strs[0].substring(0, i);
            }
        }
    }
    return strs[0];
}
```

#### 小结
同样是没有什么讨巧的办法，但是可以参考leetcode上这篇[讲解](https://leetcode.com/problems/longest-common-prefix/solution/#)获得更多思路的灵感。


### Finally
与数组不同，字符串更多的是一种输入输出的形式。而在操作思路上面，一部分可以借鉴数组的处理模式，另一部分无法计算而只能推导的题目则需要耐心摸清环节，逐步进行推导来得到结果。  
如果您有任何建议或看法，十分欢迎您通过[Github Issues](https://github.com/Wennn/wennn.github.io/issues)为我指正。