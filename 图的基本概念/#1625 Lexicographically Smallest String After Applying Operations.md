[toc]

You are given a string `s` of **even length** consisting of digits from $0$ to $9$, and two integers `a` and `b`.

You can apply either of the following two operations any number of times and in any order on `s`:

* Add `a` to all odd indices of `s` (**0-indexed**). Digits post $9$ are cycled back to $0$. For example, if `s = "3456"` and `a = 5`, `s` becomes `"3951"`.
* Rotate `s` to the right by `b` positions. For example, if `s = "3456"` and `b = 1`, `s` becomes `"6345"`.

Return the **lexicographically** smallest string you can obtain by applying the above operations any number of times on `s`.

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. For example, `"0158"` is lexicographically smaller than `"0190"` because the first position they differ is at the third letter, and `'5'` comes before `'9'`.



**Constraints**:

* $2 \le \text{s.length} \le 100$
* `s.length` is even.
* `s` consists of digits from $0$ to $9$ only.
* $1 \le a \le 9$
* $1 \le b \le \text{s.length} - 1$



## 题目解读

&emsp;给定偶数长只包含数字的字符串及两种操作，求最后可得到的字典序最小的字符串。

```java
class Solution {
    public String findLexSmallestString(String s, int a, int b) {

    }
}
```

## 程序设计

* 由于数组长度为偶数，不管`b`是偶数还是奇数，多次操作总会得到原数组；而同样的`a`叠加多次仍可得到原字符，即尝试次数是有限的，可用广度优先搜索记录尝试所有可能，统计得到最小字典序；
* 以`74`，`a`为$5$，`b`为$1$为例，总共有`74`、`79`、`47`、`97`、`42`、`92`、`24`、`29`八种情况。

```java
class Solution {
    public String findLexSmallestString(String s, int a, int b) {
        char[] seq = s.toCharArray();
        // 记录出现的字符串
        Set<String> set = new HashSet<>();
        Queue<char[]> queue = new LinkedList<>();
        queue.add(seq);
        set.add(s);
        String min = s;

        while (!queue.isEmpty()) {
            char[] cur = queue.poll();
            // 尝试反转
            char[] rotate = rotate(cur, b);
            String tmp = new String(rotate);
            if (!set.contains(tmp)) {
                queue.add(rotate);
                set.add(tmp);
                min = min.compareTo(tmp) <= 0 ? min : tmp;
            }

            // 尝试叠加
            char[] add = add(cur, a);
            tmp = new String(add);
            if (!set.contains(tmp)) {
                queue.add(add);
                set.add(tmp);
                min = min.compareTo(tmp) <= 0 ? min : tmp;
            }
        }
        return min;
    }

    private char[] rotate(char[] arr, int offset) {
        char[] res = Arrays.copyOf(arr, arr.length);
        reverse(res, 0, res.length - 1);
        reverse(res, 0, offset - 1);
        reverse(res, offset, res.length - 1);
        return res;
    }

    private void reverse(char[] arr, int i, int j) {
        while (i < j) swap(arr, i++, j--);
    }

    private void swap(char[] arr, int i, int j) {
        char tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }

    private char[] add(char[] arr, int val) {
        char[] res = Arrays.copyOf(arr, arr.length);
        for (int i = 1; i < arr.length; i += 2) {
            res[i] = (char)((res[i] - '0' + val) % 10 + '0');
        }
        return res;
    }
}
```

## 性能分析

执行用时：102ms，在所有java提交中击败了59.08%的用户。

内存消耗：42.8MB，在所有java提交中击败了45.61%的用户。

## 官方解题

&emsp;暂无，密切关注。