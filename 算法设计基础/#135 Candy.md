[toc]

There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.

What is the minimum candies you must give?



## 题目解读

&emsp;$N$个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。每个孩子至少分配到一个糖果，相邻的孩子中，评分高的孩子获得更多的糖果。试求所需的最少糖果。

```java
class Solution {
    public int candy(int[] ratings) {

    }
}
```

## 程序设计

* 根据题意，相邻的元素，评分大的糖果数多；要分配最小糖果数，则相邻元素评分大的的比小的多一个糖果；其次在示例`[1,2,2]`中，分别给三个孩子分1、2、1个糖果，是最优方法，可见相邻的两个元素即使评分相等糖果分配也可能不同，需取决于相邻元素的评分得大小。
* 可以初始分配每个孩子糖果为1，然后循环遍历数组，每次遍历到一个位置，判断其后的孩子和当前孩子分配的糖果数是否满足要求，不满足则更新当前孩子的糖果数；由于更新当前孩子的糖果数有可能会改变当前孩子和之前孩子的糖果数关系，则还要判断当前孩子和之前孩子的关系是否满足条件。

```java
class Solution {
    public int candy(int[] ratings) {
        if (ratings == null || ratings.length == 0) return 0;

        // 检查标识，标识数组中元素是否需要调整
        boolean flag = true;
        int[] candies = new int[ratings.length];
        Arrays.fill(candies, 1);
        while (flag) {
            flag = false;
            for (int i = 0; i < ratings.length; i++) {
                // 当前位置比其后评分高，糖果数不满足要求，则更新
                if (i != ratings.length - 1 && ratings[i] > ratings[i + 1] && candies[i] <= candies[i + 1]) {
                    flag = true;
                    candies[i] = candies[i + 1] + 1;
                }
                // 当前位置比其前评分高且糖果不满足要求
                if (i != 0 && ratings[i - 1] < ratings[i] && candies[i - 1] >= candies[i]) {
                    flag = true;
                    candies[i] = candies[i - 1] + 1;
                }
            }
        }
        int sum = 0;
        for (int num : candies) sum += num;

        return sum; 
    }
}
```

* 如果遍历一次决定，假设一段序列为`1,3,5,7,6,4,2`，如果从左边标记，则每个人需要分配`1,2,3,4,3,2,1`个糖果，但是考虑到峰顶左右两侧不均衡的情况，如`1,3,5,7,6,2`，则会得到`1,2,3,4,3,2`的结果，实际应该是`1,2,3,4,2,1`。
* 考虑到上述分析，可以采用两个数组分别遍历维护每个峰顶的左边和右边，最后结合得到分配方案。如`1,3,5,7,6,2`，左侧遍历维护后为`1,2,3,4,1,1`，右侧遍历维护后为`1,1,1,3,2,1`，选取最大的，最后方案为`1,2,3,4,2,1`，符合要求且最小。

```java
class Solution {
    public int candy(int[] ratings) {
        if (ratings == null || ratings.length == 0) return 0;

        int[] left = new int[ratings.length];
        int[] right = new int[ratings.length];
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        // 维护左侧
        for (int i = 1; i < ratings.length; i++) {
            if (ratings[i] > ratings[i - 1]) left[i] = left[i - 1] + 1;
        }
		// 维护右侧
        for (int i = ratings.length - 1; i >= 1; i--) {
            if (ratings[i - 1] > ratings[i]) right[i - 1] = right[i] + 1;
        }

        int sum = 0;
        // 结合左右，选择最大的
        for (int i = 0; i < ratings.length; i++) {
            sum += Math.max(left[i], right[i]);
        }
        return sum;
    }
}
```

* 分析上述过程，可在一个数组中实现，唯一需要判断的就是峰值元素选择最大的。

```java
class Solution {
    public int candy(int[] ratings) {
        if (ratings == null || ratings.length == 0) return 0;

        int[] candies = new int[ratings.length];
        Arrays.fill(candies, 1);

        // 维护左侧
        for (int i = 1; i < ratings.length; i++) {
            if (ratings[i] > ratings[i - 1] && candies[i] <= candies[i - 1]) candies[i] = candies[i - 1] + 1; 
        }
        int sum = 0;
        // 维护右侧
        for (int i = ratings.length - 1; i >= 0; i--) {
            if (i != ratings.length - 1 && ratings[i] > ratings[i + 1] && candies[i] <= candies[i + 1]) candies[i] = candies[i + 1] + 1;

            sum += candies[i];
        }
        return sum;
    }
}
```

## 性能分析

&emsp;暴力法最坏的情况评分是逆序的，此时需要遍历更新$N$遍，每一遍需要遍历数组$N$个元素，时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：1634ms，在所有java提交中击败了5.04%的用户。

内存消耗：39.9MB，在所有java提交中击败了22.58%的用户。

&emsp;改进的暴力法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了87.97%的用户。

内存消耗：40.5MB，在所有java提交中击败了5.37%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了最优解法。在上述谈论中已经提过峰顶的概念，由于峰顶两侧孩子数不均衡，导致峰顶是两侧最大孩子数，而两侧则是由谷底为1，依次递增。

<img src="../images/#135.png"  />

```java
class Solution {
    public int candy(int[] ratings) {
        if (ratings == null || ratings.length == 0) return 0;

        // 记录峰顶两侧上升坡和下降坡的长度
        int up = 0, down = 0;
        // 记录当前点和之前点所处是上升坡还是下降坡，还是持平
        int oldSlope = 0, newSlope;
        int sum = 0;

        for (int i = 1; i < ratings.length; i++) {
            // 当前点处于上升坡1还是下降坡-1，还是持平0
            newSlope = ratings[i] > ratings[i - 1] ? 1 : (ratings[i] == ratings[i - 1] ? 0 : -1);

            // 1、当前评分持平，之前为上升坡，意味着一个山峰在当前结点结束，计算之前计数；
            // 2、当前为上升坡，之前为下降坡，意味着一个山峰在当前结点结束，计算之前计数；
            // 3、当前评分持平，之前为下降坡，同理意味着一个山峰在当前结点结束，计算之前计数。
            if ((oldSlope > 0 && newSlope == 0) || (oldSlope < 0 && newSlope >= 0)) {
                // 左侧加右侧，再加顶峰（最大的单调坡节点数）
                // 此处不加1是因为下降坡的最后一个数参与下一个山峰的上升坡，故不加一，避免造成重复计算
                sum += count(up) + count(down) + Math.max(up, down);
                // 重置计数
                up = 0; down = 0;
            }
            if (newSlope > 0) up++;
            else if (newSlope < 0) down++;
            // 持平则需要加一，因为下降坡的最后一个数不参与下一个山峰的上升坡
            else sum++;

            oldSlope = newSlope;
        }
        // 注意最后山峰需要加一
        sum += count(up) + count(down) + Math.max(up, down) + 1;
        return sum;
    }

    // 计算n个上升或下降点组成的单调序列之和
    private int count(int n) {
        return n * (n + 1) / 2;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了35.84%的用户。

内存消耗：40.2MB，在所有java提交中击败了7.52%的用户。