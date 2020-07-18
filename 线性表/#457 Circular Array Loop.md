[toc]

ou are given a **circular** array `nums` of positive and negative integers. If a number $k$ at an index is positive, then move forward $k$ steps. Conversely, if it's negative (-$k$), move backward $k$ steps. Since the array is circular, you may assume that the last element's next element is the first element, and the first element's previous element is the last element.

Determine if there is a loop (or a cycle) in `nums`. A cycle must start and end at the same index and the cycle's length > 1. Furthermore, movements in a cycle must all follow a single direction. In other words, a cycle must not consist of both forward and backward movements.



**Note:**

* $-1000 \le \text{nums[i]} \le 1000$
* $\text{nums[i]} \ne 0$
* $1 \le \text{nums.length} \le 5000$



## 题目解读

&emsp;判断数组中是否存在循环。

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {

    }
}
```

## 程序设计

* 根据题目，循环方向必须是一致的，其次循环判断数目不能超过数组大小，避免造成死循环。

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            // 顺时针逆时针
            int next = getIdx(i + nums[i], nums.length);
            // 循环长度记录
            int len = 1;
            while (next != i){
                // 方向不一致或超出数组限制
                if (nums[next] * nums[i] < 0 || len > nums.length) break;
                next = getIdx(next + nums[next], nums.length);
                len++;
            }
            if (i == next && len > 1) return true;
        }
        return false;
    }

    private int getIdx(int idx, int len) {
        if (idx >= 0 && idx < len) return idx;
        if (idx < 0) return (len + idx % len) % len;
        else return idx % len;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：8ms，在所有java提交中击败了46.23%的用户。

内存消耗：36.9MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区使用快慢指针遍历判断环，若不是环，则将路径上的点置为0，避免重复遍历。

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) continue;
            // 快慢指针
            int slow = i, fast = i;
            do {
                slow = next(slow, nums);
                fast = next(fast, nums);
                // 方向不一致
                if (nums[fast] * nums[i] <= 0) break;
                fast = next(fast, nums);
                if (nums[fast] * nums[i] <= 0) break;
            } while(slow != fast);
            
            // 判断长度是否符合要求，即fast后面不能是fast（因为fast必然在环内，i可能不在环内）
            // 且旋转方向要与初始开始方向相同
            if (slow == fast && fast != next(fast, nums) 
                && nums[fast] * nums[i] > 0) return true;

            // 不是环，将遍历过的标记为0
            slow = next(i, nums);
            while (nums[i] * nums[slow] > 0) {
                int temp = slow;
                slow = next(slow, nums);
                nums[temp] = 0;
            }
            nums[i] = 0;
        }

        return false;
    }

    // 获取下一个索引
    private int next(int cur, int[] nums) {
        return getIdx(cur + nums[cur], nums.length);
    }
	// 规范索引
    private int getIdx(int idx, int len) {
        if (idx >= 0 && idx < len) return idx;
        idx %= len;
        if (idx < 0) idx += len;
        return idx;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了50.00%的用户。