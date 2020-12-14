[toc]

In a special ranking system, each voter gives a rank from highest to lowest to all teams participated in the competition.

The ordering of teams is decided by who received the most position-one votes. If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter.

Given an array of strings `votes` which is the votes of all voters in the ranking systems. Sort all teams according to the ranking system described above.

Return a string of all teams **sorted** by the ranking system.

 

**Example 1**:

```
Input: votes = ["ABC","ACB","ABC","ACB","ACB"]
Output: "ACB"
Explanation: Team A was ranked first place by 5 voters. No other team was voted as first place so team A is the first team.
Team B was ranked second by 2 voters and was ranked third by 3 voters.
Team C was ranked second by 3 voters and was ranked third by 2 voters.
As most of the voters ranked C second, team C is the second team and team B is the third.
```

**Example 2**:

```
Input: votes = ["WXYZ","XYZW"]
Output: "XWYZ"
Explanation: X is the winner due to tie-breaking rule. X has same votes as W for the first position but X has one vote as second position while W doesn't have any votes as second position. 
```

**Example 3**:

```
Input: votes = ["ZMNAGUEDSJYLBOPHRQICWFXTVK"]
Output: "ZMNAGUEDSJYLBOPHRQICWFXTVK"
Explanation: Only one voter so his votes are used for the ranking.
```

**Example 4**:

```
Input: votes = ["BCA","CAB","CBA","ABC","ACB","BAC"]
Output: "ABC"
Explanation: 
Team A was ranked first by 2 voters, second by 2 voters and third by 2 voters.
Team B was ranked first by 2 voters, second by 2 voters and third by 2 voters.
Team C was ranked first by 2 voters, second by 2 voters and third by 2 voters.
There is a tie and we rank teams ascending by their IDs.
```

**Example 5**:

```
Input: votes = ["M","M","M","M"]
Output: "M"
Explanation: Only team M in the competition so it has the first rank.
```



**Constraints**:

* $1 \le \text{votes.length} \le 1000$
* $1 \le \text{votes[i].length} \le 26$
* $\text{votes[i].length} == \text{votes[j].length}$ for $0 \le i, j < \text{votes.length}$.
* `votes[i][j]` is an English **upper-case** letter.
* All characters of `votes[i]` are unique.
* All the characters that occur in `votes[0]` **also occur** in `votes[j]` where $1 \le j < \text{votes.length}$.



## 题目解读

&emsp;给定不同投票者的排名，求最终的排名。

```java
class Solution {
    public String rankTeams(String[] votes) {

    }
}
```

## 程序设计

* 根据题意，对每个字符的排名投票情况进行统计，然后排序，出现在前面位置次数多的排在前面，如果次数相等，则根据字典序排列。

```java
class Solution {
    public String rankTeams(String[] votes) {
        Map<Character, int[]> record = new HashMap<>();
        // 统计每个字符的排名计数
        for (String vote : votes) {
            for (int i = 0; i < vote.length(); i++) {
                int[] counter = record.getOrDefault(vote.charAt(i), new int[26]);
                counter[i]++;
                record.put(vote.charAt(i), counter);
            }
        }

        List<Map.Entry<Character, int[]>> list = new ArrayList<>(record.entrySet());
        Collections.sort(list, (a, b) -> {
            int compare = compare(a.getValue(), b.getValue());
            if (compare == 0) return a.getKey() - b.getKey();
            else return -compare;
        });

        StringBuffer sb = new StringBuffer();
        for (Map.Entry<Character, int[]> ele : list) {
            sb.append(ele.getKey());
        }
        return sb.toString();
    }

    // 0相等，1大于，-1小于
    private int compare(int[] a, int[] b) {
        for (int i = 0; i < a.length; i++) {
            if (a[i] > b[i]) return 1;
            else if (a[i] < b[i]) return -1;
        }
        return 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(1)$。

执行用时：24 ms, 在所有 Java 提交中击败了31.40%的用户

内存消耗：38.3 MB, 在所有 Java 提交中击败了72.50%的用户。

## 官方解题

&emsp;官方思路类似。
