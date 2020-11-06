[toc]

Given an array of integers, find out whether there are two distinct indices `i` and `j` in the array such that the **absolute** difference between `nums[i]` and `nums[j]` is at most `t` and the **absolute** difference between `i` and `j` is at most `k`.



## 题目解读

&emsp;给定数组，检查是否存在索引相对位置小于等于$k$，两个值的差的绝对值小于等于$t$。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {

    }
}
```

## 程序设计

* 最基本的思路是每遍历一个位置，就遍历其后$k$个结点并做判断。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if(k <= 0 || t < 0) {
            return false;
        }
        for(int i = 0; i < nums.length - 1; i++) {
            for(int j = i + 1; j <= i + k && j < nums.length; j++) {
                // 注意溢出，转为long计算
                if(Math.abs((long)nums[j] - nums[i]) <= t) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

* 从窗口的角度观察，维护一个长度为$k$的队列，当前入队值如果与队列中最接近入队值的差满足条件则说明成立，否则不成立继续遍历。这个思路维护与当前值最接近的值成为重点。可以使用红黑树实现。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if(k <= 0 || t < 0) {
            return false;
        }
        TreeSet<Integer> set = new TreeSet<>();
        for(int i = 0; i < nums.length; i++) {
            // pre和next是长度为k的窗口中最接近当前数值的值，其差也是最小的
            // 检查最小的差是否满足要求，不满足要求则其它值也肯定不满足
            Integer pre = set.floor(nums[i]);
            // 转为long，避免数据溢出
            if(pre != null && (long)pre + t >= nums[i]) return true;
            Integer next = set.ceiling(nums[i]);
            if(next != null && (long)nums[i] + t >= next) return true;
            // 将当前值加入集合
            set.add(nums[i]);
            // 始终将窗口维护在k
            if(set.size() > k) set.remove(nums[i - k]);
        }
        return false;
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(Nk)$，空间复杂度为$O(1)$。

执行用时：639ms，在所有java提交中击败了5.09%的用户。

内存消耗：39.2MB，在所有java提交中击败了5.07%的用户。

&emsp;红黑树法时间复杂度为$O(N\log_2k)$，空间复杂度为$O(k)$。

执行用时：21ms，在所有java提交中击败了71.03%的用户。

内存消耗：41.2MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;除了上述方法，官方还提供了桶的思路。既然相差不能超过$t$，则可以设置桶的尺寸为$t$，如果$k+1$个长度的序列中存在同一桶中的数，表示差小于$t$，满足条件；除了同一桶，还有前后桶也需要判断差值是否小于$t$。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if(nums == null || nums.length == 0 || k <= 0 || t < 0) {
            return false;
        }
        // 统计最大最小值
        int min = Integer.MAX_VALUE;
        for(int num : nums) {
            min = Math.min(min, num);
        }
        // 桶的范围为t，由于t可能等于0，故取t+1，共分为(max - min) / (t + 1)个桶
        // 此处采用字典存储现有的桶
        Map<Long, Integer> bucket = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            long idx = ((long)nums[i] - min) / ((long)t + 1);
            // 同一个桶中存在值，在同一桶中差值必然小于t
            if(bucket.get(idx) != null) return true;
            // 查找前一个和后一个桶
            if(bucket.get(idx - 1) != null && (long)bucket.get(idx - 1) + t >= nums[i]) return true;
            if(bucket.get(idx + 1) != null && (long)nums[i] + t >= bucket.get(idx + 1)) return true;
            // 入桶
            bucket.put(idx, nums[i]);
            // 根据程序设计特性，此时字典有k+1个不符合要求的值，将最早的那个出队，为下一个值进队做准备
            if(bucket.size() > k) {
                idx = ((long)nums[i - k] - min) / ((long)t + 1);
                bucket.remove(idx);
            }
        }

        return false;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(k)$。

执行用时：12ms，在所有java提交中击败了90.60%的用户。

内存消耗：41.3MB，在所有java提交中击败了5.07%的用户。