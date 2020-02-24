[toc]

Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?



## 题目解读

&emsp;返回所有可以生成的二叉搜索树的数目。

```java
class Solution {
    public int numTrees(int n) {

    }
}
```

## 程序设计

* 采用递归解法，题目超时。

```java
class Solution {
    public int numTrees(int n) {
        if(n <= 0) {
            return 0;
        }
        return numTrees(1, n);
    }

    private int numTrees(int start, int end) {
        // null结点，返回1
        if(start > end) {
            return 1;
        }
        int num = 0;
        for(int i = start; i <= end; i++) {
            int left = numTrees(start, i - 1);
            int right = numTrees(i + 1, end);
            num += left * right;
        }
        return num;
    }
}
```

* 仔细观察，当$n = 1$时，数目为$1$，$n = 2$时数目为$2$，$n = 3$时数目为$5$。即$h(n) = h(0) * h(n - 1) + \dots + h(n - 1) * h(0)$，可以采用动态规划的形式解决。

```java
class Solution {
    public int numTrees(int n) {
        if(n < 1) {
            return 0;
        }
        // 动态规划并初始化
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for(int i = 1; i <= n; i++) {
            for(int j = 0; j < i; j++) {
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }
        return dp[n];
    }
}
```

> 测试参数n最大能取19，超出19会发生溢出。

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.4MB，在所有java提交中击败了5.27%的用户。

## 官方解题

&emsp;官方除了上述动态规划，还提供了最优解，即数学的方法。卡塔兰数公式如下：
$$
C_0 = 1\\
C_{n + 1} = \frac{2(2n + 1)}{n + 2}C_{n}
$$

```java
class Solution {
  public int numTrees(int n) {
    // 特别注意数据溢出
    long C = 1;
    for (int i = 0; i < n; ++i) {
      C = C * 2 * (2 * i + 1) / (i + 2);
    }
    return (int) C;
  }
}
```

> 特别注意此处C要定义为长整形，因为在没到除数前的乘法计算可能会发生溢出。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.5MB, 在所有java提交中击败了5.27%的用户。