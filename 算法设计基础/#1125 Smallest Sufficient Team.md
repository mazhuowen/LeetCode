[toc]

In a project, you have a list of required skills `req_skills`, and a list of `people`.  The i-th person `people[i]` contains a list of skills that person has.

Consider a sufficient team: a set of people such that for every required skill in `req_skills`, there is at least one person in the team who has that skill.  We can represent these teams by the index of each person: for example, `team = [0, 1, 3]` represents the people with skills `people[0]`, `people[1]`, and `people[3]`.

Return **any** sufficient team of the smallest possible size, represented by the index of each person.

You may return the answer in any order.  It is guaranteed an answer exists.



**Constraints**:

* $1 \le \text{req_skills.length} \le 16$
* $1 \le \text{people.length} \le 60$
* $1 \le \text{people[i].length, req_skills[i].length, people[i][j].length} \le 16$
* Elements of `req_skills` and `people[i]` are (respectively) distinct.
* `req_skills[i][j]`, `people[i][j][k]` are lowercase English letters.
* Every skill in `people[i]` is a skill in `req_skills`.
* It is guaranteed a sufficient team exists.



## 题目解读

&emsp;返回任意一个满足团队技能要求的最少人员名单。

```java
class Solution {
    public int[] smallestSufficientTeam(String[] req_skills, List<List<String>> people) {

    }
}
```

## 程序设计

* 动态规划数组`dp(i,j)`表示$0 \sim i$个人可构成技能的二进制表示$j$的最少人数。

```java
class Solution {
    public int[] smallestSufficientTeam(String[] req_skills, List<List<String>> people) {
        if (req_skills == null || req_skills.length < 1 || people.size() < 1) throw new IllegalArgumentException("invalid param");

        int n = req_skills.length, m = people.size();

        Map<String, Integer> record = new HashMap<>();
        for (int i = 0; i < n; i++) record.put(req_skills[i], i);

        int[][] dp = new int[m + 1][1 << n];
        // 最多60人，61表示无法构建团队
        Arrays.fill(dp[0], 61);
        dp[0][0] = 0;

        for (int i = 1; i <= m; i++) {
            List<String> skills = people.get(i - 1);
            // 第i个人的技能二进制表示
            int stat = 0;
            for (String skill : skills) stat |= 1 << record.get(skill);
            
            for (int j = 1; j < (1 << n); j++) {
                // 与状态j无交集
                if ((j & stat) == 0) dp[i][j] = dp[i - 1][j];
                // 与状态j有交集，表示可由之前的状态得到，即j ^ (j & stat)拼接stat可得到覆盖j的状态
                // 覆盖比如101拼接状态011得到111覆盖110
                else dp[i][j] = Math.min(dp[i - 1][j], dp[i - 1][j ^ (j & stat)] + 1);
            }
        }

        // 根据最终结果倒推
        int idx = 0, end = (1 << n) - 1;
        if (dp[m][end] == 61) throw new IllegalArgumentException("invalid param");

        int[] res = new int[dp[m][end]];
        for (int i = m; i >= 1; i--) {
            List<String> skills = people.get(i - 1);
            int stat = 0;
            for (String skill : skills) stat |= 1 << record.get(skill);
            // 第i个人得到状态end的前一个状态stat
            stat = end ^ (stat & end);
            // 表示第i个人是团队一员
            if (dp[i][end] == dp[i - 1][stat] + 1) {
                res[idx++] = i - 1;
                end = stat;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M2^N)$，空间复杂度为$O(M2^N)$。

执行用时：23ms，在所有java提交中击败了72.94%的用户。

内存消耗：61.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。可继续压缩上述动态规划数组，同时引入回溯记录数组。

```java
class Solution {
    public int[] smallestSufficientTeam(String[] req_skills, List<List<String>> people) {
        if (req_skills == null || req_skills.length < 1 || people.size() < 1) throw new IllegalArgumentException("invalid param");

        int n = req_skills.length, m = people.size();
        Map<String, Integer> record = new HashMap<>();
        for (int i = 0; i < n; i++) record.put(req_skills[i], i);

        // 提前计算每个人的技能掩码
        int[] skillMask = new int[m];
        for (int i = 0; i < m; i++) {
            int stat = 0;
            List<String> skills = people.get(i);
            for (String skill : skills) stat |= 1 << record.get(skill);
            skillMask[i] = stat;
        }

        int[] dp = new int[1 << n];
        // 记录动态规划数组前一时刻状态
        int[] prePeople = new int[1 << n];
        int[] preStatus = new int[1 << n];
        // 最多60人，61表示无法构建团队
        Arrays.fill(dp, 61);
        dp[0] = 0;

        for (int i = 1; i <= m; i++) {
            int[] cur = new int[1 << n];

            int stat = skillMask[i - 1];
            for (int j = 1; j < (1 << n); j++) {
                cur[j] = dp[j];
                if ((j & stat) != 0 && dp[j ^ (j & stat)] + 1 < cur[j]) {
                    cur[j] = dp[j ^ (j & stat)] + 1;
                    // 记录当前人及前一刻状态
                    prePeople[j] = i - 1;
                    preStatus[j] = j ^ (j & stat);
                }
            }
            dp = cur;
        }

        // 根据最终结果倒推
        int idx = 0, end = (1 << n) - 1;
        if (dp[end] == 61) throw new IllegalArgumentException("invalid param");
		// 倒推
        int[] res = new int[dp[end]];
        while (end > 0) {
            res[idx++] = prePeople[end];
            end = preStatus[end];
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(M2^N)$，空间复杂度为$O(2^N)$。

执行用时：19ms，在所有java提交中击败了74.12%的用户。

内存消耗：42.3MB，在所有java提交中击败了100.00%的用户。