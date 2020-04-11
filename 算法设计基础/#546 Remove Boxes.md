[toc]

Given several boxes with different colors represented by different positive numbers.
You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (composed of k boxes, k >= 1), remove them and get `k*k` points.
Find the maximum points you can get.

**Note:** The number of boxes `n` would not exceed 100.



## 题目解读

&emsp;给定多个箱子，每个箱子的编号代表一种颜色，移除箱子直到没有箱子为止；每次可以移除颜色相同的连续的箱子，假设有`k`个，可获得`k*k`分。给出可得到的最大分。

```java
class Solution {
    public int removeBoxes(int[] boxes) {

    }
}
```

## 程序设计

* 首先想到的是暴力回溯，每次需要记录和当前位置相连接的点并计算。时间复杂度为$O(N!)$，会超时。

```java
class Solution {
    int maxPoint = 0;

    public int removeBoxes(int[] boxes) {
        if (boxes == null || boxes.length == 0) return 0;

        // 便于排除重复试探
        Integer pre = null;
        for (int i = 0; i < boxes.length; i++) {
            if (pre != null && boxes[i] == pre) continue;
            removeBoxes(boxes, i, boxes.length, 0);
            pre = boxes[i];
        }
        return maxPoint;
    }

    // start移除点，num剩余盒子数目
    private void removeBoxes(int[] boxes, int start, int num, int curPoint) {
        // 移除点索引记录
        Set<Integer> idx = new HashSet<>();

        int left = start, right = start, target = boxes[start];
        while (left >= 0 && (boxes[left] == target || boxes[left] == Integer.MIN_VALUE)) {
            // 标记为移除
            if (boxes[left] != Integer.MIN_VALUE) {
                idx.add(left);
                boxes[left] = Integer.MIN_VALUE;
            }
            left--;
        }
        while (right < boxes.length && (boxes[right] == target || boxes[right] == Integer.MIN_VALUE)) {
            // 标记为移除
            if (boxes[right] != Integer.MIN_VALUE) {
                idx.add(right);
                boxes[right] = Integer.MIN_VALUE;
            }
            right++;
        }
        // 更新分数和剩余盒子
        curPoint += idx.size() * idx.size();
        num -= idx.size();
        // 找到一组解，更新
        if (num == 0) {
            maxPoint = Math.max(maxPoint, curPoint);
        }
        // 继续试探
        else {
            // 便于排除重复试探
            Integer pre = null;
            for (int i = 0; i < boxes.length; i++) {
                if (boxes[i] != Integer.MIN_VALUE && (pre == null || boxes[i] != pre)) {
                    removeBoxes(boxes, i, num, curPoint);
                    pre = boxes[i];
                }
            }
        }
        // 回溯
        for (int i : idx) {
            boxes[i] = target;
        }
    }
}
```

* 参考官方思路，优化的回溯方法采用三维数组记录，分别表示带移除的左右区间，即右侧区间之后与其值相等的连续序列数。对于`1,3,2,2,2,3,4,3,1`，`dp(0,0,0)`表示区间`(0,0)`其后相等的值为0，显然值是1；区间`dp(0,2,2)`表示区间`(0,2)`其后有两个相等值，显然一种转换为`dp(0,2,2) = dp(0,1,0) + 3 * 3`，即先移除`(0,1)`区间，然后移除`(2,4)`区间；另外的转换是当区间内存在与右侧区间相等的值时，如`dp(1,7,0)`，区间间索引`5`处与右边界相等，则可以尝试先消除`(6,6)`区间，使得中等的值连在一起，转变为`dp(6,6,0) +dp(1,6,1)`。而当前情况的最大值就是上述情况的最大值。

```java
class Solution {

    public int removeBoxes(int[] boxes) {
        int[][][] record = new int[boxes.length][boxes.length][boxes.length];
        return removeBoxes(boxes, record, 0, boxes.length - 1, 0);
    }

    /**
     * @param boxes 盒子数组
     * @param record 记录数组
     * @param start 起始区间
     * @param end 结束区间
     * @param same 结束区间后连续相同的数
     */
    private int removeBoxes(int[] boxes, int[][][] record, int start, int end, int same) {
        if (start > end) return 0;
        if (record[start][end][same] != 0) return record[start][end][same];

        // 如果右边界前还有相等的值，收缩
        int endTemp = end, sameTemp = same;
        while (end > start && boxes[end] == boxes[end - 1]) {
            end--;
            same++;
        }
        // 区间start到end，end后有same个相同值序列，
        // 则先移除start到end之间的盒子，再移除end及之后same个盒子的得分为：
         record[start][endTemp][sameTemp] = record[start][end][same] = removeBoxes(boxes, record, start, end - 1, 0) + (same + 1) * (same + 1);

        for (int i = start; i < end; i++) {
            if (boxes[i] != boxes[end]) continue;
            // 在start和end间找到一个和end相同的值i，先移除i+1到end-1间的盒子，然后移除start到i及end和以后same个盒子
             record[start][endTemp][sameTemp] = record[start][end][same] = Math.max(record[start][end][same],
                    removeBoxes(boxes, record, i + 1, end - 1, 0) + removeBoxes(boxes, record, start, i, same + 1));
        }
        return record[start][end][same];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^3)$。

执行用时：11ms，在所有java提交中击败了95.92%的用户。

内存消耗：49.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方的暴力搜索法比较简洁，每次移除会创建新的数组，而不是回溯。

```java

public class Solution {
    public int removeBoxes(int[] boxes) {
        return remove(boxes);
    }
    
    public int remove(int[] boxes) {
        if (boxes.length==0) return 0;
        
        int res=0;
        for (int i = 0, j = i + 1; i < boxes.length; i++) {
            // i、j为相等的连续序列
            while (j < boxes.length && boxes[i] == boxes[j]) j++;
            // 删去i～j
            int[] newboxes = new int[boxes.length - (j - i)];
            // 遍历将数组复制到新数组
            for (int k = 0, p = 0; k < boxes.length; k++) {
                // 跳过i～j
                if(k==i) k=j;
                if(k < boxes.length) newboxes[p++]=boxes[k];
            }
            // 递归
            res = Math.max(res, remove(newboxes) + (j - i) * (j - i));
        }
        return res;
    }
}
```