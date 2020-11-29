[toc]

The set `[1,2,3,...,n]` contains a total of $n!$ unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for $n = 3$:

```
"123"
"132"
"213"
"231"
"312"
"321"
```

Given n and $k$, return the `k`th permutation sequence.



**Note**:

* Given $n$ will be between $1$ and $9$ inclusive.
* Given $k$ will be between $1$ and $n!$ inclusive.



## 题目解读

&emsp;求按字典序排序的第$k$个组合。

```java
class Solution {
    public String getPermutation(int n, int k) {

    }
}
```

## 程序设计

* 通过比较$k$和当前数字开头的组合数，排除定位当前数字是否是所要组合的数字。

```java
class Solution {
    int[] factorial;

    public String getPermutation(int n, int k) {
        // 计算阶乘
        factorial = new int[n + 1];
        factorial[0] = 1;
        for (int i = 1; i <= n; i++) {
            factorial[i] = i * factorial[i - 1];
        }
        // 加入标识
        boolean[] visited = new boolean[n + 1];
        return getPermutation(n, k, visited, new StringBuffer());
    }

    private String getPermutation(int n, int k, boolean[] visited, StringBuffer sb) {
        // 找到匹配字符串
        if (n == 0) return sb.toString();

        for (int i = 1; i <= visited.length; i++) {
            if (visited[i]) continue;

            // 当前数字开头的组合有(n - 1)!个，如果k大于(n - 1)!，说明当前数字开头组合不满足要求
            if (k > factorial[n - 1]) {
                k -= factorial[n - 1];
            } 
            // 要求答案在当前开头组合中，跳出继续检索
            else {
                // 标记已占用
                visited[i] = true;
                sb.append(i);
                break;
            }
        }
        // 继续加错下一位
         return getPermutation(n - 1, k, visited, sb);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.3MB，在所有java提交中击败了9.68%的用户。

## 官方解题

&emsp;官方思路繁琐，大致如上。