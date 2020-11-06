[toc]

Given two strings `s` and `t`, you want to transform string `s` into string `t` using the following operation any number of times:

* Choose a **non-empty** substring in s and sort it in-place so the characters are in **ascending order**.

For example, applying the operation on the underlined substring in `"14234"` results in `"12344"`.

Return `true` if it is possible to transform string `s` into string `t`. Otherwise, return `false`.

A **substring** is a contiguous sequence of characters within a string.



**Constraints**:

* $\text{s.length} == \text{t.length}$
* $1 \le \text{s.length} \le 10^5$
* `s` and `t` only contain digits from `'0'` to `'9'`.



## 题目解读

&emsp;判断是否可转换为目标字符串。

```java
class Solution {
    public boolean isTransformable(String s, String t) {

    }
}
```

## 程序设计

* 如`84532`转化为`34852`，第一步排序`53`得到`84352`，第二部排序`843`得到`34852`，如果从模拟的角度看，由于存在重叠的排序区域，问题复杂不易抽象；
* 由于题目只是判断是否可得到转换后的字符串，而非转换的最少次数，可以将子字符串排序分解为类似冒泡排序的两两交换，因为不管子区间排序多少次，较小的值都只能往前，较大的值只能往后，仍然以`84532`为例，首先将`3`交换到第一个位置，然后交换`4`，依次类推得到最后的结果；
* 可见每次需要交换到前面的数字是在未交换的数字中的最小的值，不满足这个条件就无法得到目标字符串。

```java
class Solution {
    public boolean isTransformable(String s, String t) {
        if (s == null || t == null || s.length() != t.length()) return false;
        int n = s.length();
        // 统计每个数字的索引，保存未交换的字符
        Stack<Integer>[] index = new Stack[10];
        for (int i = 0; i < 10; i++) index[i] = new Stack<>();
        for (int i = 0; i < n; i++) {
            index[s.charAt(i) - '0'].push(i);
        }

        // 遍历目标字符串
        for (int i = 0; i < n; i++) {
            int digit = t.charAt(i) - '0';
            // 不存在未交换的目标字符，无法交换达成
            if (index[digit].isEmpty()) return false;
            // 目标字符不能大于其前的未交换字符
            for (int j = 0; j < digit; j++) {
                if (!index[j].isEmpty() && index[j].peek() < index[digit].peek()) return false;
            }
            // 交换目标字符，出队
            index[digit].pop();
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NC)$，空间复杂度为$O(N)$。

执行用时：30ms，在所有java提交中击败了70.52%的用户。

内存消耗：47.1MB，在所有java提交中击败了40.46%的用户。

## 官方解题

&emsp;官方思路同上。