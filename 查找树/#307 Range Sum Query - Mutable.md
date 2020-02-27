[toc]

Given an integer array `nums`, find the sum of the elements between indices $i$ and $j$ ($i \le j$), inclusive.

The update$(i, val)$ function modifies `nums` by updating the element at index $i$ to $val$.

Note:

* The array is only modifiable by the `update` function.
* You may assume the number of calls to `update` and `sumRange` function is distributed evenly.



## 题目解读

&emsp;给定数组，求出数组索引区间的元素和。需注意数组的元素值会发生变化。题目提示更新数组值的操作和统计范围之和的操作是均匀分布的。

```java
class NumArray {

    public NumArray(int[] nums) {

    }
    
    public void update(int i, int val) {

    }
    
    public int sumRange(int i, int j) {

    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```

## 程序设计

* 不考虑性能，最基础的解法是每次都重新统计。

```java
class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }
    
    public void update(int i, int val) {
        if(i < nums.length) {
            nums[i] = val;
        }
    }
    
    public int sumRange(int i, int j) {
        int sum = 0;
        for(int idx = i; idx < nums.length && idx <= j; idx++) {
            sum += nums[idx];
        }
        return sum;
    }
}
```

* 上述暴力法每次都要重新统计区间之和，在更新和统计均匀调用的情况下，性能较低。由于是区间问题，可以考虑线段树。首先定义线段树类：

```java
class Tree {
    // 区间
    int start;
    int end;
    // 和
    int val;
	// 子节点指针
    Tree left;
    Tree right;

    Tree(int start, int end, int val) {
        this.start = start;
        this.end = end;
        this.val = val;
    }

    Tree(int start, int end, int val, Tree left, Tree right) {
        this.start = start;
        this.end = end;
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

功能实现：

```java
class NumArray {
    private Tree root;

    public NumArray(int[] nums) {
        if(nums.length == 0) {
            root = null;
        }
        // 构建完全二叉树
        Queue<Tree> queue = new LinkedList<>();
        // 初始叶节点
        for(int i = 0; i < nums.length; i++) {
            queue.add(new Tree(i, i, nums[i]));
        }
        // 两个两个子结点的构造父节点
        while(queue.size() > 1) {
            int len = queue.size();
            while(len - 1 > 0) {
                Tree left = queue.poll();
                Tree right = queue.poll();
                Tree parent = new Tree(left.start, right.end, left.val + right.val, left, right);
                queue.add(parent);
                len -= 2;
            }
            // 余下一个，重新入队，参加下一轮构造
            if(len == 1) {
                queue.add(queue.poll());
            }
        }
        root = queue.poll();
    }
    
    public void update(int i, int val) {
        if(root == null) {
            return;
        }
        update(root, i, val);
    }

    private int update(Tree root, int i, int val){
        if(root == null) return 0;
        // i超出范围，不是要更新的结点，返回结点值
        if(i < root.start || i > root.end) return root.val;
        // 找到叶节点，更新并返回
        if(root.start == i && root.end == i) {
            root.val = val;
            return root.val;
        }
        // 递归计算
        int left = update(root.left, i, val);
        int right = update(root.right, i, val);
        // 重新计算路径上的和
        root.val = left + right;
        return root.val;
    }
    
    public int sumRange(int i, int j) {
       return sumRange(root, i, j);
    }

    private int sumRange(Tree root, int i, int j) {
        if(root == null) return 0;
        // 不合法的参数
        if(i > root.end || j < root.start) {
            return 0;
        }
        // 包含区间，则返回结点值
        if(i <= root.start && j >= root.end) {
            return root.val;
        }
        // 否则区间与当前结点区间相交，递归遍历子节点
        return sumRange(root.left, i, j) + sumRange(root.right, i, j);
    }
}
```

## 性能分析

&emsp;暴力法统计范围时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：94ms，在所有java提交中击败了44.78%的用户。

内存消耗：47.5MB，在所有java提交中击败了72.88%的用户。

&emsp;构建线段树的方法构造树的时间复杂度为$O(N)$；空间复杂度为$O(N)$。统计范围时间复杂度为$O(\log_2N)$，因为在向下遍历的过程中假设路径上区间不包括的索引为$j$，则其它区间都是路径上的分叉结点，不会再深入遍历下去，只有$j$会遍历到叶节点；空间复杂度为$O(\log_2N)$。因为包含索引$i$的区间只有一条路径上有，更新结点时间复杂度为$O(\log_2N)$；空间复杂度为$O(1)$。

执行用时：19 ms，在所有java提交中击败了78.37%的用户。

内存消耗：47.1MB，在所有java提交中击败了75.79%的用户。

## 官方解题

&emsp;由于是完全二叉树，可以用数组表示，官方将线段树的实现通过数组表示。

```java
class NumArray {
    // 线段树
    int[] tree;
    // 叶节点开始索引
    int index;

    public NumArray(int[] nums) {
        if(nums.length > 0) {
            index = nums.length;
            // 总共有2 * n - 1个结点，根节点在索引1
            tree = new int[2 * index];
            // 初始化叶节点
            for(int i = 0; i < index; i++) {
                tree[i + index] = nums[i];
            }
            // 初始化父节点
            for(int i = index - 1; i > 0; i--) {
                tree[i] = tree[2 * i] + tree[2 * i + 1];
            }
        }
    }
    
    public void update(int i, int val) {
        // 索引不合法
        if(i >= index || i < 0) {
            return;
        }
        // 差值
        int diff = val - tree[i + index];
        // 更新叶节点
        tree[i + index] = val;
        // 更新父节点
        int p = (i + index) / 2;
        for(; p > 0; p /= 2) {
            tree[p] += diff;
        }
    }
    
    public int sumRange(int i, int j) {
        // 索引不合法
        if(i >= index || j < 0) {
            return 0;
        }
        i += index;
        j += index;
        int sum = 0;
        while(i <= j) {
            // 下界是父节点的右结点，表示父节点包含的范围大于子节点，将子节点加入
            // 同时下界后移，若移动到是父节点的左结点，则表示可能包含在父节点区间
            if ((i % 2) == 1) {
                sum += tree[i];
                i++;
            }
            // 同理上界是父节点的左结点，表示父节点包含的范围大于子节点，将子节点加入
            // 同时上界前移，若移动到是父节点的右结点，则表示可能包含在父节点区间
            if((j % 2) == 0) {
                sum += tree[j];
                j--;
            }
            // 由于包含在父节点中，向上遍历，计算父节点的值
            i /= 2;
            j /= 2;
        }
        return sum;
    }
}
```

&emsp;构建时间复杂度为$O(N)$，空间复杂度为$O(N)$。更新时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。统计时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：14ms，在所有java提交中击败了99.87%的用户。

内存消耗：47.6MB，在所有java提交中击败了71.92%的用户。

> 从题目看有个默认的限制就是数组所有的元素之和还是整形，不发生溢出。需要和面试官沟通，如果考虑溢出需要采用特殊的结构保存，如字符串。