[toc]

Given an array `A` of `0`s and `1`s, divide the array into 3 non-empty parts such that all of these parts represent the same binary value.

If it is possible, return **any** `[i, j]` with $i+1 < j$, such that:

* `A[0]`, `A[1]`, ..., `A[i]` is the first part;
* `A[i+1]`, `A[i+2]`, ..., `A[j-1]` is the second part, and`A[j]`, `A[j+1]`, ..., `A[A.length - 1]` is the third part.
* All three parts have equal binary value.

If it is not possible, return `[-1, -1]`.

Note that the entire part is used when considering what binary value it represents.  For example, [1,1,0] represents 6 in decimal, not 3.  Also, leading zeros are allowed, so [0,1,1] and [1,1] represent the same value.



**Note:**

* $3 \le \text{A.length} \le 30000$
* $\text{A[i]} == 0$ or $\text{A[i]} == 1$



## 题目解读

&emsp;给定数组，判断是否可切分为三部分，每部分构成的二进制数相等。

```java
class Solution {
    public int[] threeEqualParts(int[] A) {

    }
}
```

## 程序设计

* 首先统计数组中1的个数，如果不是3的倍数，必然不可切分为相等的三部分；根据统计的数目初步定位第一部分的非0结尾`i`和第三部分的非0开头`j`，中间的值就是第一部分连续0结尾、第二部分和第三部分的连续0起始的序列。
* 由于第三部分结尾是确定的，如果第三部分结尾有连续的0，则第一部分`i`位置后也应该有连续的0，这样就可以得到完整的第一部分。
* 在得到第一部分后，第二部分的起始位置得到确定，可以分别遍历三个部分的非0起始，判断其后的数字是否一致，顺便确定第二部分的边界；得到第二部分边界后，最后需要判断第二部分结束到第三部分非0起始之间是否全是0。

```java
class Solution {
    public int[] threeEqualParts(int[] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid param");

        int count = 0;
        for (int bit : A) if (bit == 1) count++;
        // 1的个数不能被3整除
        if (count % 3 != 0) return new int[]{-1, -1};
        if (count == 0) return new int[]{0, A.length - 1};
        count /= 3;
        // 定位i，j的初始位置
        int i = 0, j = A.length - 1;
        while (count > 0) {
            while (A[i] != 1) i++;
            while (A[j] != 1) j--;
            count--;
            i++;
            j--;
        }
        i--;
        j++;

        // 根据第三部分结尾的0移动i，得到的第一部分为最终结果
        int k = A.length - 1;
        while (k >= j && A[k] == 0) {
            // 第一部分结尾不为0
            if (i + 1 >= j || A[i + 1] == 1) return new int[]{-1, -1};
            i++;
            k--;
        }

        // 根据第一部分矫正第二部分，同时比较第三部分
        int idx1 = 0, idx2 = i + 1, idx3 = j;
        // 定位到非0位置（第三部分j已经为非0起始）
        while (idx1 <= i && A[idx1] == 0) idx1++;
        while (idx2 < j && A[idx2] == 0) idx2++;
        while (idx1 <= i) {
            // 比较三部分的数值
            if (idx2 >= j || idx3 >= A.length || A[idx1] != A[idx2] || A[idx1] != A[idx3]) return new int[]{-1, -1};
            idx1++;
            idx2++;
            idx3++;
        }

        // 若idx2~j之间存在间隙，必须为0，即第三部分为j和高位的0组成
        for (; j > idx2; j--) {
            if (A[j - 1] != 0) return new int[]{-1, -1};
        }
        return new int[]{i, j};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了95.40%的用户。

内存消耗：44.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路类似，实现细节不同。