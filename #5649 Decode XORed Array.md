[toc]

There is a **hidden** integer array `arr` that consists of $n$ non-negative integers.

It was encoded into another integer array `encoded` of length $n - 1$, such that `encoded[i] = arr[i] XOR arr[i + 1]`. For example, if `arr = [1,0,2,1]`, then encoded = `[1,2,3]`.

You are given the `encoded` array. You are also given an integer `first`, that is the first element of `arr`, i.e. `arr[0]`.

Return the original array `arr`. It can be proved that the answer exists and is unique.

 

**Example 1**:

```
Input: encoded = [1,2,3], first = 1
Output: [1,0,2,1]
Explanation: If arr = [1,0,2,1], then first = 1 and encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]
```

**Example 2**:

```
Input: encoded = [6,2,7,3], first = 4
Output: [4,2,0,7,4]
```



**Constraints**:

* $2 \le n \le 10^4$
* $\text{encoded.length} == n - 1$
* $0 \le \text{encoded[i]} \le 10^5$
* $0 \le \text{first} \le 10^5$



## 题目解读

&emsp;根据编码值解码。

```java
class Solution {
    public int[] decode(int[] encoded, int first) {

    }
}
```

## 程序设计

* 亦或即可。

```java
class Solution {
    public int[] decode(int[] encoded, int first) {
        int n = encoded.length;
        int[] res = new int[n + 1];
        res[0] = first;
        for (int i = 0; i < n; i++) {
            res[i + 1] = res[i] ^ encoded[i];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
