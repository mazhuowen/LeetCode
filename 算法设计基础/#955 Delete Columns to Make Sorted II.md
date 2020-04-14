[toc]

We are given an array `A` of `N` lowercase letter strings, all of the same length.

Now, we may choose any set of deletion indices, and for each string, we delete all the characters in those indices.

For example, if we have an array `A = ["abcdef","uvwxyz"]` and deletion indices `{0, 2, 3}`, then the final array after deletions is `["bef","vyz"]`.

Suppose we chose a set of deletion indices `D` such that after deletions, the final array has its elements in **lexicographic** order ($A[0] \le A[1] \le A[2] \dots \le A[A.length - 1]$).

Return the minimum possible value of `D.length`.



**Note:**

* $1 \le \text{A.length} \le 100$
* $1 \le \text{A[i].length} \le 100$



## 题目解读

&emsp;给定字符串数组，删除特定的列，使得数组中的字符串序列是按照字典序排序的。

```java
class Solution {
    public int minDeletionSize(String[] A) {

    }
}
```

## 程序设计

* 最基本的思想是遍历所有字符串同一位置的字符，如果出现后一个字符小于前一个字符，则这两个字符串满足字典序，如果当前位置都满足字典序，则说明删除方法已找到，可退出循环。以`abc`、`vnd`、`csa`为例，首先判断第一个字符，发现不满足字典序，删除；然后遍历第二个字符，发现满足字典序，退出。
* 但是上述情况会漏掉当前字符相同，但后续字符不满足字典序的情况，如`abc`、`aac`、`bca`，如果只检查第一位的话会得得到错误结果；修改上述流程，即不从字符位统一开始遍历，对前后字符从头遍历删除，但这样也存在问题，如`cad`、`cac`、`fbb`、`fav`，遍历第一个字符，符合字典序，遍历第二个字符，发现`fbb`和`fav`不符合要求，删除第二个字符，但是这样`cd`和`cc`这两个字符串又不满足要求了。
* 结合上述思想，首先以字符位在最外层遍历依次判断前后两个字符串是否满足字典序，不满足则标记并判断下一位；满足则需要判断两个字符串在当前位置字符是否相等，相等则需要遍历后续位置检查是否满足字典序。

```java
class Solution {
    public int minDeletionSize(String[] A) {
        if(A == null || A.length <= 1) return 0;
        int n = A.length, m = A[0].length();
        boolean[] delete = new boolean[m];

        int count = 0;
        // 从第k位开始遍历比较
        for (int k = 0; k < m; k++) {
            if (delete[k]) continue;
            // 依次比较前后两个字符串
            String base = A[0];
            for (int i = 1; i < n; i++) {
                String cur = A[i];
                // 从第k位开始比较删除，对于首字符不同的，直接删除k，如果首字符相同，则比较后续字符并删除
                for (int j = k; j < m; j++) {
                    if (delete[j] || base.charAt(j) == cur.charAt(j)) continue;
                    // 满足字典序，直接跳出循环，进入下一位判断
                    if (base.charAt(j) < cur.charAt(j)) break;
                    // 删除并标记
                    else {
                        delete[j] = true;
                        count++;
                    }
                }
                base = cur;
            }
            // 第k位符合要求（上述的内循环保证了第k位字符相等，其后字符不满足条件会删除，从而此处可退出循环）
            if (!delete[k]) break;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M^2N)$，空间复杂度为$O(M)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方的思路从已遍历的字符串出发，而不是当前位。将已经遍历过的，字符不相等且符合字典序的序列标记为无需判断，在下次比较中不会再判断之前比较过的符合要求的字符串，只会判断前一个字符相等，当前字符是否符合情况。

```java
class Solution {
    public int minDeletionSize(String[] A) {
        if(A == null || A.length <= 1) return 0;
        int n = A.length, m = A[0].length();
        // 后续不需要判断的字符组，第i个表示i和i+1字符串不需要比较
        boolean[] unneed = new boolean[n - 1];

        int count = 0;
        // 从第一位字符开始比较
        for (int k = 0; k < m; k++) {
            boolean flag = true;
            for (int i = 0; i < n - 1; i++) {
                // 第k列不符合要求，删除并计数，进入下一位比较
                if (!unneed[i] && A[i].charAt(k) > A[i + 1].charAt(k)) {
                    count++;
                    flag = false;
                    break;
                }
            }
            // 比较完毕，第k列字符符合要求，则考虑字符相等的情况，将不相等的置为true，后续无需判断
            if (flag) {
                for (int i = 0; i < n - 1; i++) {
                    if (A[i].charAt(k) < A[i + 1].charAt(k)) unneed[i] = true;
                }
            }
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.6MB，在所有java提交中击败了100.00%的用户。