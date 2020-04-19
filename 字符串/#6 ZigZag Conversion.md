[toc]

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P    A    H   N
A P L S I I G
Y     I     R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```java
string convert(string s, int numRows);
```



## 题目解读

&emsp;给定`Z`字型字符串，给出原来的字符串。

```java
class Solution {
    public String convert(String s, int numRows) {

    }
}
```

## 程序设计

* 注意到`numRows`行的`Z`字型字符串，以行数为3为例，第一行字符编号为0，第二行字符编号1，第三行2，第二行3，第一行4，以此类推可以发现规律，即在$i \sim i + 2 * rows - 2$闭开区间范围内的字符，第$k$行对应的两个字符所以为$i + k$和$i + 2 * rows - 2 - k$（第一行和最后一行只有一个字符，需判断排除）。

```java
class Solution {
    public String convert(String s, int numRows) {
        // 特殊情况处理
        if(s == null || s.length() <= numRows || numRows == 1) return s;

        // 记录每一行的字符串
        Map<Integer, StringBuffer> record = new HashMap<>();
        for (int i = 0; i < numRows; i++) {
            record.put(i, new StringBuffer());
        }

        char[] words = s.toCharArray();
        int offset = 2 * numRows - 2;
        for (int i = 0; i < words.length; i += offset) {
            int j = i + offset;
            // 第一行和最后一行只拼接一个字符
            record.get(0).append(words[i]);
            // 需判断索引是否符合要求
            if (i + numRows - 1 < words.length) record.get(numRows - 1).append(words[i + numRows - 1]);
            // 拼接中间行，需要拼接两个字符
            for (int k = 1; k < numRows - 1; k++) {
                if (i + k < words.length) record.get(k).append(words[i + k]);
                if (j - k < words.length) record.get(k).append(words[j - k]);
            }
        }

        // 将每行字符串拼接
        StringBuffer res = new StringBuffer();
        for (int i = 0; i < numRows; i++) {
            res.append(record.get(i));
        }
        return res.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了43.16%的用户。

内存消耗：39.9MB，在所有java提交中击败了10.00%的用户。

## 官方解题

&emsp;官方思路类似，代码结构比较简单，条件判断较少，时间性能也更好。

```java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1) return s;

        StringBuilder ret = new StringBuilder();
        int n = s.length();
        int cycleLen = 2 * numRows - 2;

        // 按每行依次访问
        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < n; j += cycleLen) {
                // 拼接第一个字符
                ret.append(s.charAt(j + i));
                // 如果不是第一行和最后一行，拼接第二个字符
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                    ret.append(s.charAt(j + cycleLen - i));
            }
        }
        return ret.toString();
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了99.52%的用户。

内存消耗：39.5MB，在所有java提交中击败了13.33%的用户。