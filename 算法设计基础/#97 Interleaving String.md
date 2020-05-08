[toc]

Given `s1`, `s2`, `s3`, find whether `s3` is formed by the interleaving of `s1` and `s2`.



## 题目解读

&emsp;判断两个字符串是否可交错组成给定字符串。

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {

    }
}
```

## 程序设计

* 可使用回溯暴力遍历尝试。

```java
class Solution {
    char[] str1, str2, target;
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1 == null || s2 == null || s3 == null) throw new IllegalArgumentException("invalid param");
        if (s1.isEmpty()) return s3.equals(s2);
        if (s2.isEmpty()) return s3.equals(s1);
        if (s1.length() + s2.length() != s3.length()) return false;

        str1 = s1.toCharArray();
        str2 = s2.toCharArray();
        target = s3.toCharArray();
        return isInterleave(0, 0, 0, true) || isInterleave(0, 0, 0, false);
    }

    // turn表示当前是哪个字符串拼接
    private boolean isInterleave(int idx1, int idx2, int idx3, boolean turn) {
        if (idx1 >= str1.length && idx2 >= str2.length 
            	&& idx3 >= target.length) return true;

        // 当前匹配串索引无效，或者目标串已经匹配完
        if (turn && idx1 >= str1.length || !turn && idx2 >= str2.length 
                || idx3 >= target.length) return false;
        
        // 当前字符不匹配
        if (turn && str1[idx1++] != target[idx3++] ||
                !turn && str2[idx2++] != target[idx3++]) return false;

        // 选择继续当前字符或切换为下一个字符串
        return isInterleave(idx1, idx2, idx3, turn)
                || isInterleave(idx1, idx2, idx3, !turn);
    }
}
```

* 上述回溯每次要么选择继续在当前字符串遍历，要么切换到另一个字符串遍历，从而模拟交错的情况，必然存在重复尝试的情况。如果遍历时能够记录当前索引之后的串是否可以构成剩余的目标串，就可以节省时间。
* 但是题目强调交错，只记录`i`、`j`之后的字符串是否可构成`i + j`之后的目标串，能否和当前的`turn`匹配？实际上不管之后的串是从第一个字符串还是第二个字符串起始，构成的字符串仍然是交错的，只要不打乱字符在各自字符串的顺序，构成的字符串都是交错的。
* 这样就可以引入额外的空间记录中间结果，加速算法。

```java
class Solution {
    char[] str1, str2, target;
    int[][] record;

    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1 == null || s2 == null || s3 == null) throw new IllegalArgumentException("invalid param");
        if (s1.isEmpty()) return s3.equals(s2);
        if (s2.isEmpty()) return s3.equals(s1);
        if (s1.length() + s2.length() != s3.length()) return false;

        str1 = s1.toCharArray();
        str2 = s2.toCharArray();
        target = s3.toCharArray();

        record = new int[str1.length + 1][str2.length + 1];

        return isInterleave(0, 0, true) || isInterleave(0, 0, false);
    }

    // turn表示当前是哪个字符串拼接
    private boolean isInterleave(int idx1, int idx2, boolean turn) {
        if (record[idx1][idx2] != 0) return record[idx1][idx2] == 1;

        if (idx1 >= str1.length && idx2 >= str2.length) {
            record[idx1][idx2] = 1;
            return true;
        }

        // 当前匹配串索引无效
        if (turn && idx1 >= str1.length || !turn && idx2 >= str2.length)
            return false;
        
        // 当前字符不匹配
        if (turn && str1[idx1] != target[idx1++ + idx2] ||
                !turn && str2[idx2] != target[idx1 + idx2++])
            return false;

        // 选择继续当前字符或切换为下一个字符串
        boolean flag =  isInterleave(idx1, idx2, turn)
                || isInterleave(idx1, idx2, !turn);
        
        // 因为turn有两种情况，当前方法只是其中一种，过早记录会造成错误；
        // 此处idx1和idx2与输入时不相等，且均已完成所有情况遍历，记录
        if (flag) record[idx1][idx2] = 1;
        else record[idx1][idx2] = -1;
        return flag;
    }
}
```

* 有了上述思路，可直接使用动态规划实现。`dp(i,j)`表示字符串1的`0~i`和字符串2的`0~j`可构成前`i + j +１`个目标字符串。

```java
class Solution {

    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1 == null || s2 == null || s3 == null) throw new IllegalArgumentException("invalid param");
        if (s1.isEmpty()) return s3.equals(s2);
        if (s2.isEmpty()) return s3.equals(s1);
        if (s1.length() + s2.length() != s3.length()) return false;

        char[] str1 = s1.toCharArray(), str2 = s2.toCharArray(), target = s3.toCharArray();

        boolean[][] dp = new boolean[str1.length + 1][str2.length + 1];
        for (int i = 0; i <= str1.length; i++) {
            for (int j = 0; j <= str2.length; j++) {
                // 初始
                if (i == 0 && j == 0) dp[i][j] = true;
                else if (i == 0) {
                    dp[i][j] = str2[j - 1] == target[j - 1] && dp[i][j - 1];
                } else if (j == 0) {
                    dp[i][j] = str1[i - 1] == target[i - 1] && dp[i - 1][j];
                } else {
                    dp[i][j] = str2[j - 1] == target[i + j - 1] && dp[i][j - 1] || str1[i - 1] == target[i + j - 1] && dp[i - 1][j];
                }
            }
        }

        return dp[str1.length][str2.length];
    }
}
```

* 可继续优化空间：

```java
class Solution {

    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1 == null || s2 == null || s3 == null) throw new IllegalArgumentException("invalid param");
        if (s1.isEmpty()) return s3.equals(s2);
        if (s2.isEmpty()) return s3.equals(s1);
        if (s1.length() + s2.length() != s3.length()) return false;

        char[] str1 = s1.toCharArray(), str2 = s2.toCharArray(), target = s3.toCharArray();

        boolean[] dp = new boolean[str2.length + 1];
        for (int i = 0; i <= str1.length; i++) {
            for (int j = 0; j <= str2.length; j++) {
                // 初始
                if (i == 0 && j == 0) dp[j] = true;
                else if (i == 0) {
                    dp[j] = str2[j - 1] == target[j - 1] && dp[j - 1];
                } else if (j == 0) {
                    dp[j] = str1[i - 1] == target[i - 1] && dp[j];
                } else {
                    dp[j] = str2[j - 1] == target[i + j - 1] && dp[j - 1] || str1[i - 1] == target[i + j - 1] && dp[j];
                }
            }
        }

        return dp[str2.length];
    }
}
```

## 性能分析

&emsp;最坏的情况类似`aa…a`、`aa…aa`，判断是否组成`aa…ab`，此时时间复杂度为$O(2^{M + N})$，空间复杂度为$O(M + N)$。

执行用时：2106ms，在所有java提交中击败了6.38%的用户。

内存消耗：37.9MB，在所有java提交中击败了14.29%的用户。

&emsp;引入额外空间后时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时:1ms，在所有java提交中击败了99.53%的用户。

内存消耗:38.3MB，在所有java提交中击败了14.29%的用户。

&emsp;动态规划时间复杂度为$O(MN)$，空间复杂度为$O(MN)$，优化空间后为$O(N)$。

执行用时：4ms，在所有java提交中击败了74.46%的用户。

内存消耗：37.8MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;同上。