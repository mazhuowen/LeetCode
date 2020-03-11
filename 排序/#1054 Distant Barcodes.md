[toc]

In a warehouse, there is a row of barcodes, where the `i`-th barcode is `barcodes[i]`.

Rearrange the barcodes so that no two adjacent barcodes are equal.  You may return any answer, and it is guaranteed an answer exists.

**Note:**

* $1 \le \text{barcodes.length} \le 10000$
* $1 \le \text{barcodes[i]} \le 10000$



## 题目解读

&emsp;排序使得相邻数字不相等。

```java
class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {

    }
}
```

## 程序设计

* 与不相邻字符类似，先计数，然后穿插如数组。

```java
class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {
        if(barcodes == null || barcodes.length == 0) {
            throw new IllegalArgumentException("invalid param");
        }
        // 计数
        Map<Integer, Integer> counter = new HashMap<>();
        for (int num : barcodes) counter.put(num, counter.getOrDefault(num, 0) + 1);
        // 最大堆排序
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> b[1] - a[1]);
        for (Map.Entry<Integer, Integer> entry : counter.entrySet()) {
            queue.add(new int[]{entry.getKey(), entry.getValue()});
        }
        int[] res = new int[barcodes.length];
        int idx = 0;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            // 检测是否满足条件
            if(cur[1] > (1 + barcodes.length) / 2) throw new IllegalArgumentException("error param");
            // 穿插，计数大的加入偶数索引，索引用完则加入奇数索引
            for (int i = cur[1]; i > 0; i--) {
                if(idx >= barcodes.length) {
                    idx = 1;
                }
                res[idx] = cur[0];
                idx += 2;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(max(M\log_2M,N))$，空间复杂度为$O(M)$，其中$M$为不同的数字数。

执行用时：30ms，在所有java提交中击败了69.59%的用户。

内存消耗：43.9MB，在所有java提交中击败了5.97%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有一种直接交换的思路：

```java
class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {
        int temp;
        // 检查偶数索引数字
        for (int i = 0; i < barcodes.length - 2; i++) {
            // 和邻接元素相等，则交换到后面的偶数索引
            if (barcodes[i] == barcodes[i + 1]) {
                for (int j = i + 2; j < barcodes.length; j++) {
                    if (barcodes[i] != barcodes[j]) {
                        temp = barcodes[j];
                        barcodes[j] = barcodes[i + 1];
                        barcodes[i + 1] = temp;
                        break;
                    }
                }
            }
        }
        // 上述偶数索引交换完后，若存在不满足条件的值val，则其后的偶数索引都是val，无法交换，此时需要从后往前遍历并交换
        for (int i = barcodes.length - 1; i > 1; i--) {
            if (barcodes[i] == barcodes[i - 1]) {
                for (int j = i - 2; j >= 0; j--) {
                    if (barcodes[i] != barcodes[j]) {
                        temp = barcodes[j];
                        barcodes[j] = barcodes[i - 1];
                        barcodes[i - 1] = temp;
                        break;
                    }
                }
            }
        }
        return barcodes;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了100.00%的用户。

内存消耗：43MB，在所有java提交中击败了7.46%的用户。