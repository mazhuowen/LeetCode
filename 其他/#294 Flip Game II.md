[toc]

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: `+` and `-`, you and your friend take turns to flip two **consecutive** `"++"` into `"--"`. The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.



**Example**:

```
Input: s = "++++"
Output: true 
Explanation: The starting player can guarantee a win by flipping the middle "++" to become "+--+".
```



**Follow up**:
Derive your algorithm's runtime complexity.



## 题目解读

&emsp;翻转游戏，判断先手是否必胜。

```java
class Solution {
    public boolean canWin(String s) {

    }
}
```

## 程序设计

* 可以采用回溯的方式。

```java
class Solution {
    public boolean canWin(String s) {
        return canWin(s.toCharArray(), new HashMap<>());
    }

    private boolean canWin(char[] seq, Map<String, Boolean> record) {
        String s = new String(seq);
        if (record.containsKey(s)) return record.get(s);

        boolean res = false;
        for (int i = 0; i < seq.length - 1; i++) {
            if (seq[i] == '-' || seq[i] != seq[i + 1]) continue;
            // 试探
            seq[i] = seq[i + 1] = '-';
            if (!canWin(seq, record)) res = true;
            // 回溯
            seq[i] = seq[i + 1] = '+';
        }
        
        record.put(s, res);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N2^N)$，空间复杂度为$O(2^N)$。

执行用时：22 ms, 在所有 Java 提交中击败了32.98%的用户。

内存消耗：40.2 MB, 在所有 Java 提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用巧妙定义的sg函数来解决。通过定义连续`+`的数目来表示当前状态转换，例如`-+++--+--++`，可拆分为$SG(3) \oplus SG(1) \oplus SG(2)$；而对于连续的一段$SG(x)$，选择其中两个进行翻转，以翻转位置为$i$为例，可拆分为$SG(i) \oplus SG(x - 2 - i)$，即是所有可能结果的集合，最终通过$\text{mex}$函数得到$SG(x)$，从而得到当前的级数。

```java
class Solution {
    public boolean canWin(String s) {
        int n = s.length();
        if (n < 2) return false;
        // 表示连续+的数目及对应的sg级数
        int[] sgArr = new int[n + 1];
        Arrays.fill(sgArr, -1);
        // 零态
        sgArr[0] = sgArr[1] = 0;
        // 一级胜态
        sgArr[2] = 1;

        int res = 0;
        int left = -1, right = 0;
        char[] seq = s.toCharArray();
        // 统计连续的+号
        while (right < seq.length) {
            if (seq[right] == '-') left = right;
            // 计算连续的+的程度
            else if (right == seq.length - 1 || seq[right + 1] == '-') {
                res ^= sg(right - left, sgArr);
            }
            right++;
        }
        return res != 0;
    }

    // SG函数
    private int sg(int num, int[] sgArr) {
        if (sgArr[num] != -1) return sgArr[num];

        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < num - 1; i++) {
            set.add(sg(i, sgArr) ^ sg(num - 2 - i, sgArr));
        }
        return sgArr[num] = mex(set);
    }

    private int mex(Set<Integer> set) {
        for (int i = 0; ; i++) {
            if (!set.contains(i)) return i;
        }
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：36.3 MB, 在所有 Java 提交中击败了91.25%的用户。