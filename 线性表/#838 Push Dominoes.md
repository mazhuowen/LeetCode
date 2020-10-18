[toc]

There are $N$ dominoes in a line, and we place each domino vertically upright.

In the beginning, we simultaneously push some of the dominoes either to the left or to the right.

<img src="..\images\#838.png" style="zoom: 80%;" />

After each second, each domino that is falling to the left pushes the adjacent domino on the left.

Similarly, the dominoes falling to the right push their adjacent dominoes standing on the right.

When a vertical domino has dominoes falling on it from both sides, it stays still due to the balance of the forces.

For the purposes of this question, we will consider that a falling domino expends no additional force to a falling or already fallen domino.

Given a string "S" representing the initial state. `S[i] = 'L'`, if the i-th domino has been pushed to the left; `S[i] = 'R'`, if the i-th domino has been pushed to the right; `S[i] = '.'`, if the `i`-th domino has not been pushed.

Return a string representing the final state. 



**Note:**

* $0 \le N \le 10^5$
* String `dominoes` contains only `'L`', `'R'` and `'.'`



## 题目解读

&emsp;给定多米诺骨牌的左右倒向方向，求最后的状态。

```java
class Solution {
    public String pushDominoes(String dominoes) {

    }
}
```

## 程序设计

* 只有左侧是`R`右侧是`L`才会两边向中间倒，其他都是一边倒，可用双指针检测前后指令并作出判断。

```java
class Solution {
    public String pushDominoes(String dominoes) {
        char[] seq = dominoes.toCharArray();
        // 指针
        int left = 0, right = 0;
        while (right < seq.length) {
            // 倒向左侧
            if (seq[right] == 'L') {
                while (left < right) seq[left++] = 'L';
            } 
            // 倒向右侧，检测其后是否存在L
            else if (seq[right] == 'R') {
                left = right++;
                while (right < seq.length && seq[right] == '.') right++;
                // 倒向右侧
                if (right == seq.length || seq[right] == 'R') {
                    while (left < right) seq[left++] = 'R';
                    right--;
                }
                // 倒向中间
                else {
                    int tmp = right;
                    while (left < right) {
                        seq[left++] = 'R';
                        seq[right--] = 'L';
                    }
                    left = right = tmp;
                }
            }
            right++;
        }
        return new String(seq);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.2MB，在所有java提交中击败了99.45%的用户。

## 官方解题

&emsp;官方思路不够简洁，不做介绍。