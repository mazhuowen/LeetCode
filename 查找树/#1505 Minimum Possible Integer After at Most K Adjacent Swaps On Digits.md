[toc]

Given a string `num` representing **the digits** of a very large integer and an integer $k$.

You are allowed to swap any two adjacent digits of the integer **at most** $k$ times.

Return the minimum integer you can obtain also as a string.

 

**Example 1**:

<img src="..\images\#1505_exp1.jpg"  />

```
Input: num = "4321", k = 4
Output: "1342"
Explanation: The steps to obtain the minimum integer from 4321 with 4 adjacent swaps are shown.
```

**Example 2**:

```
Input: num = "100", k = 1
Output: "010"
Explanation: It's ok for the output to have leading zeros, but the input is guaranteed not to have any leading zeros.
```

**Example 3**:

```
Input: num = "36789", k = 1000
Output: "36789"
Explanation: We can keep the number without any swaps.
```

**Example 4**:

```
Input: num = "22", k = 22
Output: "22"
```

**Example 5**:

```
Input: num = "9438957234785635408", k = 23
Output: "0345989723478563548"
```



**Constraints**:

* $1 \le \text{num.length} \le 30000$
* `num` contains **digits** only and doesn't have **leading zeros**.
* $1 \le k \le 10^9$



## 题目解读

&emsp;给定数组和可交换的最大交换次数$k$，两两交换相邻的数字，求可交换得到的最小数字。

```java
class Solution {
    public String minInteger(String num, int k) {

    }
}
```

## 程序设计

* 在可交换的范围内，必然是将最小数字交换到当前位置最优；可将字符索引根据字符值排序，这样第一个交换字符就是从左往右遍历遇到的索引小于等于$k$的值，但问题在于第二个交换字符该怎么处理；由于第一次交换后，有部分索引会后移，这样导致排序的思路行不通；
* 引入树状数组，当前字符交换到前面后可用树状数组更新记录；这样首先将字符索引根据值维护在不同的队列，每次都从$0 \sim 9$尝试字符所在位置是否可交换到当前位置。

```java
class Solution {
    public String minInteger(String num, int k) {
        int n = num.length();
        // 保存0~10的字符索引
        Queue<Integer>[] pos = new Queue[10];
        for (int i = 0; i < 10; i++) pos[i] = new LinkedList<>();
        // 因为树状数组的缘故，采用索引加一
        for (int i = 0; i < n; i++) pos[num.charAt(i) - '0'].add(i);

        BinaryIndexedTree tree = new BinaryIndexedTree(n, false, null);
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < n; i++) {
            // 从较小的数字开始尝试是否可交换到当前位置
            for (int j = 0; j < 10; j++) {
                if (pos[j].isEmpty()) continue;
                // 查询索引后移的数目
                int behind = tree.rangeSum(pos[j].peek() + 1, n - 1);
                // 交换数不够
                if (pos[j].peek() + behind - i > k) continue;
                sb.append(j);
                // 更新表示当前位置交换到前面
                tree.update(pos[j].peek(), 1);
                k -= pos[j].poll() + behind - i;
                break;
            }
        }

        return sb.toString();
    }
}

class BinaryIndexedTree {
    private int[] bitArr;

    BinaryIndexedTree(int n, boolean init, int[] origin) {
        // 1~n，0为根节点
        this.bitArr = new int[n + 1];
        // O(N)初始化
        if (init) {
            for (int i = 0; i < n; i++) bitArr[i + 1] = origin[i];
            for (int i = 1; i <= n; i++) {
                int j = i + lowBit(i);
                if (j <= n) bitArr[j] += bitArr[i];
            } 
        }
    }

    private static int lowBit(int x) {
        return x & (-x);
    }

    public void update(int idx, int delta) {
        idx++;
        while (idx < bitArr.length) {
            bitArr[idx] += delta;
            idx += lowBit(idx);
        }
    }

    public int prefixSum(int idx) {
        idx++;
        int sum = 0;
        while (idx > 0) {
            sum += bitArr[idx];
            idx -= lowBit(idx);
        }
        return sum;
    }

    public int rangeSum(int fromIdx, int toIdx) {
        return prefixSum(toIdx) - prefixSum(fromIdx - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：42 ms, 在所有 Java 提交中击败了83.53%的用户

内存消耗：40.9 MB, 在所有 Java 提交中击败了44.70%的用户。

## 官方解题

&emsp;上述思路参考官方解题。