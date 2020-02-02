[toc]

Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

Note: The length of given array won't exceed 10000.



## 题目解读

&emsp;对一个循环数组寻找每个数较大的后继，可知数组中最大的那个数没有较大后继，为-1，其它数都有较大后继。

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        
    }
}
```

## 程序设计

* 由于是循环数组，遍历一次不能得到所有数的较大后继，需要遍历两次。

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int[] res = new int[nums.length];
        // 初始化为-1
        for(int i = 0; i < nums.length; i++) {
            res[i] = -1;
        }
        Stack<Integer> stack = new Stack<>();
        // 遍历两次
        for(int i = 0; i < 2 * nums.length; i++) {
            while(!stack.isEmpty() && nums[stack.peek()] < nums[i % nums.length]) {
                res[stack.pop()] = nums[i % nums.length];
            }
            stack.push(i % nums.length);
        }

        return res;
    }
}
```

测试样例：顺序数字数组测试循环；相同数字数组；普通数组。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：47ms，在所有java提交中击败了55.53%的用户。

内存消耗：45.3MB，在所有java提交中击败了15.04%的用户。

## 官方解题

&emsp;官方遍历从数组尾开始，时间复杂度、空间复杂度不变，运行时间更长。

```java
public class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int[] res = new int[nums.length];
        Stack<Integer> stack = new Stack<>();
        // 从尾部开始遍历
        for (int i = 2 * nums.length - 1; i >= 0; --i) {
            // 栈顶小于当前数字，出栈
            while (!stack.empty() && nums[stack.peek()] <= nums[i % nums.length]) {
                stack.pop();
            }
            // 更新数组
            res[i % nums.length] = stack.empty() ? -1 : nums[stack.peek()];
            stack.push(i % nums.length);
        }
        return res;
    }
}
```

> 社区一些运行时间较快的代码，没有使用数据结构或者使用简单的数组作为栈使用。