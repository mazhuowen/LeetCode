[toc]

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.



## 题目解读

&emsp;给定嵌套数组，数组元素是整数或数组，实现迭代器功能，扁平化的输出整数元素。

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {

    public NestedIterator(List<NestedInteger> nestedList) {
        
    }

    @Override
    public Integer next() {
        
    }

    @Override
    public boolean hasNext() {
        
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

## 程序设计

* 在题目中已说明代码总会先执行`hasNext`方法再执行`next`迭代。考虑到列表中嵌套列表，可在`hasNext`判断时展开，而`next`只需返回数据。
* 可以使用双向队列的方式，将列表中的对象依次入队；`hasNext`就是判断队列头是否为空，若不是空还要判断队列头是否是嵌套列表，是嵌套列表则还需要从列表头将嵌套队列倒序入队；`next只是简单的返回队头数值，并出队。`
* 上述方法中，构建迭代器时顺序入队，而展开队首列表时需要倒序从头入队。可以使用栈，统一所有操作为倒序入栈。

```java
public class NestedIterator implements Iterator<Integer> {
    private Stack<NestedInteger> stack;

    public NestedIterator(List<NestedInteger> nestedList) {
        // 初始化
        stack = new Stack<>();
        // 倒序入栈
        for(int i = nestedList.size() - 1; i >= 0; i--) {
            stack.push(nestedList.get(i));
        }
    }

    @Override
    public Integer next() {
        if(stack.isEmpty()) {
            return null;
        }
        // 出栈返回
        return stack.pop().getInteger();
    }

    @Override
    public boolean hasNext() {
        // 栈顶，也就是接下来要遍历的值是列表
        while(!stack.isEmpty() && !stack.peek().isInteger()) {
            List<NestedInteger> tempList = stack.pop().getList();
            // 倒序入栈
            for(int i = tempList.size() - 1; i >= 0; i--) {
                stack.push(tempList.get(i));
            }
        }
        // 栈为空，或者列表为空如[[]]导致入栈为空，这两种情况都返回false
        if(stack.isEmpty()){
            return false;
        }
        return true;
    }
}
```

测试样例：空嵌套列表、普通列表。

## 性能分析

&emsp;对于`next`操作，时间复杂度为$O(1)$，对于`hasNest`操作，时间复杂度与列表结构有关，最坏情况列表有$N$个嵌套，每个嵌套列表的首元素又是一个嵌套（最后一个嵌套首元素是整数），假设每个嵌套列表长$M$，则时间复杂度为$O(NM)$。空间复杂度就是整体包含的元素个数。

 执行用时：6ms，在所有java提交中击败了22.87%的用户。

内存消耗：37.6MB，在所有java提交中击败了49.65%的用户。

## 官方解题

&emsp;暂无，密切关注。

> 社区运行时间较快的方法，都是在构造函数中一步到位展开所有链表；要么使用双向队列，要么使用递归。