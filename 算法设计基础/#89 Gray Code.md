[toc]

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer $n$ representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with $0$.



## 题目解读

&emsp;格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。给定一个代表编码总位数的非负整数$n$，打印其格雷编码序列。格雷编码序列必须以0开头。

```java
class Solution {
    public List<Integer> grayCode(int n) {

    }
}
```

## 程序设计

* 回溯尝试组合，直到有一个成功返回。

```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> res = new ArrayList<>();
        if (n < 0) return res;

        int size = 1 << n;
        // 标记是否加入
        boolean[] visited = new boolean[size];
        grayCode(0, n, visited, res);
        return res;
    }

    // cur为当前要加入的数
    private boolean grayCode(int cur, int n, boolean[] visited, List<Integer> res) {
        // 已加入
        if (visited[cur]) return false;

        // 加入
        visited[cur] = true;
        res.add(cur);
        // 找到编码，返回
        if (res.size() == visited.length) return true;

        // 试探
        int bit = 1;
        for (int i = 0; i < n; i++) {
            // 生成i位不同的数字
            int next = getNum(cur, bit, i);
            if (next >= 0 && next < visited.length && !visited[next] && grayCode(next, n, visited, res)) return true;
            bit <<= 1;
        }

        // 回溯
        visited[cur] = false;
        res.remove(res.size() - 1);
        return false;
    }

    private int getNum(int num, int bit, int i) {
        // i位为1，变为0
        if ((num & bit) == bit) num -= 1 << i;
        // i位为0，变为1
        else num += 1 << i;
        return num;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(2^N)$。

执行用时：1ms，在所有java提交中击败了86.66%的用户。

内存消耗：37.7MB，在所有java提交中击败了12.50%的用户。

## 官方解题

&emsp;暂无，密切关注。社区巧妙使用格雷编码的数学性质。

```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> ret = new ArrayList<>();
        for(int i = 0; i < 1<<n; ++i)
            ret.add(i ^ i>>1);
        return ret;
    }
}
```

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了12.50%的用户。