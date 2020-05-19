[toc]

X is a good number if after rotating each digit individually by 180 degrees, we get a valid number that is different from X.  Each digit must be rotated - we cannot choose to leave it alone.

A number is valid if each digit remains a digit after rotation. 0, 1, and 8 rotate to themselves; 2 and 5 rotate to each other (on this case they are rotated in a different direction, in other words 2 or 5 gets mirrored); 6 and 9 rotate to each other, and the rest of the numbers do not rotate to any other number and become invalid.

Now given a positive number N, how many numbers X from 1 to N are good?



**Note:**

- N will be in range `[1, 10000]`.



## 题目解读

&emsp;统计$1 \sim N$之间有多少个好数。

```java
class Solution {
    public int rotatedDigits(int N) {

    }
}
```

## 程序设计

* 暴力法遍历判断每一个数字。

```java
class Solution {
    public int rotatedDigits(int N) {
        int count = 0;
        if (N < 1) return 0;
        for (int i = 1; i <= N; i++) {
            if (isValid(i)) count++;
        }
        return count;
    }

    private boolean isValid(int n) {
        String num = Integer.toString(n);
        char[] nums = num.toCharArray();

        for (char c : nums) {
            if (c == '3' || c == '4' || c == '7') return false;
        }
        int left = 0, right = nums.length - 1;
        while (left < right) {
            // 旋转数字
            nums[left] = reverse(nums[left++]);
            nums[right] = reverse(nums[right--]);
        }
        if (left == right) nums[left] = reverse(nums[left]);
        // 比较旋转后是否相等
        return n != Integer.valueOf(new String(nums));
    }

    private char reverse(char c) {
        switch (c) {
            case '6':
                return '9';
            case '9':
                return '6';
            case '2':
                return '5';
            case '5':
                return '2';
            default:
                return c;
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：19ms，在所有java提交中击败了20.15%的用户。

内存消耗：39.3MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;官方提供了动态规划的思路。首先社区暴力法采用整数整除的思路，而非转化为字符串。

```java
class Solution {
    public int rotatedDigits(int N) {
        int count = 0;
        for (int i = 1; i <= N; i++){
            boolean flag = false;
            int num = i;
            while (num > 0){
                int remain = num % 10;
                // 不是好数
                if (remain == 3 || remain == 4 || remain == 7){
                    break;
                }
                // 存在2、5、6、9表示旋转后数字不相等，是好数
                if (remain == 2 || remain == 5 || remain == 6 || remain == 9){
                    flag = true;
                }
                // 继续迭代下一位
                num /= 10;
            }
            count = (num == 0 && flag) ? count + 1 : count;
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了62.79%的用户。

内存消耗：36.4MB，在所有java提交中击败了50.00%的用户。

&emsp;可见每个好数只能包含数字`0125689`，并且至少包含`2569`中的一个。社区思路是如果除去最低位，其他位是没有`347`也不包含`2569`的普通数，则低位决定当前数；如果已经是好数，只要低位不是`347`，仍然是好数。

```java
class Solution {
    public int rotatedDigits(int N) {
        int[] dp = new int[N + 1];//1好数，0普通数，-1坏数
        int count = 0;
        for (int i = 0; i <= N; i++) {
            if (i < 10) {
                //初始条件
                if (i ==2 || i == 5 || i == 6 || i == 9) {
                    dp[i] = 1;
                } else if (i==0 || i==8 || i==1) {
                    dp[i] = 0;
                } else {
                    dp[i] = -1;
                }
            } else {
                if (dp[i / 10] == -1 || dp[i % 10] == -1) {
                    dp[i] = -1;
                } else if (dp[i / 10]==0 && dp[i % 10]== 0) {
                    dp[i] = 0;
                } else {
                    dp[i] = 1;
                }
            }
            if (dp[i] == 1) {
                count++;
            }
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了93.99%的用户。

内存消耗：36.9MB，在所有java提交中击败了25.00%的用户。



```java
class Solution {
    public int rotatedDigits(int N) {
        char[] nums = Integer.toString(N).toCharArray();
        int len = nums.length;
        int[][][] dp = new int[len + 1][2][2];
        dp[len][0][1] = dp[len][1][1] = 1;

        for (int i = len - 1; i >= 0; i--) {
            for (int j = 0; j <= 1; j++) {
                for (int k = 0; k <= 1; k++) {
                    for (char a = '0'; a <= (j == 1 ? nums[i] : '9'); a++) {
                        // 存在３、４、７必然不是好数
                        if (a == '3' || a == '4' || a == '7') continue;
                        // 当前是否存在２、５、６、９，存在则说明是好数
                        boolean diff = a == '2' || a == '5' || a == '6' || a == '9';

                        dp[i][j][k] += dp[i + 1][a == nums[i] ? j : 0][diff ? 1 : k];
                    }

                }
            }
        }
        return dp[0][1][0];
    }
}
```

