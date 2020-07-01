[toc]

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of $\text{length} = n$.



## 题目解读

&emsp;找到所有长度为$n$的中心对称串。

```java
class Solution {
    public List<String> findStrobogrammatic(int n) {

    }
}
```

## 程序设计

* 采用回溯，需要注意$0$不能在开头，除非是$0$本身.

```java
class Solution {
    public List<String> findStrobogrammatic(int n) {
        List<String> res = new LinkedList<>();

        findStrobogrammatic(new char[n], 0, res);
        return res;
    }

    private void findStrobogrammatic(char[] strs, int idx, List<String> res) {
        // 完成拼接
        if (idx == (strs.length + 1) / 2) {
            res.add(new String(strs));
            return;
        }

        // 0不能在开头，除非长度只有一位
        if (strs.length == 1 || idx != 0) {
            strs[idx] = strs[strs.length - idx - 1] = '0';
            findStrobogrammatic(strs, idx + 1, res);
        }
        // 拼接1、8
        strs[idx] = strs[strs.length - idx - 1] = '1';
        findStrobogrammatic(strs, idx + 1, res);
        strs[idx] = strs[strs.length - idx - 1] = '8';
        findStrobogrammatic(strs, idx + 1, res);
        // 如果是奇数长，不能在中间，拼接6、9
        if (idx != strs.length / 2) {      
            strs[idx] = '6';
            strs[strs.length - idx - 1] = '9';
            findStrobogrammatic(strs, idx + 1, res);
            strs[idx] = '9';
            strs[strs.length - idx - 1] = '6';
            findStrobogrammatic(strs, idx + 1, res);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(5^{\frac{n}{2}})$，空间复杂度为$O(5^{\frac{n}{2}})$。

执行用时：3ms,在所有java提交中击败了100.00%的用户。

内存消耗：48.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。