[toc]

Given a string, we can "shift" each of its letter to its successive letter, for example: `"abc" -> "bcd"`. We can keep "shifting" which forms the sequence:

`"abc" -> "bcd" -> ... -> "xyz"`
Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.



## 题目解读

&emsp;给定一个字符串，对该字符串可以进行移位的操作，也就是将字符串中每个字母都变为其在字母表中后续的字母。给定仅包含小写字母字符串的列表，将列表中满足移位操作的组合进行分组。

```java
class Solution {
    public List<List<String>> groupStrings(String[] strings) {

    }
}
```

## 程序设计

* 对于移位字符串分类，如果每次判断都从原来的字符串出发，移动生成并做判断，最坏的情况下需要移动生成26次。仔细观察字符串结构，会发现字符串移动后其字符之间的相对距离不变，这样每一组字符串对应一个唯一相对距离数组。
* 特殊情况需要考虑相对偏移为负数的情况，只需加26即可（由于全是小写字母）。

```java
class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        List<List<String>> res = new LinkedList<>();
        if(strings == null || strings.length == 0) {
            return res;
        }
        // 记录分组，键为每个字符对应前一字符的偏移
        Map<String, List<String>> group = new HashMap<>();
        for(String str : strings) {
            // 获得当前字符串的偏移字符串
            String key = getOffset(str);
            if(group.get(key) == null) {
                group.put(key, new LinkedList<>());
            }
            group.get(key).add(str);
        }
        res = new LinkedList<>(group.values());
        return res;
    }

    private String getOffset(String str) {
        StringBuffer sb = new StringBuffer();
        // 第一个字符偏移标记为0
        sb.append("#0");
        for(int i = 1; i < str.length(); i++) {
            // 相对前一字符的偏移量
            int offset = str.charAt(i) - str.charAt(i - 1);
            // 类似az -> ba，偏移为26 + offset
            if(offset < 0) {
                offset += 26;
            }
            sb.append("#").append(offset);
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$，其中$N$为字符串数目，$M$为字符串的平均长度。

执行用时：3ms，在所有java提交中击败了80.10%的用户。

内存消耗：39.6MB, 在所有java提交中击败了5.10%的用户。

## 官方解题

&emsp;暂无，密切关注。