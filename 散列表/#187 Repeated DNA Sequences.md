[toc]

All DNA is composed of a series of nucleotides abbreviated as `A`, `C`, `G`, and `T`, for example: `ACGAATTCCG`. When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.



## 题目解读

&emsp;查找到DNA序列中所有等于10的重复序列。

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {

    }
}
```

## 程序设计

* 使用哈希表保存子序列，并遍历计数，等于2说明有重复，加入链表。

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> res = new LinkedList<>();
        // 序列小于等于10则不存在重复序列
        if(s == null || s.length() < 11) {
            return res;
        }
        Map<String, Integer> record = new HashMap<>();
        for(int i = 0; i <= s.length() - 10; i++) {
            String key = s.substring(i, i + 10);
            // 计数加一
            int count = record.getOrDefault(key, 0);
            record.put(key,  ++count);
            // 加入链表（大于2的说明之前加过，不再加入）
            if(count == 2) {
                res.add(key);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：25ms，在所有java提交中击败了64.30%的用户。

内存消耗：51.4MB，在所有java提交中击败了5.44%的用户。

## 官方解题

&emsp;暂无，密切关注。社区对上述思路进行了优化，前后两次字符串的截取操作实质上只有两个字符不一致，其它字符一致，每次重新截取是种浪费。可以将字符表示为二进制形式，这样字符串的截取可以看做是二进制位操作。

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> res = new LinkedList<>();
        if(s == null || s.length() < 11) {
            return res;
        }
        Map<Integer, Integer> record = new HashMap<>();
        // A：0，C：1，G：2，T：3
        int[] trans = new int[26];
        trans['A' - 'A'] = 0;
		trans['C' - 'A'] = 1;
		trans['G' - 'A'] = 2;
		trans['T' - 'A'] = 3;

        int key = 0;
        // 初始化键
        for(int i = 0; i < 10; i++) {
            // 左移两位，嵌入新字符
            key <<= 2;
            key |= trans[s.charAt(i) - 'A'];
        }
         record.put(key,  1);
        for(int i = 10; i < s.length(); i++) {
            key <<= 2;
            key |= trans[s.charAt(i) - 'A'];
            // 低位20为与1与，保留低位20为，左移的高两位会变为0
            key &= 0xfffff;
            // 计数加一
            int count = record.getOrDefault(key, 0);
            record.put(key,  ++count);
            // 加入链表（大于2的说明之前加过，不再加入）
            if(count == 2) {
                res.add(s.substring(i - 9, i + 1));
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：27ms，在所有java提交中击败了53.86%的用户。

内存消耗：46.1MB，在所有java提交中击败了85.48%的用户。