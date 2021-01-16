[toc]

Start from integer $1$, remove any integer that contains $9$ such as $9, 19, 29, \dots$

So now, you will have a new integer sequence: $1, 2, 3, 4, 5, 6, 7, 8, 10, 11, \dots$

Given a positive integer $n$, you need to return the n-th integer after removing. Note that $1$ will be the first integer.

 

**Example 1**:

```
Input: n = 9
Output: 10
```



**Constraints**:

* $1 \le n \le 8 \times 10^8$



## 题目解读

&emsp;给定$n$，求从$1$开始除去任何包含$9$的数的第$n$个数字。

```java
class Solution {
    public int newInteger(int n) {

    }
}
```

## 程序设计

* 首先想到的是由$1 \sim 8$作为根节点，子节点为$0 \sim 8$构成的多叉树森林，其路径组成的数字必然不包含$9$；由于每层的数字数目是可计算的，可首先定位到目标数字所在的层数，又由于多叉树是满多叉树的结构，每个节点有$9$个子节点，这样当我们知道目标节点在同层中的编号$no$后，就可以知道父节点的编号$(no - 1) / 9 + 1$和当前节点的值$(no - 1) \% 9$，可通过递归自底向上求解。

```java
class Solution {
    public int newInteger(int n) {
        if (n < 1) return 0;
        // 判断数字所在的层
        int level = 1;
        while (8 * count(level) < n) {
            n -= 8 * count(level++);
        }
        // 从叶结点倒推出数字
        return getNum(n, level);
    }

    private long count(int level) {
        return (long)Math.pow(9, level - 1);
    }

    private int getNum(int no, int level) {
        // 根节点
        if (level == 1) return no;
        // 从当前层的序号，推出上一层的序号和本层的数字
        int topNo = (no - 1) / 9 + 1, curNum = (no - 1) % 9;
        // 递归求上一层
        return getNum(topNo, level - 1) * 10 + curNum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.4 MB, 在所有 Java 提交中击败了40.91%的用户。

## 官方解题

&emsp;官方采用更巧妙的数学思路。仔细观察上述分析，可发现这个结构实际是九进制表示，可直接求解$n$的九进制。

```java
class Solution {
    public int newInteger(int n) {
        int res = 0, bit = 1;
        // 从低位开始转换为九进制
        while (n > 0) {
            res = bit * (n % 9) + res;
            n /= 9;
            bit *= 10;
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.3 MB, 在所有 Java 提交中击败了61.90%的用户。

```java
class Solution {
    public int newInteger(int n) {
        return Integer.parseInt(Integer.toString(n, 9));
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.7 MB, 在所有 Java 提交中击败了9.52%的用户。