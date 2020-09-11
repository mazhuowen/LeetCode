[toc]

Given an array of integers `nums` and a positive integer $k$, find whether it's possible to divide this array into sets of $k$ consecutive numbers
Return `True` if its possible otherwise return `False`.



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* $1 \le \text{nums[i]} \le 10^9$
* $1 \le k \le \text{nums.length}$



## 题目解读

&emsp;同[#846 Hand of Straights](./#846 Hand of Straights.md)。

```java
class Solution {
    public boolean isPossibleDivide(int[] nums, int k) {

    }
}
```

## 程序设计

* 同[#846 Hand of Straights](./#846 Hand of Straights.md)。

```java
class Solution {
    public boolean isPossibleDivide(int[] nums, int k) {
        Arrays.sort(nums);
        Map<Integer, Integer> record = new HashMap<>();
        for (int num : nums) {
            record.put(num, record.getOrDefault(num, 0) + 1);
        }
        for (int num : nums) {
            int card = record.get(num);
            if (card == 0) continue;
            record.put(num, 0);
            
            for (int i = 1; i < k; i++) {
                if (record.getOrDefault(num + i, 0) < card) return false;
                record.put(num + i, record.get(num + i) - card);
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\frac{N}{K})$，空间复杂度为$O(N)$。

执行用时：49ms，在所有java提交中击败了82.47%的用户。

内存消耗：56.9MB，在所有java提交中击败了5.43%的用户。

## 官方解题

&emsp;同上。