[toc]

Given a string `S`, find out the length of the longest repeating substring(s). Return $0$ if no repeating substring exists.



**Constraints**:

* The string `S` consists of only lowercase English letters from `'a' - 'z'`.

* $1 \le \text{S.length} \le 1500$



## 题目解读

&emsp;求字符串中的最长重复子串长度。

```java
class Solution {
    public int search(int L, int a, long modulus, int n, int[] nums) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    public int longestRepeatingSubstring(String S) {
        int n = S.length();

        int[] nums = new int[n];
        for (int i = 0; i < n; ++i) nums[i] = (int)S.charAt(i) - (int)'a';
        
        // 26进制
        int a = 26;
        // 字符串哈希值取模
        long modulus = (long)Math.pow(2, 24);

        int left = 1, right = n;
        int L;
        while (left <= right) {
            L = left + (right - left) / 2;
            if (search(L, a, modulus, n, nums) != -1) left = L + 1;
            else right = L - 1;
        }

        return left - 1;
    }
    
    private int search(int L, int a, long modulus, int n, int[] nums) {
        // 计算第一个窗口的哈希值
        long h = 0;
        for (int i = 0; i < L; ++i) h = (h * a + nums[i]) % modulus;

        HashSet<Long> seen = new HashSet();
        seen.add(h);
        
        long aL = 1;
        for (int i = 1; i <= L; ++i) aL = (aL * a) % modulus;
		// 滑动窗口计算哈希值
        for(int start = 1; start < n - L + 1; ++start) {
            h = (h * a - nums[start - 1] * aL % modulus + modulus) % modulus;
            h = (h + nums[start + L - 1]) % modulus;
            if (seen.contains(h)) return start;
            seen.add(h);
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了99.22%的用户。

内存消耗：39.7MB，在所有java提交中击败了55.56%的用户。

## 官方解题

&emsp;见上。