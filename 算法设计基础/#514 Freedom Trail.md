[toc]

In the video game Fallout 4, the quest "Road to Freedom" requires players to reach a metal dial called the "Freedom Trail Ring", and use the dial to spell a specific keyword in order to open the door.

Given a string **ring**, which represents the code engraved on the outer ring and another string **key**, which represents the keyword needs to be spelled. You need to find the **minimum** number of steps in order to spell all the characters in the keyword.

Initially, the first character of the **ring** is aligned at 12:00 direction. You need to spell all the characters in the string **key** one by one by rotating the ring clockwise or anticlockwise to make each character of the string **key** aligned at 12:00 direction and then by pressing the center button.

At the stage of rotating the ring to spell the key character `key[i]`:

* You can rotate the **ring** clockwise or anticlockwise **one place**, which counts as 1 step. The final purpose of the rotation is to align one of the string **ring's** characters at the 12:00 direction, where this character must equal to the character `key[i]`.
* If the character `key[i]` has been aligned at the 12:00 direction, you need to press the center button to spell, which also counts as 1 step. After the pressing, you could begin to spell the next character in the key (next stage), otherwise, you've finished all the spelling.



**Note**:

* Length of both ring and **key** will be in range $1$ to $100$.
* There are only lowercase letters in both strings and might be some duplcate characters in both strings.
* It's guaranteed that string **key** could always be spelled by rotating the string **ring**.



## 题目解读

&emsp;旋转字符串环来拼接给定的字符串，求最少的步数。

```java
class Solution {
    public int findRotateSteps(String ring, String key) {

    }
}
```

## 程序设计

* 最基本的思路是统计每个字符在字符串中的所有索引位置，方便后续计算距离，然后回溯暴力遍历每种可能性；最坏情况时间复杂度为$O(KR^R)$；
* 该思路会超时，如果加入剪枝条件，即只选择最短的距离，结果会出错，以`ring`为`caotmcaataijjxi`为例，若`key`中的一段是`ita`，则根据上述剪枝思路`i`到`t`选择索引为$3$的`t`最短，但实际上最优结果为索引为$8$的`t`。

```java
class Solution {
    int minCost = Integer.MAX_VALUE;
    char[] r, k;
    List<Integer>[] index;

    public int findRotateSteps(String ring, String key) {
        r = ring.toCharArray();
        k = key.toCharArray();
        // 统计每个字符的索引
        index = new LinkedList[26];
        for (int i = 0; i < 26; i++) index[i] = new LinkedList<>();
        for (int i = 0; i < r.length; i++) index[r[i] - 'a'].add(i);

        backTracing(0, 0, 0);
        return minCost;
    }

    // 当前的key位置和当前的ring位置
    private void backTracing(int keyIdx, int ringIdx, int cost) {
        // 剪枝
        if (cost >= minCost) return;
        if (keyIdx == k.length) {
            minCost = cost;
            return;
        }

        // 移动到的ring位置
        for (int next : index[k[keyIdx] - 'a']) {
            backTracing(keyIdx + 1, next, cost + getSteps(ringIdx, next, r.length) + 1);
        }
    }

    // 获取两点之间的最小距离
    private int getSteps(int start, int end, int len) {
        int dis = Math.abs(end - start);
        return Math.min(dis, len - dis);
    }
}
```

* 根据上述回溯可知，每次只需选择在`key`、`ring`上的索引上匹配剩余字符串的最小代价，即加入记忆化数组，减少重复计算。

```java
class Solution {
    char[] r, k;
    List<Integer>[] index;
    int[][] record;

    public int findRotateSteps(String ring, String key) {
        r = ring.toCharArray();
        k = key.toCharArray();
        // 统计每个字符的索引
        index = new LinkedList[26];
        // 记忆数组
        record = new int[k.length][r.length];
        for (int i = 0; i < 26; i++) index[i] = new LinkedList<>();
        for (int i = 0; i < r.length; i++) index[r[i] - 'a'].add(i);

        return backTracing(0, 0);
    }

    // 当前的key位置和当前的ring位置
    private int backTracing(int keyIdx, int ringIdx) {
        // 匹配结束
        if (keyIdx == k.length) return 0;
        if (record[keyIdx][ringIdx] != 0) return record[keyIdx][ringIdx];

        int res = Integer.MAX_VALUE;
        // 移动到的位置
        for (int next : index[k[keyIdx] - 'a']) {
            res = Math.min(res, backTracing(keyIdx + 1, next) + getSteps(ringIdx, next, r.length) + 1);
        }
        return record[keyIdx][ringIdx] = res;
    }

    // 获取两点之间的最小距离
    private int getSteps(int start, int end, int len) {
        int dis = Math.abs(end - start);
        return Math.min(dis, len - dis);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(KR^2)$，空间复杂度为$O(KR)$。

执行用时：8ms，在所有java提交中击败了95.95%的用户。

内存消耗：38.7MB，在所有java提交中击败了98.13%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有采用动态规划的思路。

```java
class Solution {
    public int findRotateSteps(String ring, String key) {
        char[] r = ring.toCharArray(), k = key.toCharArray();
        // 统计每个字符的索引
        List<Integer>[] index = new LinkedList[26];
        for (int i = 0; i < 26; i++) index[i] = new LinkedList<>();
        for (int i = 0; i < r.length; i++) index[r[i] - 'a'].add(i);

        // 动态规划数组
        int[][] dp = new int[k.length][r.length];
        for (int[] arr : dp) Arrays.fill(arr, Integer.MAX_VALUE);
        // 初始化首个字符配对
        for (int idx : index[k[0] - 'a']) {
            dp[0][idx] = getSteps(0, idx, r.length) + 1;
        }

        for (int i = 0; i < k.length - 1; i++) {
            for (int j = 0; j < r.length; j++) {
                if (dp[i][j] == Integer.MAX_VALUE) continue;
                char c = k[i + 1];
                for (int idx : index[c - 'a']) {
                    dp[i + 1][idx] = Math.min(dp[i + 1][idx], dp[i][j] + getSteps(j, idx, r.length) + 1);
                }
            }
        }

        // 选择最小值
        int res = Integer.MAX_VALUE;
        for (int num : dp[k.length - 1]) res = Math.min(res, num);
        return res;
    }

    // 获取两点之间的最小距离
    private int getSteps(int start, int end, int len) {
        int dis = Math.abs(end - start);
        return Math.min(dis, len - dis);
    }
}
```

&emsp;时间复杂度为$O(KR^2)$，空间复杂度为$O(KR)$。

执行用时：12ms，在所有java提交中击败了77.03%的用户。

内存消耗：39MB，在所有java提交中击败了94.39%的用户。