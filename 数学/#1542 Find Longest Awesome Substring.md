[toc]

Given a string `s`. An awesome substring is a non-empty substring of `s` such that we can make any number of swaps in order to make it palindrome.

Return the length of the maximum length **awesome substring** of `s`.



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s` consists only of digits.



## 题目解读

&emsp;给定字符串，求最长的超赞子字符串长度。超赞子字符串定义为非空字符串进行任意次交换后可得到一个回文字符串。

```java
class Solution {
    public int longestAwesome(String s) {

    }
}
```

## 程序设计

* 最基本的方法是从长度$1 \sim n$遍历所有子字符串，并统计字符串中字符的计数，由于回文字符串最多只有一个字符为奇数，根据字符计数即可判断是否符合要求。该方法时间复杂度为$O(N^2)$，会超时。
* 由于回文只跟字符的奇偶相关，具体的统计数目无关，可使用十位二进制表示来代表$0 \sim 9$各个数字是奇数还是偶数，$1$表示奇数，$0$表示偶数；在统计时，可使用亦或操作。
* 假设当前状态为$0010001101$，而历史记录存在该状态，由于奇数减去奇数是偶数，偶数减去偶数仍然是偶数，可知必然存在子字符串各个字符统计为偶数，可形成回文串；对于存在一个奇数的情况，需要遍历二进制位，进行亦或操作，然后判断是否存在历史记录。
* 该思路可看作是前缀和的巧妙变形应用，只需遍历统计一次，在这个过程中当前状态与之前状态一致，说明中间这段子字符串是回文字符串。

```java
class Solution {
    public int longestAwesome(String s) {
        if (s == null || s.isEmpty()) return 0;

        // 子字符串中数字奇偶数二进制表示，1表示奇数，0表示偶数
        int stat = 0;
        // 子字符串状态与索引记录
        Map<Integer, Integer> record = new HashMap<>();
        record.put(stat, -1);

        int max = 0, idx = 0;
        for (char c : s.toCharArray()) {
            // 更新统计状态
            stat ^= 1 << (c - '0');
            
            // 有一个奇数的情况
            for (int i = 0; i < 10; i++) {
                int tmp = stat ^ (1 << i);
                if (record.containsKey(tmp)) max = Math.max(max, idx - record.get(tmp));
            }

            // 全部为偶数个的情况
            if (record.containsKey(stat)) max = Math.max(max, idx - record.get(stat));
            // 加入当前状态
            else record.put(stat, idx);

            idx++;
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：141ms，在所有java提交中击败了60.53%的用户。

内存消耗：42.1MB，在所有java提交中击败了15.79%的用户。

## 官方解题

&emsp;暂无，密切关注。