[toc]

Given a sequence of words, check whether it forms a valid word square.

A sequence of words forms a valid word square if the `kth` row and column read the exact same string, where $0 \le k < \max(\text{numRows}, \text{numColumns})$.



**Note**:

* The number of words given is at least $1$ and does not exceed $500$.
* Word length will be at least $1$ and does not exceed $500$.
* Each word contains only lowercase English alphabet `a-z`.

**Example 1**:

```
Input:
[
  "abcd",
  "bnrt",
  "crmy",
  "dtye"
]

Output:
true

Explanation:
The first row and first column both read "abcd".
The second row and second column both read "bnrt".
The third row and third column both read "crmy".
The fourth row and fourth column both read "dtye".

Therefore, it is a valid word square.
```

**Example 2**:

```
Input:
[
  "abcd",
  "bnrt",
  "crm",
  "dt"
]

Output:
true

Explanation:
The first row and first column both read "abcd".
The second row and second column both read "bnrt".
The third row and third column both read "crm".
The fourth row and fourth column both read "dt".

Therefore, it is a valid word square.
```

**Example 3**:

```
Input:
[
  "ball",
  "area",
  "read",
  "lady"
]

Output:
false

Explanation:
The third row reads "read" while the third column reads "lead".

Therefore, it is NOT a valid word square.
```



## 题目解读

&emsp;判断单词是否可构成单词方块。注意单词可以不等长。

```java
class Solution {
    public boolean validWordSquare(List<String> words) {

    }
}
```

## 程序设计

* 将单词维护在二维数组中，然后判断$(i,j)$与$(j,i)$是否相等，注意单词长度不一致，对于较短的单词其后为空，对应的对称位置也要为空。

```java
class Solution {
    public boolean validWordSquare(List<String> words) {
        if (words == null || words.isEmpty()) return true;
        int m = words.size();

        char[][] record = new char[m][m];
        for (int i = 0; i < m; i++) {
            char[] word = words.get(i).toCharArray();
            // 长度不符合要求
            if (word.length > m) return false;
            // 填充并对比
            for (int j = 0; j < word.length; j++) {
                // 不对称
                if (j < i && record[i][j] != word[j]) return false;
                record[i][j] = record[j][i] = word[j];
            }
            // 对比超出当前单词长度的对称位置是否是空
            for (int j = word.length; j < m; j++) {
                if (record[j][i] != 0) return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：3 ms, 在所有 Java 提交中击败了92.00%的用户。

内存消耗：38.1 MB, 在所有 Java 提交中击败了65.31%的用户。

## 官方解题

&emsp;暂无，密切关注。
