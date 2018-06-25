---
layout: post
title:  "数组-菜鸟级题目"
date:   2018-06-23 22:15:00 +0800 
categories: exam
---
- 这是一个目录
{:toc}

### 从排序数组中删除重复项
#### 题目
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。例如：
>给定数组nums = [1,1,2], 函数应该返回新的长度2, 并且原数组nums的前两个元素被修改为1, 2。不需要考虑数组中超出新长度后面的元素。

#### 整体思路
由于是排好序的数组，那么相同的元素排在一起。那么只需要两个指针，一个用来遍历数组元素，另一个用来标识新数组的尾巴。

#### 代码实现
```java
public int solution(int[] nums) {
    if (nums.length < 2) {
        return nums.length;
    }

    int len = 1;
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] != nums[i-1]) {
            nums[len] = nums[i];
            len++;
        }
    }

    return len;
}
```
#### 小结
由于题目中明确要求了“原地”，那么用两个指针分别操作就是比较容易想到的思路了。

### 买卖股票的最佳时机 II
#### 题目
给定一个数组，它的第i个元素是一支给定股票第i天的价格。设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（可以多次买卖该支股票）。例如：
>输入：[7,1,5,3,6,4]，输出：7  
>输入：[7,6,4,3,1]，输出：0

#### 整体思路
由于可以进行多次买卖，那么只要这一天的价格高于前一天，我们就应该进行交易，来获得所有利润。不过，如果我们真的能预知股票价格的话，就不会有人赔钱了吧...
#### 代码实现
```java
public int solution(int[] nums) {
    int profile = 0;
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] > nums[i-1]) {
            profile += nums[i] - nums[i-1];
        }
    }
    return profile;
}
```
#### 小结
尽管看上去很像，但是这道题目并不是一道适用动态规划来解决的题目。因为你可以进行多次交易并已知道了股票的全部价格，那么只要将所有的利润都获得到即是最大利润。

### 旋转数组
#### 题目

给定一个数组，将数组中的元素向右移动k个位置，其中k是非负数。例如：
>输入：[1,2,3,4,5]，k=2  
>输出：[4,5,1,2,3]

#### 整体思路

关于这道题目，主要有四种解决方法。  

最简单的是通过暴力解决，即将数组中的每一个元素依次向右移动一位，执行k次。这样的时间复杂度为O(n * k)，空间复杂度为O(1)。虽然可以通过（k = k % 数组长度）这样的操作来降低k的数值，但是当n、k都比较大时，这样的时间很可能是无法接受的。  

第二种方法是借助额外的空间，即创建一个与输入数组等长的数组，并遍历输入数组，依次将元素放置在新数组中移动后的位置上。这样的时间复杂度为O(n)，空间复杂度也为O(n)。  

