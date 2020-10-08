[toc]

Given a string `s`, we make queries on substrings of `s`.

For each query `queries[i] = [left, right, k]`, we may **rearrange** the substring `s[left], ..., s[right]`, and then choose **up to** $k$ of them to replace with any lowercase English letter. 

If the substring is possible to be a palindrome string after the operations above, the result of the query is `true`. Otherwise, the result is `false`.

Return an array `answer[]`, where `answer[i]` is the result of the `i`-th query `queries[i]`.

Note that: Each letter is counted **individually** for replacement so if for example `s[left..right] = "aaa"`, and `k = 2`, we can only replace two of the letters.  (Also, note that the initial string `s` is never modified by any query.)



**Constraints**:

* $1 \le \text{s.length, queries.length} \le 10^5$
* $0 \le \text{queries[i][0]} \le \text{queries[i][1]} < \text{s.length}$
* $0 \le \text{queries[i][2]} \le \text{s.length}$
* `s` only contains lowercase English letters.



## 题目解读

&emsp;给定区间，重新排列区间并最多替换$k$个字符，使得字符串是回文，判断是否存在替换方案。

```java
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {

    }
}
```

## 程序设计

* 由于是重新重新排列区间，关注的就是区间的字符计数。由于需要重复统计区间字符数，首先想到的是线段树，通过线段树统计并判断。

```java
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
        SegmentTree tree = new SegmentTree(s);
        int n = queries.length;
        List<Boolean> res = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            int[] query = queries[i];
            res.add(check(tree.search(query[0], query[1]), query[1] - query[0] + 1, query[2]));
        } 

        return res;
    }

    private boolean check(int[] counter, int len, int k) {
        // 奇数类别计数
        int count = 0;
        for (int num : counter) {
            if (num % 2 == 1) count++;
        }
        // 长度为奇数，中间的数字选择一个奇数类别，计数减一
        if (len % 2 == 1) count--;
        
        // 将一半计数为奇数的数字各选一个变为另一半的数字，从而整体是偶数
        // 此处count必然是偶数
        return count / 2 <= k;
    }
}

class SegmentTree {
    Node root;

    SegmentTree(String s) {
        this.root = new Node(0, s.length() - 1);
        // 构建
        this.root.count(s.toCharArray());
    }

    public int[] search(int s, int e) {
        return this.root.search(s, e);
    }
}

class Node {
    int start, end;
    Node left, right;
    // 区间计数
    int[] counter;

    Node(int start, int end) {
        this.start = start;
        this.end = end;
    }

    private int mid() {
        return (this.start + this.end) / 2;
    }

    private Node left() {
        if (this.left == null) this.left = new Node(this.start, mid());
        return this.left;
    }

    private Node right() {
        if (this.right == null) this.right = new Node(mid() + 1, this.end);
        return this.right;
    }

    private int[] union(int[] c1, int[] c2) {
        int[] res = new int[26];
        for (int i = 0; i < 26; i++) {
            if (c1 != null) res[i] += c1[i];
            if (c2 != null) res[i] += c2[i];
        }
        return res;
    }

    // 统计当前区间
    public int[] count(char[] seq) {
        if (this.start == this.end) {
            this.counter = new int[26];
            this.counter[seq[start] - 'a'] = 1;
        } else {
            // 合并左右区间值
            this.counter = union(this.left().count(seq), this.right().count(seq));
        }
        return this.counter;
    }

    public int[] search(int s, int e) {
        if (s > this.end || e < this.start || s > e) return null;
        if (s <= this.start && e >= this.end) return this.counter;

        int mid = mid();
        return union(this.left().search(s, Math.min(mid, e)), this.right().search(Math.max(mid + 1, s), e));
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2M)$，空间复杂度为$O(M)$。

执行用时：419ms，在所有java提交中击败了5.30%的用户。

内存消耗：121.8MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用非常技巧的方法，即使用二进制表示每个字符的奇偶情况，通过类似前缀和的思路来求区间计数。判断是否可组成回文思路同上。

```java
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
        // 统计0~i的字符串字符奇偶情况
        int[] stat = new int[s.length() + 1];
        int cur = 0, idx = 1;
        for (char c : s.toCharArray()) {
            // 亦或，偶数为0，奇数为1
            cur ^= 1 << (c - 'a');
            stat[idx++] = cur;
        }

        List<Boolean> res = new LinkedList<>();
        for (int[] query : queries) res.add(check(stat[query[0]] ^ stat[query[1] + 1], query[1] - query[0] + 1, query[2]));
        return res;
    }

    private boolean check(int stat, int len, int k) {
        // 奇数类别计数
        int count = Integer.bitCount(stat);
        // 长度为奇数，中间的数字选择一个奇数类别，计数减一
        if (len % 2 == 1) count--;
        
        // 将一半计数为奇数的数字各选一个变为另一半的数字，从而整体是偶数
        // 此处count必然是偶数
        return count / 2 <= k;
    }
}
```

&emsp;时间复杂度为$O(\max(N, M))$，空间复杂度为$O(\max(N,M))$。

执行用时：11ms，在所有java提交中击败了96.26%的用户。

内存消耗：97.5MB，在所有java提交中击败了73.00%的用户。