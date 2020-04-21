[toc]

Given an array of characters, compress it in-place.

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a **character** (not int) of length 1.

After you are done **modifying the input array in-place**, return the new length of the array.



Follow up:
Could you solve it using only $O(1)$ extra space?



Note:

* All characters have an ASCII value in `[35, 126]`.
* $1 \le \text{len(chars)} \le 1000$.



## 题目解读

&emsp;压缩字符串，然后返回压缩后的长度。

```java
class Solution {
    public int compress(char[] chars) {

    }
}
```

## 程序设计

* 压缩连续的字符，如果数目大于2，则需要合并，即字符拼接字符出现的数字。

```java
class Solution {
    public int compress(char[] chars) {
        if (chars == null || chars.length == 0) return 0;

        // 压缩字符索引
        int idx = 0;
        // 前一字符起始索引
        int pre = 0;
        for (int i = 0; i < chars.length; i++) {
            // 字符连续，继续
            if (chars[i] == chars[pre]) continue;

            // 拼接字符
            chars[idx++] = chars[pre];

            if (i - pre > 1) {
                // 拼接数字
                idx = addNumToArray(chars, idx, i - pre);
            } 
            // 迭代
            pre = i;
        }
        // 拼接字符（最后的字符或连续字符）
        chars[idx++] = chars[pre];
        if (chars.length - pre > 1) {
            idx = addNumToArray(chars, idx, chars.length - pre);
        }

        return idx;
    }
    
    private int addNumToArray(char[] chars, int idx, int num) {
        if (num < 10) {
            chars[idx++] = (char)(num + '0');
        } 
        // 转化为字符串拼接
        else {
            String s = Integer.toString(num);
            for (char c : s.toCharArray()) {
                chars[idx++] = c;
            }
        }
        return idx;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;官方代码结构更简洁。