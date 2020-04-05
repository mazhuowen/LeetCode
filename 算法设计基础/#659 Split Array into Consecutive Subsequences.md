[toc]

Given an array `nums` sorted in ascending order, return `true` if and only if you can split it into 1 or more subsequences such that each subsequence consists of consecutive integers and has length at least 3.



Constraints:

- $1 \le \text{nums.length} \le 10000$



## 题目解读

&emsp;输入一个按升序排序的整数数组，可能包含重复数字，将它们分割成一个或几个子序列，其中每个子序列至少包含三个连续整数。

```java
class Solution {
    public boolean isPossible(int[] nums) {
        
    }
}
```

## 程序设计

* 参考官方解题，假设有`[10,10,11,11,11,11,12,12,12,12,13]`，统计每个数字的数目，10有2个，11有4个，12有4个，13有1个；首先序列必须从10开始，有两个；然后11比10多2个，意味着以11开始的序列有2个，当前总共4个序列；13比12少3个，这意味这有三条序列在12处结束；最后在13处结束一条。
* 结合本题，需要记录前一个数字及其计数，与当前数字计数比较；存在两种情况：一种是当前数字与之前数字不连续，则前一数字必然是序列的结尾，则判断之前的序列是否多于3个，然后以当前数字为起点继续遍历；第二种是当前数字与之前数字连续，则判断计数，如果当前计数比之前计数大，则表示当前数字是某些序列的起点，如果小，则表示之前数字是某些序列的终点。
* 为了保证序列较大，采用队列保存起始数字，这样先进先出，序列是目前最大长度的。

```java
class Solution {
    public boolean isPossible(int[] nums) {
        if (nums == null || nums.length < 3) return false;
        // 记录起始数字
        Queue<Integer> queue = new LinkedList<>();

        // 记录前一个数字
        Integer preNum = null;
        // 记录前一个数字计数及当前数字开始索引
        int preCount = 0, idx = 0;

        for (int i = 0; i < nums.length; i++) {
            // 遍历到相同数字的最后一位或者数组的最后一位
            if (i != nums.length - 1 && nums[i] == nums[i + 1]) continue;
            // 当前数字的数目
            int count = i - idx + 1;

            // 前一个数字与当前数字不连续
            if (preNum != null && nums[i] - preNum > 1) {
                // 检查当前元素与开始元素构成的序列是否大于2
                while (preCount-- > 0) {
                    if (preNum - queue.poll() < 2) return false;
                }
                // 重新将前一元素置为空，表示从当前数字重新开始序列
                preNum = null;
            }
            // 前一数字为空或数字连续
            if (preNum == null || nums[i] - preNum == 1) {
                // 前面数字多于当前数字，表示前一数字是结束点
                while (preCount > count) {
                    // 出队判断
                    if (preNum - queue.poll() < 2) return false;
                    preCount--;
                }
                // 前面数字少于当前数字，表示当前数字是起始点
                while (preCount++ < count) {
                    queue.add(nums[i]);
                }
            }
            preNum = nums[i];
            preCount = count;
            // 下一数字的起始点
            idx = i + 1;
        }
        // 对数组的最后一位数字进行判断，数组最后一位必然是结尾
        while (preCount-- > 0) {
            // 出队判断
            if (nums[nums.length - 1] - queue.poll() < 2) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了99.54%的用户。

内存消耗：40.9MB，在所有java提交中击败了72.73%的用户。

## 官方解题

&emsp;官方还提出了贪婪法思路，首先对数字计数，然后遍历判断每个数字是否存在序列中，记录更新序列最后的数字。以`1,2,3,3,4,5`为例，遍历到1，在计数字典中检查后两位2、3是否存在，存在则更新序列字典，加入后一个需要的数字4，同时计数减一；遍历到2，发现计数为0，继续遍历；遍历到3，计数不为0，且没有需要3的序列，则以3为起点，加入4、5。最后得到答案。该思路的贪婪思想为如果存在序列，则大的序列要比小的序列更好，采用这个贪婪思路，遍历数字优先加入到需要该数字的序列，没有才会以该数字为起点，创建新序列。

```java
class Solution {
    public boolean isPossible(int[] nums) {
        if (nums == null || nums.length < 3) return false;

        // 对数字计数
        Map<Integer, Integer> counter = new HashMap<>();
        for (int num : nums) counter.put(num, counter.getOrDefault(num, 0) + 1);
        // 记录连续序列所需的后一个元素
        Map<Integer, Integer> record = new HashMap<>();

        for (int num : nums) {
            // 已经加入序列，继续遍历
            if (counter.get(num) <= 0) continue;
            // 加入所需的序列，并更新序列所需数字为后一个数字
            if (record.getOrDefault(num, 0) > 0) {
                // 更新下一个所需数字
                record.put(num, record.get(num) - 1);
                record.put(num + 1, record.getOrDefault(num + 1, 0) + 1);
            }
            // 没有所需当前数字的序列，则以当前数字为起点判断加入序列
            else if (counter.getOrDefault(num + 1, 0) > 0 && counter.getOrDefault(num + 2, 0) > 0) {
                // 减少计数
                counter.put(num + 1, counter.get(num + 1) - 1);
                counter.put(num + 2, counter.get(num + 2) - 1);
                // 加入序列所需的下一个数字
                record.put(num + 3, record.getOrDefault(num + 3, 0) + 1);
            }
            // 当前数字后续数字不足3个，返回
            else {
                return false;
            }

            counter.put(num, counter.get(num) - 1);
        }
        return true;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：35ms，在所有java提交中击败了61.64%的用户。

内存消耗：41.1MB，在所有java提交中击败了63.64%的用户。