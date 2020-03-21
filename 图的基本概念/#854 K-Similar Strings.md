[toc]

Strings `A` and `B` are `K`-similar (for some non-negative integer `K`) if we can swap the positions of two letters in `A` exactly `K` times so that the resulting string equals `B`.

Given two anagrams `A` and `B`, return the smallest `K` for which `A` and `B` are `K`-similar.



**Note:**

* $1 \le \text{A.length} == \text{B.length} \le 20$
* `A` and `B` contain only lowercase letters from the set `{'a', 'b', 'c', 'd', 'e', 'f'}`



## 题目解读

&emsp;如果可以通过将`A`中的两个小写字母精确地交换位置`K`次得到与`B`相等的字符串，称字符串`A`和`B`的相似度为`K`。给定两个字母异位词`A`和`B`，返回`A`和`B`的相似度`K`的最小值。

```java
class Solution {
    public int kSimilarity(String A, String B) {

    }
}
```

## 程序设计

* 参考官方思路，采用队列保存一次所有可能的交换方案，直到遇到与目标字符串一致的结果，返回交换操作数。

```java
class Solution {
    public int kSimilarity(String A, String B) {
        if (A == null || B == null || A.length() != B.length()) throw new IllegalArgumentException("invalid param");
        if (A.equals(B)) return 0;
        // 记录生成的字符串和步数
        Map<String, Integer> record = new HashMap<>();
        Queue<String> queue = new LinkedList<>();
        queue.add(A);
        record.put(A, 0);
        while (!queue.isEmpty()) {
            // 当前字符串及从A交换的最少次数
            String cur = queue.poll();
            int count = record.get(cur);
            // 找到和B第一个不相符的字符
            int i = 0;
            for (; i < B.length(); i++) {
                if (cur.charAt(i) != B.charAt(i)) break;
            }
            // 找到所有的交换组合，并入队
            char[] strArr = cur.toCharArray();
            for (int j = i + 1; j < B.length(); j++) {
                if (cur.charAt(i) == B.charAt(j)) {
                    // 交换
                    swap(strArr, i, j);
                    // 一次交换后新生成的字符串
                    String newStr = new String(strArr);
                    // 如果不存在则放入，如果存在说明之前就可以交换到该字符串，所用轮数更小
                    if (record.get(newStr) == null) {
                        record.put(newStr, count + 1);
                        if (newStr.equals(B)) return record.get(newStr);
                        queue.add(newStr);
                    }
                    // 交换回来，继续下一轮交换
                    swap(strArr, i, j);
                }
            }
        }
        throw new RuntimeException("logic error");
    }

    private void swap(char[] str, int i, int j) {
        char temp = str[i];
        str[i] = str[j];
        str[j] = temp;
    }
}
```

## 性能分析

&emsp;本题时间复杂度分析比较复杂，时间复杂度$O(\sum_{k=0}^n \binom{N}{k} \frac{\min(2^k, (N-k)!)}{W * (\frac{N-k}{W})!})$，其中$N$是字符串的长度，$W$是字母的数量。

执行用时：24ms，在所有java提交中击败了73.23%的用户。

内存消耗：42.1MB，在所有java提交中击败了39.03%的用户。

## 官方解题

&emsp;上述方法参考官方思路。