[toc]

A string `S` represents a list of words.

Each letter in the word has 1 or more options.  If there is one option, the letter is represented as is.  If there is more than one option, then curly braces delimit the options.  For example, `"{a,b,c}"` represents options `["a", "b", "c"]`.

For example, `"{a,b,c}d{e,f}"` represents the list `["ade", "adf", "bde", "bdf", "cde", "cdf"]`.

Return all words that can be formed in this manner, in lexicographical order.



Note:

* $1 \le \text{S.length} \le 50$
* There are no nested curly brackets.
* All characters inside a pair of consecutive opening and ending curly brackets are different.



## 题目解读

&emsp;输出所有可能字符串，按照字典序排列。

```java
class Solution {
    public int makeArrayIncreasing(int[] arr1, int[] arr2) {

    }
}
```

## 程序设计

* 首先预处理，组织字符串为字符链表，使得候选字符按照字典序排列；然后利用简单的回溯思路，回溯尝试。

```java
class Solution {
    int idx = 0;

    public String[] expand(String S) {
        if (S == null || S.isEmpty()) return new String[0];

        // 组合数目
        int count = 1;
        // 是否在花括号中
        boolean flag = false;
        List<List<Character>> seq = new LinkedList<>();
        List<Character> temp = new ArrayList<>();
        for (char c : S.toCharArray()) {
            if (c == ',') continue;

            if (c == '{') {
                // 标识置为true
                flag = true;
            } else if (c == '}') {
                // 排序加入序列，重置链表
                if (!temp.isEmpty()) {
                    Collections.sort(temp);
                    seq.add(temp);
                    count *= temp.size();
                    temp = new ArrayList<>();
                }
                // 标识结束
                flag = false;
            } else {
                temp.add(c);
                // 不在{}中的字符直接添加进去
                if (!flag) {
                    seq.add(temp);
                    temp = new ArrayList<>();
                }
            }
        }

        String[] res = new String[count];
        backTrace(seq, 0, new StringBuffer(), res);
        return res;
    }

    private void backTrace(List<List<Character>> seq, int no, StringBuffer sb, String[] res) {、
        // 找到一组解
        if (no == seq.size()) {
            res[idx++] = sb.toString();
            return;
        }

        List<Character> list = seq.get(no);
        for (Character c : list) {
            // 试探
            sb.append(c);
            backTrace(seq, no + 1, sb, res);
            // 回溯
            sb.setLength(sb.length() - 1);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\frac{N}{M}^{M})$，空间复杂度为$O(\frac{N}{M}^M)$，其中$N$为字符串长度，$M$为平均的候选字符串长度。

执行用时：3ms，在所有java提交中击败了88.78%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。