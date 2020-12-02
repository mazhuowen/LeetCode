[toc]

Given an integer array `nums` and a positive integer $k$, return the most **competitive** subsequence of `nums` of size $k$.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence `a` is more **competitive** than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number **less** than the corresponding number in `b`. For example, $[1,3,4]$ is more competitive than $[1,3,5]$ because the first position they differ is at the final number, and $4$ is less than $5$.

 

**Example 1**:

```
Input: nums = [3,5,2,6], k = 2
Output: [2,6]
Explanation: Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.
```

**Example 2**:

```
Input: nums = [2,4,3,3,5,4,9,6], k = 4
Output: [2,3,3,4]
```



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* $0 \le \text{nums[i]} \le 10^9$
* $1 \le k \le \text{nums.length}$



## 题目解读

&emsp;寻找最具竞争力的子序列，竞争力指序列所组成的数字最小。

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {

    }
}
```

## 程序设计

* 由于实际是求给定数组的长为$K$的子序列的最小字典序，可使用贪婪思路，遍历数组，遇到较小的值则替换序列值；
* 需注意可替换的范围，对于$i \in [0, n - 1]$，如果$n - i \ge K$即$i$后的元素有多于$K$个，此时$i$位置的值可替换子序列中$[0, K - 1]$中的第一个大于它的值；如果$n - i < K$，此时$i$位置可替换$[K - n + i, K - 1]$范围内的值，因为如果$i$替换到$K - n + i$前面时，其后的元素不足以填充长度为$K$的子序列。

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        if (nums == null || nums.length < k) throw new IllegalArgumentException("invalid param");
        int left, right = 0;
        int[] res = new int[k];
        for (int i = 0; i < nums.length; i++) {
            // 根据数据位置确定放置的区间范围
            if (nums.length - i >= k) left = 0;
            else left = k - nums.length + i;
            // 二分查找确定替换位置
            int replace = binarySearch(nums[i], res, left, right);
            if (replace < k) {
                res[replace] = nums[i];
                // 需收缩右侧边界
                right = replace + 1;
            }
        }
        return res;
    }

    // 找到大于target的第一个数值
    private int binarySearch(int target, int[] array, int left, int right) {
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (array[mid] > target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2K)$，空间复杂度为$O(K)$。

执行用时：47 ms, 在所有 Java 提交中击败了20.73%的用户。

内存消耗：50.2 MB, 在所有 Java 提交中击败了66.85%的用户。

## 官方解题

&emsp;暂无，密切关注。社区更巧妙的使用单调栈的思路来解决，栈中保存单调递增序列即可。

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        Stack<Integer> stack = new Stack<>();
        for (int i = 0 ; i < nums.length; i++) {
            // 如果当前元素小于栈中值且后续有足够元素，则出栈
            while (!stack.isEmpty() && nums[i] < stack.peek() && stack.size() + nums.length - i > k) {
                stack.pop();
            }
            // 入栈
            if (stack.size() < k) stack.push(nums[i]);
        }

        int[] res = new int[k];
        while (k > 0) {
            res[--k] = stack.pop();
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(K)$。

执行用时：43 ms, 在所有 Java 提交中击败了32.97%的用户

内存消耗：50.1 MB, 在所有 Java 提交中击败了73.18%的用户。

&emsp;使用数组代替栈，优化得：

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        if (nums == null || nums.length < k) throw new IllegalArgumentException("invalid param");
        int idx = 0;
        int[] res = new int[k];
        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            while (idx > 0 && res[idx - 1] > num && idx + nums.length - i > k) idx--;
            if (idx < k) res[idx++] = num;
        }
        return res;
    }
}
```

执行用时：7 ms, 在所有 Java 提交中击败了97.98%的用户

内存消耗：50.2 MB, 在所有 Java 提交中击败了66.85%的用户。