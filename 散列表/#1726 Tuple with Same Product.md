[toc]

Given an array `nums` of **distinct** positive integers, return the number of tuples $(a, b, c, d)$ such that $a * b = c * d$ where $a$, $b$, $c$, and $d$ are elements of `nums`, and $a \ne b \ne c \ne d$.

 

**Example 1**:

```
Input: nums = [2,3,4,6]
Output: 8
Explanation: There are 8 valid tuples:
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (3,4,2,6) , (3,4,6,2) , (4,3,6,2)
```

**Example 2**:

```
Input: nums = [1,2,4,5,10]
Output: 16
Explanation: There are 16 valids tuples:
(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2)
(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1)
(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,4,5)
(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)
```

**Example 3**:

```
Input: nums = [2,3,4,6,8,12]
Output: 40
```

**Example 4**:

```
Input: nums = [2,3,5,7]
Output: 0
```



**Constraints**:

* $1 \le \text{nums.length} \le 1000$
* $1 \le \text{nums[i]} \le 10^4$
* All elements in `nums` are **distinct**.



## 题目解读

&emsp;给定只包含唯一值的数组，返回四元组的数目使得乘积相等。

```java
class Solution {
    public int tupleSameProduct(int[] nums) {

    }
}
```

## 程序设计

* 由于数组中值唯一，且数值大于$0$，故不存在$i$，$j$，$k$使得`A[i]*A[j] = A[i]*A[k]`，这样可遍历并用哈希表记录之前出现的乘积。

```java
class Solution {
    public int tupleSameProduct(int[] nums) {
        int res = 0;
        Map<Integer, Integer> counter = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                int prod = nums[i] * nums[j];
                if (counter.containsKey(prod)) res += 8 * counter.get(prod);
                counter.put(prod, counter.getOrDefault(prod, 0) + 1);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：254 ms, 在所有 Java 提交中击败了73.27%的用户。

内存消耗：62.8 MB, 在所有 Java 提交中击败了67.78%的用户。

## 官方解题

&emsp;暂无，密切关注。
