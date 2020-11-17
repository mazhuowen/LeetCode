[toc]

To some string `S`, we will perform some replacement operations that replace groups of letters with new ones (not necessarily the same size).

Each replacement operation has `3` parameters: a starting index `i`, a source word `x` and a target word `y`.  The rule is that if `x` starts at position `i` in the **original string** `S`, then we will replace that occurrence of `x` with `y`.  If not, we do nothing.

For example, if we have `S = "abcd"` and we have some replacement operation `i = 2`, `x = "cd"`, `y = "ffff"`, then because `"cd"` starts at position `2` in the original string `S`, we will replace it with `"ffff"`.

Using another example on `S = "abcd"`, if we have both the replacement operation `i = 0`, `x = "ab"`, `y = "eee"`, as well as another replacement operation `i = 2`, `x = "ec"`, `y = "ffff"`, this second operation does nothing because in the original string `S[2] = 'c'`, which doesn't match `x[0] = 'e'`.

All these operations occur simultaneously.  It's guaranteed that there won't be any overlap in replacement: for example, `S = "abc"`, `indexes = [0, 1]`, `sources = ["ab","bc"]` is not a valid test case.



**Notes**:

* $0 \le \text{indexes.length} = \text{sources.length} = \text{targets.length} \le 100$
* $0 < \text{indexes[i]} < \text{S.length} \le 1000$
* All characters in given inputs are lowercase letters.



## 题目解读

&emsp;根据标记的起始位置，判断是否可替换为目标值。

```java
class Solution {
    public String findReplaceString(String S, int[] indexes, String[] sources, String[] targets) {

    }
}
```

## 程序设计

* 参考官方思路，先标记替换位置，再替换拼接，避免边标记判断边替换造成的字符串长度变化，增加难度。

```java
class Solution {
    public String findReplaceString(String S, int[] indexes, String[] sources, String[] targets) {
        if (S == null || S.isEmpty()) return S;

        char[] str = S.toCharArray();
        // 保存匹配的字符串索引和indexes索引
        int[] matcher = new int[str.length];
        Arrays.fill(matcher, -1);
        for (int i = 0; i < indexes.length; i++) {
            int start = indexes[i];
            // 记录替换位置
            if (isSame(str, start, sources[i].toCharArray())) matcher[start] = i;
        }

        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < str.length; i++) {
            // 未匹配到，直接拼接
            if (matcher[i] == -1) sb.append(str[i]);
            else {
                // 替换
                sb.append(targets[matcher[i]]);
                // i迭代
                i += sources[matcher[i]].length() - 1;
            }
        }
        return sb.toString();
    }

    private boolean isSame(char[] source, int start, char[] target) {
        if (start + target.length - 1 >= source.length) return false;

        for (char c : target) {
            if (c != source[start++]) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NC)$，空间复杂度为$O(N)$，$C$为替换操作数。

执行用时：1ms，在所有java提交中击败了99.11%的用户。

内存消耗：40MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;上述思路参考官方。