第三种方法是使用了循环替代。简单的说就是从第一个元素开始，按照k的步长依次执行向后替换过程，直到回到起始位置，此时以该元素开启的这一链条上的元素都被置于正确的位置上；执行完一轮之后，如果替换次数小于数组长度，则再从第二个元素开始……以此类推，直至每一个元素都替换过，即替换次数等于数组长度。这一思路的时间复杂度为O(n)，空间复杂度为O(1)。对于这一思路的证明过程可以参考[Rotate Array](https://leetcode.com/articles/rotate-array/#)一文中的相应段落。  

第四种方法使用了三次反转实现。据说在旋转字符串中也有类似的解法（[参考](https://blog.csdn.net/geekmanong/article/details/50913518)）。  

对于左旋转，其反转过程为：
1. 反转前k个元素
2. 反转后面n-k个元素
3. 反转所有元素


而对于本题要求的右旋转，反转过程则刚好相反：
1. 反转所有元素
2. 反转前k个元素
3. 反转后n-k个元素


这一思路的时间复杂度依然为O(n)，空间复杂度为O(1)。
#### 代码实现

这里主要给出后面两种思路的代码实现。  

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
#### 小结

数组旋转是数组的一种基础操作。考虑到时间复杂度以及数组考察中常见的原地算法的要求，后面两种思路就显得更加值得思考。
### 存在重复
#### 题目

给定一个整数数组，判断是否存在重复元素。例如：
>输入：[1,2,3,1]  
>输出：true

#### 整体思路

最简单的思路依然是通过暴力求解，即将数组中所有元素两两比较，判断是否存在重复。在一个有n个元素的数组中，两两成对的组合共有(n-1)*n/2种可能，因此这一思路的时间复杂度即为O(n<sup>2</sup>)。  

第二种思路则是将数组进行排序，排序后相同的元素便会相邻。这样只需要再遍历一遍数组即可找到是否存在重复元素。这一思路时间复杂度即排序的时间复杂度，也就是O(n log n)。  

第三种思路则更为直接，借助Hash的方式即可快速定位到数据结构中存储的元素。因此，只需要将数组中的元素依次加入到一个实现了Hash的动态数据结构（例如HashSet）中，每次加入元素之前判断该元素是否已经存在，即可判断数组中是否存在重复元素。由于Hash数据结构的检索可以视为常数，因此这一思路的时间复杂度就是遍历一遍数组元素的时间复杂度，即O(n)。

#### 代码实现

由于暴力求解极易超时，因此本文主要考察后两种思路的实现方式。  

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

需要说明的是，jdk 1.8中Arrays.sort使用了所谓的双轴快速排序（DualPivotQuicksort），具体实现暂时不再展开，有兴趣的可以参考[相关内容](https://stackoverflow.com/questions/20917617/whats-the-difference-of-dual-pivot-quick-sort-and-quick-sort)。  

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

jdk 1.8中的HashSet实质上是利用HashMap实现的。在HashSet中持有一个静态Object对象PRESENT，每当有新元素add时，便向HashMap中put一个key为新元素、value为PRESENT的对象，借此来保证数据结构中元素的唯一性以及O(1)的检索时间。
#### 小结

这道题目是一道非常基础的题目。在这样的场景下，能够灵活地使用数据结构是解决问题的关键。  
### 只出现一次的数字
#### 题目

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>输入：[2,２,1]  
>输出：１

#### 整体思路

由于题目中已经说明了该数组中仅存在一个只出现了一次的元素。因此可以利用数学方式，也就是异或，来解决这一问题。基于异或的特性x ^ 0 = x且x ^ x = 0，只要将数组中的元素逐个进行异或，那么其它出现了两次的元素最终计算得到0，再与剩下的一个元素进行异或即可得到结果。


#### 代码实现
```java
public int solution(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```
#### 小结

这道题目同样是一道非常基础的题目。只要能想到异或这一技巧，编码就非常简单了。

### 两个数组的交集 II
#### 题目

给定两个数组，写一个方法来计算它们的交集。其中，输出结果中每个元素出现的次数，应与在两个数组中出现的次数一致。例如：
>输入：nums1=[1, 2, 2, 1], nums2=[2, 2]  
>输出：[2, 2]

#### 整体思路

对于这一题目，主要有两种思路。一种就是利用排序，另一种则是利用hash进行快速检索。  

对于排序思路，我们可以将两个数组进行排序（例如皆为升序）。分别同时遍历两个数组，如果数组元素相等，则将其加入到结果数组当中，同时移动两个数组的指针；如果两个元素不等，由于两个数组皆为升序，那么当前元素较小数组的指针只有继续前移才有可能与当前较大的元素相等，因此此时要将较小元素数组的指针向前移动一位。直至**其中一个**数组的元素全部被遍历完。这一思路的时间复杂度主要在排序操作上，也就是O(n log n)。  

对于利用hash的思路，则需要借助一个实现了hash操作的数据结构。同时，由于本题需要保留元素出现的次数，因此我们这里就借助HashMap，在保存元素的同时记录下每个元素出现的次数。我们声明一个HashMap来保存下其中一个数组的元素（作为key）及其出现的次数（作为value）。之后遍历另外一个数组，对该HashMap进行反向操作，也就是如果HashMap中value大于0的key中包含这个数组的元素，则将该元素加入到结果数组当中，同时将value减1。这一思路的时间复杂度为O(n)，但是由于需要一个额外的HashMap，空间复杂度也为O(n)。

#### 代码实现

第一种思路：
```java
public int[] solution(int[] nums1, int[] nums2) {
    Arrays.sort(nums1);
    Arrays.sort(nums2);

    ArrayList<Integer> middle = new ArrayList<Integer>();

    int index1 = 0, index2 = 0;

    while (index1 < nums1.length && index2 < nums2.length) {
        if (nums1[index1] == nums2[index2]) {
            middle.add(nums1[index1]);
            index1++;
            index2++;
        } else if (nums1[index1] < nums2[index2]) {
            index1++;
        } else if (nums1[index1] > nums2[index2]) {
            index2++;
        }
    }

    int[] result = new int[middle.size()];

    for (int i = 0; i < result.length; i++) {
        result[i] = middle.get(i);
    }
    return result;
}
```

第二种思路：
```java
public int[] solution(int[] nums1, int[] nums2) {
    HashMap<Integer, Integer> numMap = new HashMap<>();
    ArrayList<Integer> middle = new ArrayList<>();

    for (int num : nums1) {
        if (numMap.containsKey(num)) {
            numMap.put(num, numMap.get(num)+1);
        } else {
            numMap.put(num, 1);
        }
    }

    for (int num : nums2) {
        if (numMap.containsKey(num) && numMap.get(num) > 0) {
            middle.add(num);
            numMap.put(num, numMap.get(num)-1);
        }
    }

    int[] result = new int[middle.size()];
    for (int i = 0; i < result.length; i++) {
        result[i] = middle.get(i);
    }

    return result;
}
```
#### 小结
不同于直接求数学意义上的交集时可以直接使用HashSet进行操作，本题还需要记录下元素出现的次数。因此，HashMap替代了HashSet的位置。排序思路则更为通用一些，不论是数学意义上的交集还是本题所需要的结果，都可直接套用。

### 加一
#### 题目
给定一个非负整数组成的非空数组，在该数的基础上加一，返回一个新的数组。最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。你可以假设除了整数0之外，这个整数不会以零开头。例如：
>输入：[1,2,3]，输出：[1,2,4]  

#### 整体思路
第一反应会是两个整数相加的思路，但是这样的操作对于本题就啰嗦了。由于本题只需要对输入加**一**，那么只要不发生进位，加法过程也就结束了。当然，如果结果的位数比输入多一位的话，那么返回值只有可能是[1,0,...,0]。
#### 代码实现
```java
public int[] solution(int[] nums) {
    for (int i = nums.length - 1; i > -1; i--) {
        if (nums[i] < 9) {
            nums[i]++;
            return nums;
        }

        nums[i] = 0;
    }

    int[] result = new int[nums.length+1];
    result[0]=1;
    return result;
}
```
#### 小结
加**一**可能并不需要每一位都去计算。

### 移动零
#### 题目
给定一个数组nums，编写一个函数将所有0移动到数组的末尾，同时保持非零元素的相对顺序。例如：
>输入：[0,1,0,3,12]，输出：[1,3,12,0,0]  

#### 整体思路：
稍微思考一下，我们发现这个题目与原地去重有一点类似。因此就形成了与原地去重方法类似的思路：使用两个指针，一个用来指示非零数组的尾巴，另一个用来遍历数组来获得非零元素；在移动完成之后，将非零数组尾巴到输入数组尾巴之间的元素全部填充为0。这样的时间复杂度为O(n)，空间复杂度为O(1)。
参考了leetcode上的[文章](https://leetcode.com/problems/move-zeroes/solution/#)之后，发现了更进一步的思路，也就是用交换来替代填充。同样是使用两个指针，当遍历数组的指针发现非0元素时，将指示非零数组尾巴的指针指向的元素与刚刚发现的非0元素进行交换。尽管这一思路的时间复杂度仍然为O(n)，但是在[0,0,...,0,1]等极端情况下，可以有效地降低写入次数。
#### 代码实现
第一种思路：
```java
public void solution(int[] nums) {
    int len = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[len] = nums[i];
            len++;
        }
    }

    while (len < nums.length) {
        nums[len] = 0;
        len++;
    }
}
```
第二种思路：
```java
public void solution(int[] nums) {
    int len = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            swap(nums, i, len);
            len++;
        }
    }
}
public void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```
#### 小结
与原地去重类似，依然是需要两个指针。前一种思路则是因为懒得写swap方法而牺牲了执行效率..

### 两数之和
#### 题目
给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。例如：
>输入：nums = [2, 7, 11, 15], target = 9；因为 nums[0] + nums[1] = 2 + 7 = 9，所以返回 [0, 1]  

#### 整体思路
HashMap与HashSet在需要快速搜素时是非常重要的数据结构。本题实际上就是需要快速搜索到满足条件的整数的index，那么HashMap是我们应该纳入考虑的数据结构。那么基本思路就是，遍历数组元素，将其与target的差值作为key，该元素的index作为value存入map（在leetcode的[讲解](https://leetcode.com/problems/two-sum/solution/#)中，是将该元素自身及其index存入map）；这样在遍历下一个元素时，可先行在map中进行搜索该元素自身（或该元素与target的差值）是否在map当中，如果存在，则将value中的index与当前index拼成数组返回即可。
#### 代码实现
尽管原理是一致的，但是本文提出的思路与leetcode讲解中提出的思路考虑问题的角度是不一样的，因此下面我们将给出两种实现方式：  
差值作为key：
```java
public int[] solution(int[] nums, int target) {
    int[] result = new int[2];
    HashMap<Integer, Integer> numMap = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (numMap.containsKey(nums[i])) {
            result[0] = numMap.get(nums[i]);
            result[1] = i;
            return result;
        } else {
            numMap.put(target - nums[i], i);
        }
    }

    throw new IllegalArgumentException("No solution!");
}
```
自身作为key：
```java
public int[] solution(int[] nums, int target) {
    int[] result = new int[2];
    HashMap<Integer, Integer> numMap = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int temp = target - nums[i];
        if (numMap.containsKey(temp)) {
            result[0] = numMap.get(temp);
            result[1] = i;
            return result;
        } else {
            numMap.put(nums[i], i);
        }
    }

    throw new IllegalArgumentException("No solution!");
}
```
#### 小结
这道题本质上是查找，那么HashMap就是我们不得不考虑的基础数据结构了。

### 有效的数独
#### 题目
判断一个9x9的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。
1. 数字1-9在每一行只能出现一次。
2. 数字1-9在每一列只能出现一次。
3. 数字1-9在每一个以粗实线分隔的3x3宫内只能出现一次。

例如：
>输入：  
[  
&emsp;["5","3",".",".","7",".",".",".","."],  
&emsp;["6",".",".","1","9","5",".",".","."],  
&emsp;[".","9","8",".",".",".",".","6","."],  
&emsp;["8",".",".",".","6",".",".",".","3"],  
&emsp;["4",".",".","8",".","3",".",".","1"],  
&emsp;["7",".",".",".","2",".",".",".","6"],  
&emsp;[".","6",".",".",".",".","2","8","."],  
&emsp;[".",".",".","4","1","9",".",".","5"],  
&emsp;[".",".",".",".","8",".",".","7","9"]  
]  
输出：true；  
输入：  
[  
&emsp;["8","3",".",".","7",".",".",".","."],  
&emsp;["6",".",".","1","9","5",".",".","."],  
&emsp;[".","9","8",".",".",".",".","6","."],  
&emsp;["8",".",".",".","6",".",".",".","3"],  
&emsp;["4",".",".","8",".","3",".",".","1"],  
&emsp;["7",".",".",".","2",".",".",".","6"],  
&emsp;[".","6",".",".",".",".","2","8","."],  
&emsp;[".",".",".","4","1","9",".",".","5"],  
&emsp;[".",".",".",".","8",".",".","7","9"]  
]  
输出：false

#### 整体思路
题目看上去很繁杂，又涉及到二维数组的处理。但是，题干中也已经给出了本题的切入点，也就是三条规则。那么，只能想办法遍历数组中的元素并判断其是否符合三条规则。  
数组为9行9列。那么，前两条规则相对来说比较简单，只需要使用相应的数据结构保存已经出现过的元素即可，如果出现了重复元素，则直接返回false；对于第三条规则，其重点就是确定当前元素所属的大方格，判断元素值与方格中的其它值是否重复即可。  
我们可以声明3个数组来保存三条规则下元素的出现情况，例如对于行、列规则，我们可以声明两个二维数组row[][]与col[][]，其中row[i][x-1]为true则表示第i行中x已经出现过；同理，col[j][x-1]为true则表示第j列中x已经出现过。而对于第三条规则，我们既可以声明一个三维数组，例如cube[][][]，其中cube[i/3][j/3][x-1]为true则表示元素[i,j]所属的大方格中x已经出现过；也可以与前述两个规则的数组形式类似，声明一个二维数组cube[][]，其中第一个index用来表示数组的编号，即从左上角数第几个大方格，也就是i/3*x+j/3，那么cube[i/3*3+j/3][x-1]为true表示在[i,j]所属的方格中x已经出现过。
####　代码实现
```java
public boolean solution(char[][] board) {
    boolean[][] row = new boolean[9][9];
    boolean[][] col = new boolean[9][9];
    boolean[][] cube = new boolean[9][9];

    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.') {
                int elem = Character.getNumericValue(board[i][j])-1;
                if (row[i][elem] || col[j][elem] || cube[i/3*3+j/3][elem]) {
                    return false;
                }
            
                row[i][elem] = col[j][elem] = cube[i/3*3+j/3][elem] = true;
            }
        }
    }

    return true;
}
```
#### 小结
这一道题目只能是在遍历数组元素的基础上，想办法适配三条规则，来获得结果。
### 旋转图像
#### 题目
给定一个n×n的二维矩阵表示一个图像。将图像顺时针旋转 90 度。说明：你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。例如：
>给定 matrix =   
[  
&emsp;[1,2,3],  
&emsp;[4,5,6],  
&emsp;[7,8,9]  
],  
>原地旋转输入矩阵，使其变为:  
[  
&emsp;[7,4,1],  
&emsp;[8,5,2],  
&emsp;[9,6,3]  
]  

#### 整体思路
图像旋转一般可以通过两次反转实现，即一次对角线翻转、一次行/列翻转。其中一种通用的反转方法是：
1. 先沿着中间上下翻转
2. 再沿左上->右下的对角线翻转

如果是想要先沿着对角线翻转，那么流程就变为：
1. 先沿左上->右下的对角线翻转
2. 再沿着中间左右翻转

同理，可以摸索其它的翻转形式。
#### 代码实现
第一种方法：
```java
public void solution(int[][] matrix) {
    for (int i = 0; i < matrix.length / 2; i++) {
        int[] temp = matrix[i];
        matrix[i] = matrix[matrix.length-1-i];
        matrix[matrix.length-1-i] = temp;
    }

    for (int i = 0; i < matrix.length; i++) {
        for (int j = i + 1; j < matrix[i].length; j++) {
            int t = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = t;
        }
    }

}
```
第二种方法：
```java
public void solution(int[][] matrix) {
    for (int i = 0; i < matrix.length; i++) {
        for (int j = i + 1; j < matrix[i].length; j++) {
            int t = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = t;
        }
    }

    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix.length / 2; j++) {
            int len = matrix[i].length;
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][len-1-j];
            matrix[i][len-1-j] = temp;
        }
    }
}
```
#### 小结
通过上述编码不难发现，对于二维数组而言，左右翻转效率很低。因此，我们还是应该优先考虑结合上下翻转的旋转方案。


### Finally

上述都是数组中十分基础的题目。不难发现，排序以及hash是解决数组问题很重要的手段。
如果您有任何建议或看法，十分欢迎您通过[Github Issues](https://github.com/Wennn/wennn.github.io/issues)为我指正。