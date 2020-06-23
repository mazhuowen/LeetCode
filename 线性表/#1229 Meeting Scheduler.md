[toc]

Given the availability time slots arrays `slots1` and `slots2` of two people and a meeting duration `duration`, return the **earliest time slot** that works for both of them and is of duration `duration`.

If there is no common time slot that satisfies the requirements, return an **empty array**.

The format of a time slot is an array of two elements `[start, end]` representing an inclusive time range from `start` to `end`.  

It is guaranteed that no two availability slots of the same person intersect with each other. That is, for any two time slots `[start1, end1]` and `[start2, end2]` of the same person, either `start1 > end2` or `start2 > end1`.



**Constraints**:

* $1 \le \text{slots1.length, slots2.length} \le 10^4$
* $\text{slots1[i].length, slots2[i].length} == 2$
* $\text{slots1[i][0]} < \text{slots1[i][1]}$
* $\text{slots2[i][0]} < \text{slots2[i][1]}$
* $0 \le \text{slots1[i][j], slots2[i][j]} \le 10^9$
* $1 \le \text{duration} \le 10^6 $



## 题目解读

&emsp;给定两个人的空闲时间表和会议持续时间，给出最早开始的会议时间，不存在则返回空数组。

```java
class Solution {
public:
    vector<int> minAvailableDuration(vector<vector<int>>& slots1, vector<vector<int>>& slots2, int duration) {

    }
};
```

## 程序设计

* 首先对时间区间数组排序，然后类似归并排序的合并阶段，依次遍历比较。

```java
class Solution {
    public List<Integer> minAvailableDuration(int[][] slots1, int[][] slots2, int duration) {
        List<Integer> res = new ArrayList<>();
        if (slots1 == null || slots1.length == 0 
            || slots2 == null || slots2.length == 0 || duration < 1) return res;

        // 排序
        Arrays.sort(slots1, (a, b) -> a[0] - b[0]);
        Arrays.sort(slots2, (a, b) -> a[0] - b[0]);

        // 遍历迭代
        int idx1 = 0, idx2 = 0;
        while (idx1 < slots1.length && idx2 < slots2.length) {
            int s1 = slots1[idx1][0], e1 = slots1[idx1][1];
            int s2 = slots2[idx2][0], e2 = slots2[idx2][1];

            // 当前区间不相交，迭代
            if (e1 <= s2) idx1++;
            else if (e2 <= s1) idx2++;
            // 相交，判断相交区间长度是否符合要求
            else {
                int intervalS = Math.max(s1, s2);
                int intervalE = Math.min(e1, e2);
                // 找到合适的会议区间
                if (intervalE - intervalS >= duration) {
                    res.add(intervalS);
                    res.add(intervalS + duration);
                    break;
                }
                // 区间长度不满足要求，继续迭代
                if (e1 < e2) idx1++;
                else idx2++;
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N\log_2N,M\log_2M,N + M))$，空间复杂度为$O(1)$。

执行用时：37ms，在所有java提交中击败了69.79%的用户。

内存消耗：49.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。