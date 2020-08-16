[toc]

<img src="..\images\#1521.png" style="zoom:80%;" />

Winston was given the above mysterious function `func`. He has an integer array `arr` and an integer `target` and he wants to find the values `l` and `r` that make the value `|func(arr, l, r) - target|` minimum possible.

Return the minimum possible value of `|func(arr, l, r) - target|`.

Notice that `func` should be called with the values `l` and `r` where $0 \le l, r < \text{arr.length}$.

 

**Constraints**:

* $1 \le \text{arr.length} \le 10^5$
* $1 \le \text{arr[i]} \le 10^6$
* $0 \le \text{target} \le 10^7$



## 题目解读

&emsp;求区间按位与值与目标值的最小差距。

```java
class Solution {
    public int closestToTarget(int[] arr, int target) {
        
    }
}
```

## 程序设计

* 参考社区思路，如果题目中是区间和，则可用前缀和的思路解决。当换为按位与时，考虑到数字不超过$20$位，类似与前缀和，可以记录$20$位数字变化情况，数组`bitRecord[i][j]`表示截止第$i$个数字，第$j$位按位与后变为$0$的索引，如果第$i$个数字第$j$位是$1$，则记录前面出现$0$的位置，否则记录当前位置。
* 在得到索引数组后可以从每个位置向前遍历二进制位置发生变化的索引，从而避免从数组遍历。

```java
class Solution {
    public int closestToTarget(int[] arr, int target) {
        if (arr == null || arr.length == 0) throw new IllegalArgumentException("invalid param");
        
        // 统计使得每一位变为0的索引，由于10^6不超过20位，只需关注20位
        int[][] bitRecord = new int[arr.length][20];
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < 20; j++) {
                bitRecord[i][j] = i > 0 ? bitRecord[i - 1][j] : -1;
                // 当前数字j位为0，记录当前索引
                if (((arr[i] >> j) & 1) == 0) bitRecord[i][j] = i;
            }
        }
        
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < arr.length; i++) {
            int val = arr[i];
            // 单独数字与目标值比较
            min = Math.min(min, Math.abs(val - target));
            if (min == 0) break;
            
            // 向前查找使得当前数字变化的索引
            int j = i;
            while (j >= 0) {
                int last = -1;
                for (int k = 0; k < 20; k++) {
                    // i位置数字k位为0，与任何数都是0，跳过k位
                    if (((val >> k) & 1) == 0) continue;
                    // i位置数字k位为1，能引起变化则j位数字为0
                    last = Math.max(last, bitRecord[j][k]);
                }
                // last为能引起i变化的最近的数
                if (last != -1) {
                    val &= arr[j];
                    min = Math.min(min, Math.abs(val - target));
                }
                // 在last的基础上基础向前探索
                j = last;
            }
        }
        return min;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：147ms，在所有java提交中击败了31.35%的用户。

内存消耗：53.3MB，在所有java提交中击败了55.81%的用户。

## 官方解题

&emsp;官方思路换一个角度，由于区间的按位与结果不超过$20$个，同时官方注意到区间按位与随着区间的增大而递减，故每次只需记录当前区间左侧从近到远的按位与值到链表中，此时链表必然是单点递减的，然后遍历下一个位置，并更新按位与值。

```java
class Solution {
    public int closestToTarget(int[] arr, int target) {
        // 初始化，链表表示当前区间左侧从近到远的区间按位与值
        int ans = Math.abs(arr[0] - target);
        List<Integer> valid = new ArrayList<Integer>();
        valid.add(arr[0]);
        
        for (int num : arr) {
            // 当前数字单独与目标值比较
            ans = Math.min(ans, Math.abs(num - target));
            List<Integer> validNew = new ArrayList<Integer>();
            validNew.add(num);
            
            // 从近到远遍历前面的区间按位与值
            int last = num;
            for (int prev : valid) {
                // 此处由于按位与的值具有单调递减性，故只需根据上一次的值排重并加入新链表
                int curr = prev & num;
                if (curr != last) {
                    validNew.add(curr);
                    ans = Math.min(ans, Math.abs(curr - target));
                    last = curr;
                }
            }
            // 更新迭代
            valid = validNew;
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：49ms，在所有java提交中击败了70.47%的用户。

内存消耗：48.7MB，在所有java提交中击败了83.72%的用户。