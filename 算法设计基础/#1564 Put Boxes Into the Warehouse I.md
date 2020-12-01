[toc]

You are given two arrays of positive integers, `boxes` and `warehouse`, representing the heights of some boxes of unit width and the heights of $n$ rooms in a warehouse respectively. The warehouse's rooms are labelled from $0$ to $n - 1$ from left to right where `warehouse[i]` (0-indexed) is the height of the `ith` room.

Boxes are put into the warehouse by the following rules:

* Boxes cannot be stacked.
* You can rearrange the insertion order of the boxes.
* Boxes can only be pushed into the warehouse from left to right only.
* If the height of some room in the warehouse is less than the height of a box, then that box and all other boxes behind it will be stopped before that room.

Return the maximum number of boxes you can put into the warehouse.

 

**Example 1**:

<img src="..\images\#1564_exp1.png"  />

```
Input: boxes = [4,3,4,1], warehouse = [5,3,3,4,1]
Output: 3
Explanation: 
```

<img src="..\images\#1564_exp2.png"  />

```
We can first put the box of height 1 in room 4. Then we can put the box of height 3 in either of the 3 rooms 1, 2, or 3. Lastly, we can put one box of height 4 in room 0.
There is no way we can fit all 4 boxes in the warehouse.
```

**Example 2**:

<img src="..\images\#1564_exp3.png"  />

```
Input: boxes = [1,2,2,3,4], warehouse = [3,4,1,2]
Output: 3
Explanation: 
```

<img src="..\images\#1564_exp4.png"  />

```
Notice that it's not possible to put the box of height 4 into the warehouse since it cannot pass the first room of height 3.
Also, for the last two rooms, 2 and 3, only boxes of height 1 can fit.
We can fit 3 boxes maximum as shown above. The yellow box can also be put in room 2 instead.
Swapping the orange and green boxes is also valid, or swapping one of them with the red box.
```

**Example 3**:

```
Input: boxes = [1,2,3], warehouse = [1,2,3,4]
Output: 1
Explanation: Since the first room in the warehouse is of height 1, we can only put boxes of height 1.
```

**Example 4**:

```
Input: boxes = [4,5,6], warehouse = [3,3,3,3,3]
Output: 0
```



**Constraints**:

* $n == \text{warehouse.length}$
* $1 \le \text{boxes.length, warehouse.length} \le 10^5$
* $1 \le \text{boxes[i], warehouse[i]} \le 10^9$



## 题目解读

&emsp;给定仓库和箱子的高度，箱子只能从仓库左端向右放置，箱子放入顺序可随意调整，求可放入箱子的最大数目。

```java
class Solution {
    public int maxBoxesInWarehouse(int[] boxes, int[] warehouse) {

    }
}
```

## 程序设计

* 由于箱子只能从左向右放置，仓库的实际放入高度是从左向右递减的，可首先修改仓库高度，然后排序箱子，将最低的箱子与仓库最低处匹配放置。

```java
class Solution {
    public int maxBoxesInWarehouse(int[] boxes, int[] warehouse) {
        // 排序box
        Arrays.sort(boxes);
        // 处理仓库高度
        for (int i = 1; i < warehouse.length; i++) {
            if (warehouse[i] > warehouse[i - 1]) warehouse[i] = warehouse[i - 1];
        }

        int res = 0, boxIdx = 0, whIdx = warehouse.length - 1;
        // 贪婪匹配
        while (boxIdx < boxes.length && whIdx >= 0) {
            // 仓库高于箱子，放置
            if (boxes[boxIdx] <= warehouse[whIdx--]) {
                res++;
                boxIdx++;
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + M)$，空间复杂度为$O(1)$。

执行用时：19 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：51 MB, 在所有 Java 提交中击败了72.73%的用户。

## 官方解题

&emsp;暂无，密切关注。