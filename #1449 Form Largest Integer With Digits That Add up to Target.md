[toc]

Given an array of integers `cost` and an integer `target`. Return the **maximum** integer you can paint under the following rules:

* The cost of painting a digit (i+1) is given by `cost[i]` (0 indexed).
* The total cost used must be equal to `target`.
* Integer does not have digits 0.

Since the answer may be too large, return it as string.

If there is no way to paint any integer given the condition, return "0".



**Constraints:**

- $\text{cost.length} == 9$
- $1 \le \text{cost[i]} \le 5000$
- $1 \le \text{target} \le 5000$



## 题目解读

&emsp;返回指定代价能够拼接的最大数字。

```java
class Solution {
    public String largestNumber(int[] cost, int target) {

    }
}
```

## 程序设计

* 本质是完全背包问题，每个数字可以无限制的添加，由于`target`较大，方法会超时。

```java
class Solution {
    public String largestNumber(int[] cost, int target) {
        if (cost == null || cost.length == 0) return "0";

        // 表示前0~i个序列可组成的容量为j的组合
        Set<String>[][] dp = new HashSet[10][target + 1];
        // 初始化
        dp[0][0] = new HashSet<>();
        dp[0][0].add("");

        for (int i = 1; i < 10; i++) {
            int weight = cost[i - 1];

            for (int j = 0; j <= target; j++) {
                dp[i][j] = dp[i - 1][j];
                if (dp[i][j] == null) dp[i][j] = new HashSet<>();

                if (j < weight) continue;

                for (int k = 1; j >= k * weight; k++) {
                    if (dp[i - 1][j - k * weight] == null) continue;

                    for (String s : dp[i - 1][j - k * weight]) {
                        dp[i][j].add(append(s, i, k));
                    }
                }
            }
        }

        if (dp[9][target] == null || dp[9][target].size() == 0) return "0";
        return sort(dp[9][target]);
    }

    // 拼接数字
    private String append(String s, int num, int times) {
        StringBuffer sb = new StringBuffer(s);
        for (int i = 0; i < times; i++) {
            sb.append(num);
        }
        return sb.toString();
    }

    // 排序，长度越长，组成的数字越大，长度相等则比较字典序。
    private String sort(Set<String> set) {
        List<String> temp = new ArrayList<>();
        for (String s : set) {
            char[] strs = s.toCharArray();
            Arrays.sort(strs);
            StringBuffer sb = new StringBuffer(new String(strs)).reverse();
            temp.add(sb.toString());
        }
        temp.sort((a, b) -> a.length() == b.length() ? b.compareTo(a) : b.length() - a.length());
        return temp.get(0);
    }
}
```

* 将上述动态规划状态压缩，当`target`过大时，如测试用例`[2,4,2,5,3,2,5,5,4]` ，`739`超时，时间复杂度为$O(N^2M)$，其中$M$为平均的组合数。

```java
class Solution {
    public String largestNumber(int[] cost, int target) {
        if (cost == null || cost.length == 0) return "0";

        Set<String>[] dp = new HashSet[target + 1];
        dp[0] = new HashSet<>();
        dp[0].add("");

        for (int i = 1; i < 10; i++) {
            int weight = cost[i - 1];

            for (int j = target; j >= weight; j--) {
                if (dp[j] == null) dp[j] = new HashSet<>();

                for (int k = 1; j >= k * weight; k++) {
                    if (dp[j - k * weight] == null) continue;

                    for (String s : dp[j - k * weight]) {
                        dp[j].add(append(s, i, k));
                    }
                }
            }
        }

        if (dp[target] == null || dp[target].size() == 0) return "0";
        return sort(dp[target]);
    }
}
```

* 继续优化，首先代价数组中可能存在多个代价一样的数字，显然代价相等的情况下，数字最大的值最优，可以提前处理代价数组。其次动态规划数组无需保存所有组合，只需选择同等代价最大值。

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;