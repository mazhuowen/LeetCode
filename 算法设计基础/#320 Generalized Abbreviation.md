[toc]

Write a function to generate the generalized abbreviations of a word. 

Note: The order of the output does not matter.



## 题目解读

&emsp;写出单词的所有缩写。缩写中使用数字表示单词位数。

```java
class Solution {
    public List<String> generateAbbreviations(String word) {

    }
}
```

## 程序设计

* 最基本的思路是回溯所有可能的缩写。存在两个问题，一个是存在重复，需要去重，里有个问题是，缩写中不能有相邻的数字出现。第一个问题用集合来解决；第二个问题使用标识位`flag`来表示上一个是数字缩写，下次不考虑数字缩写。

```java
class Solution {
    List<String> res;
    Set<String> set;

    public List<String> generateAbbreviations(String word) {
        set = new HashSet<>();
        if (word == null) return new LinkedList<>();

        String[] temp = new String[word.length()];
        generateAbbreviations(word.toCharArray(), 0, temp, 0, false);
        res = new LinkedList<>(set);
        return res;
    }

    private void generateAbbreviations(char[] chars, int start, String[] temp, int idx, boolean flag) {
        // 完成一组
        if (start >= chars.length) {
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < idx; i++) {
                sb.append(temp[i]);
            }
            // 去重
            set.add(sb.toString());
            return;
        }

        if (!flag) {
            for (int i = start; i < chars.length; i++) {
                // 试探数字缩写
                temp[idx++] = (i - start + 1) + "";

                // 回溯
                generateAbbreviations(chars, i + 1, temp, idx--, true);
            }
        }

        for (int i = start; i < chars.length; i++) {
            // 保持字母不变
            temp[idx++] = chars[i] + "";
            // 回溯
            generateAbbreviations(chars, i + 1, temp, idx, false);
        }
    }
}
```

* 上述代码很冗余，在本轮需要遍历后续序列，而递归也会遍历后续序列，造成重复需要额外的去重等。可优化为如下结构，每轮迭代只决定当前字符是保持原状还是替换为数字；如果替换为数字，则需判断之前是否也是数字，是的话需要叠加。

```java
class Solution {
    List<String> res;

    public List<String> generateAbbreviations(String word) {
        res = new LinkedList<>();
        if (word == null) return res;

        char[] temp = new char[word.length()];
        generateAbbreviations(word.toCharArray(), 0, temp, 0, 0);
        return res;
    }

    // num标识前一个是否为数字缩写，如果不为０，则表示之前的字符或序列用数字num代替
    private void generateAbbreviations(char[] chars, int start, char[] temp, int idx, int num) {
        // 完成一组
        if (start >= chars.length) {
            String s = new String(temp, 0, idx);
            if (num != 0) s += num + "";
            res.add(s);
            return;
        }

        // 尝试保持字母原样，注意如果数字不是0，则需要先将数字拼接
        int newIdx = num == 0 ? idx : insertNum(temp, idx, num);
        temp[newIdx++] = chars[start];
        generateAbbreviations(chars, start + 1, temp, newIdx, 0);

        // 尝试数字
        num += 1;
        generateAbbreviations(chars, start + 1, temp, idx, num);
    }

    private int insertNum(char[] temp, int idx, int num) {
        char[] nums = Integer.toString(num).toCharArray();
        for (char n : nums) {
            temp[idx++] = n;
        }
        return idx;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(Ｎ2^N)$，其中$N$为构造字符串花费的时间；空间复杂度为$O(2^N)$。

执行用时：13ms，在所有java提交中击败了71.82%的用户。

内存消耗：45.8MB，在所有java提交中击败了100.00%的用户．

## 官方解题

&emsp;官方回溯思路与上述形式一致，拼接采用`StringBuffer`，速度更快，同时调整尝试次序，先尝试数字再尝试字母，结构更简洁。

```java
class Solution {
    List<String> res;

    public List<String> generateAbbreviations(String word) {
        res = new LinkedList<>();
        if (word == null) return res;

        generateAbbreviations(word.toCharArray(), 0, new StringBuffer(), 0);
        return res;
    }

    // num标识前一个是否为数字缩写，如果不为０，则表示之前的字符或序列用数字num代替
    private void generateAbbreviations(char[] chars, int start, StringBuffer sb, int num) {
        int len = sb.length();
        // 完成一组
        if (start >= chars.length) {
            if (num != 0) sb.append(num);
            res.add(sb.toString());
        } else {
            // 尝试数字
            generateAbbreviations(chars, start + 1, sb, num + 1);

            // 尝试保持字母原样，注意如果数字不是0，则需要先将数字拼接
            if (num != 0) sb.append(num);
            sb.append(chars[start]);
            generateAbbreviations(chars, start + 1, sb, 0);
        }
        // 回溯
        sb.setLength(len);
    }
}
```

&emsp;时间复杂度，空间复杂度不变。

执行用时：7ms，在所有java提交中击败了88.18%的用户。

内存消耗：45.8MB，在所有java提交中击败了100.00%的用户。