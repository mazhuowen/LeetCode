[toc]

Given an array `A` of strings made only from lowercase letters, return a list of all characters that show up in all strings within the list (**including duplicates**).  For example, if a character occurs 3 times in all strings but not 4 times, you need to include that character three times in the final answer.

You may return the answer in any order.

 

**Note**:

* $1 \le \text{A.length} \le 100$
* $1 \le \text{A[i].length} \le 100$
* `A[i][j]` is a lowercase letter



## 题目解读

&emsp;统计所有单词共同出现的字符。

```java
class Solution {
    public List<String> commonChars(String[] A) {

    }
}
```

## 程序设计

* 简单统计并取最小值。

```java
class Solution {
    public List<String> commonChars(String[] A) {
        int[] counter = new int[26];
        Arrays.fill(counter, Integer.MAX_VALUE);
        for (String word : A) {
            int[] cur = new int[26];
            for (char c : word.toCharArray()) cur[c - 'a']++;
            for (int i = 0; i < 26; i++) counter[i] = Math.min(counter[i], cur[i]);
        }

        List<String> res = new LinkedList<>();
        for (char c = 'a'; c <= 'z'; c++) {
            for (int i = 0; i < counter[c - 'a']; i++) {
                res.add(String.valueOf(c));
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了97.06%的用户。

内存消耗：38.5MB，在所有java提交中击败了70.05%的用户。

## 官方解题

&emsp;官方思路类似。