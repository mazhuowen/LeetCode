[toc]

You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the **sum** of scores of all the players in the team.

However, the basketball team is not allowed to have **conflicts**. A **conflict** exists if a younger player has a **strictly higher** score than an older player. A conflict does **not** occur between players of the same age.

Given two lists, `scores` and `ages`, where each `scores[i]` and `ages[i]` represents the score and age of the `i`th player, respectively, return the highest overall score of all possible basketball teams.



**Constraints**:

* $1 \le \text{scores.length, ages.length} \le 1000$
* $\text{scores.length} == \text{ages.length}$
* $1 \le \text{scores[i]} \le 10^6$
* $1 \le \text{ages[i]} \le 1000$



## 题目解读

&emsp;选择的分最高的无矛盾球队，矛盾定义为年纪轻的球员比年极高的球员得分更高。

```java
class Solution {
    public int bestTeamScore(int[] scores, int[] ages) {

    }
}
```

## 程序设计

* 最基本的思路就是动态规划，首先根据年龄排序，这样问题转化为在得分的序列中选择一个增序序列使得子序列之和最大；
* 采用动态规划，`dp(i)`表示选中第$i$个人的序列最大的分。

```java
class Solution {
    public int bestTeamScore(int[] scores, int[] ages) {
        int n = scores.length;
        Integer[] index = new Integer[n];
        for (int i = 0; i < n; i++) index[i] = i;
        // 根据年纪排序
        Arrays.sort(index, (a, b) -> ages[a] == ages[b] ? scores[a] - scores[b] : ages[a] - ages[b]);

        int max = 0;
        // 包含第i个人的球队最大得分
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) {
            int idx = index[i];
            int curScores = dp[i] = scores[idx];
            for (int j = i - 1; j >= 0; j--) {
                // 年纪小的球员大于年纪大的球员，跳过
                if (ages[index[j]] < ages[idx] && scores[index[j]] > scores[idx]) continue;
                dp[i] = Math.max(dp[i], dp[j] + curScores);
            }
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：67ms，在所有java提交中击败了37.72%的用户。

内存消耗：38.9MB，在所有java提交中击败了20.88%的用户。

## 官方解题

&emsp;暂无，密切关注。