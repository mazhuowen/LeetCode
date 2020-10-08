[toc]

Given an array of integers `arr` of even length $n$ and an integer $k$.

We want to divide the array into exactly $n / 2$ pairs such that the sum of each pair is divisible by $k$.

Return True If you can find a way to do that or False otherwise.



**Constraints**:

* $\text{arr.length} == n$
* $1 \le n \le 10^5$
* $n$ is even.
* $-10^9 \le \text{arr[i]} \le 10^9$
* $1 \le k \le 10^5$



## 题目解读

&emsp;将给定数组划分为$n/2$个数据对，判断数据对之和是否可被$k$整除。

```java
class Solution {
    public boolean canArrange(int[] arr, int k) {

    }
}
```

## 程序设计

* 首先想到的是两个数字能被$k$整除则必然两个数字对$k$取余后的数字之和等于$0$或$k$，这样可对数字取余$k$后的数排序，然后遍历判断。

```java
class Solution {
    public boolean canArrange(int[] arr, int k) {
        if (k <= 0) throw new IllegalArgumentException("invalid param");
        Integer[] tmp = new Integer[arr.length];
        for (int i = 0; i < arr.length; i++) tmp[i] = arr[i];

        // 根据余数排序
        Arrays.sort(tmp, (a, b) -> remainder(a, k) - remainder(b, k));
        int left = 0, right = tmp.length - 1;
        int count = 0;
        // 余数为0的数字互相配对，必须为偶数
        while (left < tmp.length && remainder(tmp[left], k) == 0) left++;
        if (left % 2 == 1) return false;
        // 余数不为0的数字于互补的余数配对
        while (left <= right) {
            int r = remainder(tmp[left], k);
            while (remainder(tmp[left++], k) == r) count++;
            while (remainder(tmp[right--], k) == k - r) count--;
            if (count != 0) return false;
        }
        return true;
    }

    private int remainder(int a, int k) {
        return (k + a % k) % k;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：199ms，在所有java提交中击败了5.29%的用户。

内存消耗：54.2MB，在所有java提交中击败了30.06%的用户。

## 官方解题

&emsp;官方思路类似，仔细观察上述思路实际上只需要统计各个取余值得数目，无需排序。

```java
class Solution {
    public boolean canArrange(int[] arr, int k) {
        if (k <= 0) throw new IllegalArgumentException("invalid param");
        int[] counter = new int[k];
        for (int num : arr) counter[remainder(num, k)]++;
        
        for (int i = 1; i + i < k; i++) {
            if (counter[i] != counter[k - i]) return false;
        }
        // 题目限定数组长度为偶数，当k为偶数时不必检测余数为k/2是否是偶数，因为只要其他值是偶数，剩余的必然是偶数
        return counter[0] % 2 == 0;
    }

    private int remainder(int num, int k) {
        return (k + num % k) % k;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(K)$

执行用时：6ms，在所有java提交中击败了76.71%的用户。

内存消耗：47.3MB，在所有java提交中击败了65.03%的用户。