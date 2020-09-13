[toc]

Given a string `s` and an array of integers `cost` where `cost[i]` is the cost of deleting the character `i` in `s`.

Return the minimum cost of deletions such that there are no two identical letters next to each other.

Notice that you will delete the chosen characters at the same time, in other words, after deleting a character, the costs of deleting other characters will not change.



**Constraints**:

* $\text{s.length} == \text{cost.length}$
* $1 \le \text{s.length, cost.length} \le 10^5$
* $1 \le \text{cost[i]} \le 10^4$
* `s` contains only lowercase English letters.



## 题目解读

&emsp;给定字符串和删除每个字符的代价，求删除字符使得字符串不重复的最小代价。

```java
class Solution {
    public int minCost(String s, int[] cost) {
        
    }
}
```

## 程序设计

* 对于存在重复字符的字符串，假设某个重复子字符串长度为$l$，则需要删除$l - 1$个，只留$1$个，由于代价需要最低，可知留下的这个是$l$个中代价最高的。
* 这样问题变为一个滑动窗口的问题，统计窗口内的和和最大值，最后代价就是和减去最大值。

```java
class Solution {
    public int minCost(String s, int[] cost) {
        if (s == null || s.length() <= 1) return 0;

        int minCost = 0;
        int left = -1, right = 0, sum = 0, maxCost = 0;
        while (right < cost.length) {
            // 字符不相等，重新定位窗口
            if (left == -1 || s.charAt(left) != s.charAt(right)) {
                // 如果之前的窗口长度大于1，需要删除多余的重复字符
                if (right - left > 1) minCost += sum - maxCost;
                // 重置窗口参数
                sum = maxCost = cost[right];
                left = right;
            } 
            // 队列求和及最大值
            else {
                sum += cost[right];
                maxCost = Math.max(maxCost, cost[right]);
            }
            right++;
        }
        // 最后的窗口
        if (right - left > 1) {
            minCost += sum - maxCost;
        }
        return minCost;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：10ms，在所有java提交中击败了62.90%的用户。

内存消耗：56.6MB，在所有java提交中击败了52.49%的用户。

## 官方解题

&emsp;暂无，密切关注。