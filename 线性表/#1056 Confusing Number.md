[toc]

Given a number $N$, return `true` if and only if it is a confusing number, which satisfies the following condition:

We can rotate digits by 180 degrees to form new digits. When `0, 1, 6, 8, 9` are rotated 180 degrees, they become `0, 1, 9, 8, 6` respectively. When `2, 3, 4, 5` and `7` are rotated 180 degrees, they become invalid. A confusing number is a number that when rotated 180 degrees becomes a different number with each digit valid.

 

**Note**:

* $0 \le N \le 10^9$
* After the rotation we can ignore leading zeros, for example if after rotation we have `0008` then this number is considered as just `8`.



## 题目解读

&emsp;定义易混淆数为每个数字反转180度后可以得到有效且与原先数字不等的数字，给定数字判断是否是易混淆数。

```java
class Solution {
    public boolean confusingNumber(int N) {
        
    }
}
```

## 程序设计

* 可使用双指针遍历对比。

```java
class Solution {
    // -1表示反转后不是数字
    int[] state = new int[]{0, 1, -1, -1, -1, -1, 9, -1, 8, 6};

    public boolean confusingNumber(int N) {
        if (N < 0) return false;

        char[] nums = Integer.toString(N).toCharArray();
        int left = 0, right = nums.length - 1;

        // 是否数字不相等
        boolean flag = false;
        while (left < right) {
            int l = state[nums[left] - '0'], r = state[nums[right] - '0'];
            if (l == -1 || r == -1) return false;
            else if (l != nums[right] - '0' || r != nums[left] - '0') flag = true;
            left++;
            right--;
        }

        if (left == right) {
            int l = state[nums[left] - '0'];
            if (l == -1) return false;
            else if (!flag && l == nums[left] - '0') return false;
            else return true;
        }
        return flag;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.6MB，在所有java提交中击败了42.86%的用户。

## 官方解题

&emsp;暂无，密切关注。