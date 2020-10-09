[toc]

In a string composed of `'L'`, `'R'`, and `'X'` characters, like `"RXXLRXRXL"`, a move consists of either replacing one occurrence of `"XL"` with `"LX"`, or replacing one occurrence of `"RX"` with `"XR"`. Given the starting string `start` and the ending string `end`, return `True` if and only if there exists a sequence of moves to transform one string to the other.



**Constraints**:

* $1 \le \text{start.length} \le 10^4$
* $\text{start.length} == \text{end.length}$
* Both `start` and `end` will only consist of characters in `'L'`, `'R'`, and `'X'`.



## 题目解读

&emsp;判断两个字符串是否可以转换得到。

```java
class Solution {
    public boolean canTransform(String start, String end) {

    }
}
```

## 程序设计

* 粗略看似乎是遍历模拟交换，但是对于`"XLXRRXXRXX"`转化为`"LXXXXXXRRR"`的序列很难单方向模拟；可以看到`L`只能向左移动，`R`只能向右移动，即变换后的`L`一定在原始`L`的前面，`R`在后面；
* 其次`L`和`R`不能相互穿透。

```java
class Solution {
    public boolean canTransform(String start, String end) {
        // 检测L和R的数目及相对位置是否一致，不一致返回
        if (!start.replace("X", "").equals(end.replace("X", ""))) return false;

        // 检测变换后的L在原始L之前
        int t = 0;
        for (int i = 0; i < start.length(); ++i) {
            if (start.charAt(i) == 'L') {
                while (end.charAt(t) != 'L') t++;
                if (i < t++) return false;
            }
        }

        // 检测变换后的R在原始R之前
        t = 0;
        for (int i = 0; i < start.length(); ++i) {
            if (start.charAt(i) == 'R') {
                while (end.charAt(t) != 'R') t++;
                if (i > t++) return false;
            }
        }
        return true;
    }
}
```

* 可直接利用性质遍历两个数组中的`L`或`R`，变换后的`L`索引一定小于等于原始索引，变换后的`R`索引一定大于等于原始索引；其次在遍历过程中若一方匹配到`L`或`R`，而另一方遍历到末尾都未匹配到，数目不对等，返回；遍历结束若一方还未遍历完，则检测剩余部分是否是`X`。

```java
class Solution {
    public boolean canTransform(String start, String end) {
        char[] S = start.toCharArray(), T = end.toCharArray();
        // 遍历两个数组
        int i = 0, j = 0;
        while (i < S.length && j < T.length) {
            // 跳过X
            while (i < S.length && S[i] == 'X') i++;
            while (j < T.length && T[j] == 'X') j++;
            if (i < S.length && j < T.length) {
                // 由于L、R不可穿透，此时必须相等
                // 变换后的L在前面
                // 变换后的R在后面 
                if (S[i] != T[j] || S[i] == 'L' && i < j || S[i] == 'R' && i > j) return false;
            }
            // 当前R或L有一方缺失
            else if (i < S.length || j < T.length) {
                return false;
            }
            i++;
            j++;
        }

        // 一方已经全部匹配完（L或R在末尾），另一方检测剩余部分是否全部是X
        while (i < S.length) if (S[i++] != 'X') return false;
        while (j < T.length) if (T[j++] != 'X') return false;
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了64.05%的用户。

内存消耗：39.6MB，在所有java提交中击败了11.38%的用户。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.8MB，在所有java提交中击败了84.43%的用户。

## 官方解题

&emsp;上述思路参考官方。