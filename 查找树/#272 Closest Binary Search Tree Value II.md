[toc]

Given a non-empty binary search tree and a target value, find $k$ values in the BST that are closest to the target.



**Note**:

* Given target value is a floating point.
* You may assume $k$ is always valid, that is: $k ≤ \text{total nodes}$.
* You are guaranteed to have only one unique set of $k$ values in the BST that are closest to the target.

**Follow up**:
Assume that the BST is balanced, could you solve it in less than $O(n)$ runtime (where n = total nodes)?



## 题目解读

&emsp;找到$k$个与目标值相近的元素。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {

    }
}
```

## 程序设计

* 最简单的想法就是遍历所有的结点然后通过堆排序得到$k$个最接近的值。

```java
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        // 最大堆，维护在k个
        PriorityQueue<Long> queue = new PriorityQueue<>((a, b) -> {
            double diff = Math.abs(b - target) - Math.abs(a - target);
            if(diff == 0) return 0;
            else if(diff > 0) return 1;
            else return -1; 
        });
        // 递归入堆
        closestKValues(root, k, queue);
        // 拼接
        List<Integer> res = new LinkedList<>();
        while(!queue.isEmpty()) {
            res.add(queue.poll().intValue());
        }
        return res;
    }

    private void closestKValues(TreeNode root, int k, PriorityQueue<Long> queue) {
        if(root == null) {
            return;
        }
        // 入堆
        queue.add((long)root.val);
        if(queue.size() > k) {
            queue.poll();
        }
        closestKValues(root.left, k, queue);
        closestKValues(root.right, k, queue);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2k)$，空间复杂度为$O(k)$。

执行用时：9ms，在所有java提交中击败了25.00%的用户。

内存消耗：42.8MB，在所有java提交中击败了6.56%的用户。

## 官方解题

&emsp;暂无，密切关注。社区一种思路是利用二叉查找树的中序序列有序性，保持一个$k$的窗口比较并加入、删除。

```java
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> res = new LinkedList<>();
        closestKValues(root, target, k, res);
        return res;
    }
	// 中序遍历递归
    private void closestKValues(TreeNode root, double target, int k, List<Integer> queue) {
        if (root == null) return;
        
        closestKValues(root.left, target, k, queue);
        // 窗口已满，且当前值不符合要求，根据后面的有序性后面结点也不符合要求，直接返回
        if(queue.size() == k && Math.abs((long)root.val - target) >= Math.abs((long)queue.get(0) - target)) return;
        
        // 入队尾，如果满了，则出队队头
        queue.add(root.val);
        if(queue.size() > k) queue.remove(0);
        closestKValues(root.right, target, k, queue);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.8MB，在所有java提交中击败了6.56%的用户。

&emsp;题目进阶在平衡的二叉查找树中要实现小于$O(N)$时间复杂度的算法。由于是平衡二叉树，搜索最接近`target`的时间为$O(\log_2N)$，搜索到`target`后，可以向前、向后遍历结点直到$k$个。寻找前驱主要有一下三种情况：

* 当前结点存在左子节点，则前驱是左子节点的右支路末端结点；
* 当前结点不存在左子结点，是其父节点的右子节点，则前驱是其父节点；
* 当前结点不存在左子节点，是其父节点的左子节点，则向上遍历直到遇到祖先结点是其父节点的右子节点，前驱就是该祖先结点。

寻找后继同理。

```java
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> res = new LinkedList<>();
        // 记录路径（分别前后查找）
        Stack<TreeNode> prePath = new Stack<>();
        Stack<TreeNode> nextPath = new Stack<>();
        // 查找最接近target的结点
        TreeNode temp = root;
        TreeNode min = root;
        while(temp != null) {
            prePath.push(temp);
            // 找到直接退出
            if(temp.val == target) {
                min = temp;
                break;
            }
            // 更新
            if(Math.abs((long)min.val - target) > Math.abs((long)temp.val - target)) { 
                min = temp;
            }
            // 继续搜索
            if(temp.val > target) {
                temp = temp.left;
            } else {
                temp = temp.right;
            }
        }
        // 将min后的结点删除
        while(prePath.peek() != min) {
            prePath.pop();
        }
        // 复制一份
        nextPath.addAll(prePath);

        // 将min添加进去
        res.add(min.val);
        k--;
        TreeNode pre = getPred(prePath);
        TreeNode next = getNext(nextPath);
        // 存在前驱或后继
        while(pre != null && next != null && k > 0) {
            // 前驱差值更小，迭代前驱
            if(Math.abs((long)pre.val - target) < Math.abs((long)next.val - target)) {
                res.add(pre.val);
                pre = getPred(prePath);
            } 
            // 迭代后继
            else {
                res.add(next.val);
                next = getNext(nextPath);
            }
            k--;
        }
        // 只存在前驱或后继的情况，继续迭代
        while(pre != null && k > 0) {
            res.add(pre.val);
            pre = getPred(prePath);
            k--;
        }
        while(next != null && k > 0) {
            res.add(next.val);
            next = getNext(nextPath);
            k--;
        }
        return res;
    }

    // 查找前驱
    private TreeNode getPred(Stack<TreeNode> path) {
        if(path.isEmpty()) {
            return null;
        }
        // 当前结点，需要查找当前结点的前驱
        TreeNode cur = path.pop();
        // 存在左子树，则前驱是左子结点的右支路
        if(cur.left != null) {
            // 重点：这儿要把当前结点加入进去，不然栈中存的就不是连续的路径了
            // 此时栈中子结点是当前结点的左子树，在求子节点前驱时或寻找右父节点，故当前结点会被作为路径跳过，而不影响结果
            path.push(cur);
            cur = cur.left;
            while(cur.right != null) {
                // 加入路径
                path.push(cur);
                cur = cur.right;
            }
            path.push(cur);
            return cur;
        }
        // 左子树为空，则检测父节点
        else {
            // 当前结点存在父节点且是其右子节点，则前驱是父结点
            // 如果是父节点的左子节点，则向路径遍历，直到有一个祖先结点右子树是当前结点
            while(!path.isEmpty() && path.peek().left == cur) {
                cur = path.pop();
            }
            // 遍历到栈空没找到，说明没有前驱
            if(path.isEmpty()) {
                return null;
            }
            // 此时栈顶是前驱结点
            return path.peek();
        }
    }
    // 查找后继
    private TreeNode getNext(Stack<TreeNode> path) {
        if(path.isEmpty()) {
            return null;
        }
        TreeNode cur = path.pop();
        if(cur.right != null) {
            path.push(cur);
            cur = cur.right;
            while(cur.left != null) {
                path.push(cur);
                cur = cur.left;
            }
            path.push(cur);
            return cur;
        }
        // 右子节点
        else {
            while(!path.isEmpty() && path.peek().right == cur) {
                cur = path.pop();
            }
            if(path.isEmpty()) {
                return null;
            }
            return path.peek();
        }
    }
}
```

&emsp;虽然查找一次前驱或后继的时间复杂度可能是$O(\log_2N)$，但是$k$个分摊下来平均时间复杂度为$O(1)$，则总的时间复杂度为$O(\log_2N + k)$；空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了84.48%的用户。

内存消耗：41.7MB，在所有java提交中击败了6.56%的用户。