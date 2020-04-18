[toc]

Given a **non-empty** array of numbers, $a_0, a_1, a_2, \dots , a_{n-1}$, where $0 \le a_i < 2^{31}$.

Find the maximum result of $a_i$ XOR $a_j$, where $0 \le i, j < n$.

Could you do this in $O(n)$ runtime?



## 题目解读

&emsp;给定数组，找到最大的异或值。

```java
class Solution {
    public int findMaximumXOR(int[] nums) {
        
    }
}
```

## 程序设计

* 题目要求时间复杂度为$O(N)$，可借鉴字典树的思路，采用前缀匹配的思想，将数字的位按照从高到底加入字典树，依次向下遍历，优先选择当前位不相同的分支尝试，最后比较返回最大值。
* 由于每一位都是0或1，退化为一个二叉树。

```java
class Solution {
    public int findMaximumXOR(int[] nums) {
        // 加入字典树
        Tree root = new Tree();
        for (int num : nums) {
            root.insert(num);
        }
        // 获取最大值
        return root.getMaxXOR();
    }
}

class Tree {
    // 记录最大亦或值
    int max = Integer.MIN_VALUE;
    Tree left;
    Tree right;
    
    public void insert(int num) {
        // 采用栈保存数字位数，方便从高到低加入字典树
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < 31; i++) {
            stack.push(num & 1);
            num >>= 1;
        }
        // 遍历加入
        Tree cur = this;
        while (!stack.isEmpty()) {
            // 加入右分支
            if (stack.pop() == 1) {
                if (cur.right == null) cur.right = new Tree();
                cur = cur.right;
            } 
            // 加入左分支
            else {
                if (cur.left == null) cur.left = new Tree();
                cur = cur.left;
            }
        }
    }

    public int getMaxXOR() {
        getXOR(0, this, this);
        return max;
    }

    // 返回表示尝试路径是否存在，避免无用尝试超时
    private boolean getXOR(int num, Tree node1, Tree node2) {
        // 不满足条件的路径
        if (node1 == null || node2 == null) return false;
        // 已到达最末端，更新结果
        if (node1.left == null && node1.right == null  
            && node2.left == null && node2.right == null) {
            max = Math.max(max, num);
            return true;
        } 

        // 优先尝试当前位互异的分之，注意两个都要尝试
        boolean flag = getXOR((num << 1) | 1, node1.left, node2.right);
        flag =  getXOR((num << 1) | 1, node1.right, node2.left) || flag;

        // 上述路径不存在，则尝试，注意两种情况都需要尝试
        if (!flag) {
            flag = getXOR((num << 1) | 0, node1.right, node2.right);
            flag = getXOR((num << 1) | 0, node1.left, node2.left) || flag;
        }
        
        return flag;
    }
}
```

## 性能分析

&emsp;构建字典树时间复杂度为$O(32N)$，即$O(N)$，遍历查找最大异或值，最坏情况是一棵完全二叉树，需要尝试所有可能，时间复杂度为$O(2^{31})$，根据加法法则，最后时间复杂度为$O(N)$。空间复杂度为$O(32N)$，即$O(N)$。

执行用时：36ms，在所有java提交中击败了85.24%的用户。

内存消耗：48.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方主要思路是前缀匹配，提出了字典树和哈希表两种方式保存二进制前缀。