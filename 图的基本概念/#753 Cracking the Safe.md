[toc]

There is a box protected by a password. The password is a sequence of n digits where each digit can be one of the first `k` digits $0, 1, \cdots, k-1$.

While entering a password, the last `n` digits entered will automatically be matched against the correct password.

For example, assuming the correct password is `"345"`, if you type `"012345"`, the box will open because the correct password matches the suffix of the entered password.

Return any password of minimum length that is guaranteed to open the box at some point of entering it.



**Note**:

* `n` will be in the range `[1, 4]`.
* `k` will be in the range `[1, 10]`.
* $k^n$ will be at most `4096`.



## 题目解读

&emsp;最短的密码链。

```java
class Solution {
    public String crackSafe(int n, int k) {

    }
}
```

## 程序设计

* 由于共$k^n$个密码，如果使用回溯必然超时。转变思路，对于$n$位的密码，可看作是$n-1$位的状态后拼接$0 \sim k-1$的数字组成新的密码的过程，$n-1$位的状态有$k^{n - 1}$个，可看作是节点，而每个状态节点有出边$k$个，入边$k$个，即共$k^n$个边。问题转化位从起始状态出发的欧拉回路问题，由于每个状态入度与出度相等，必然存在欧拉回路。
* 以$n=3$，$k = 2$为例，状态分别是$00$、$01$、$10$、$11$，边共计$8$条。

<img src="..\images\#753.png" style="zoom:120%;" />

```java
class Solution {
    public String crackSafe(int n, int k) {
        StringBuffer sb = new StringBuffer();
        // 拼接n-1位0，即初始状态
        for (int i = 0; i < n - 1; i++) sb.append("0");

        StringBuffer res = new StringBuffer();
        Set<String> visited = new HashSet<>();
        // 丛n-1位0出发进行欧拉回路遍历
        dfs(sb.toString(), k, visited, res);
        res.append(sb);
        return res.toString();
    }

    private void dfs(String stat, int k, Set<String> visited, StringBuffer res) {
        for (int i = 0; i < k; i++) {
            // 当前的数字锁
            String tmp = stat + Integer.toString(i);
            if (!visited.contains(tmp)) {
                visited.add(tmp);
                dfs(tmp.substring(1), k, visited, res);
                // 欧拉回路拼接
                res.append(Integer.toString(i));
            }
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(n * k^n)$，空间复杂度为$O(k^{n-1})$。

执行用时：13ms，在所有java提交中击败了92.31%的用户。

内存消耗：40.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;见上。