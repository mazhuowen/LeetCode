[toc]

Given a list of daily temperatures T, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.

For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

Note: The length of temperatures will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.



## 题目解读

&emsp;给定数组，每个元素表示每日的温度，给出当天后等多久气温会升高超过当天。如果没有则记为0。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        
    }
}
```

## 程序设计

* 从左至右遍历数组，若当前值小于等于栈顶，入栈；否则出栈直到栈为空或栈顶大于当前值。当前值入栈。最后栈中剩余的温度没有后续更高的温度，记为0。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < T.length; i++) {
            // 栈顶小于当前值，出栈并记录天数
            while(!stack.isEmpty() && T[i] > T[stack.peek()]) {
                // 记录天数，并出栈
                res[stack.peek()] = i - stack.peek();
                stack.pop();
            }
            // 栈为空或当前值小于等于栈顶值
            stack.push(i);
        }
        // 没有更大的气温
        while(!stack.isEmpty()) {
            res[stack.pop()] = 0;
        }
        return res;
    }
}
```

## 性能分析

&emsp;由于主要涉及到数组遍历，时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：73ms，在所有java提交中击败了53.43%的用户。

内存消耗：43.9MB，在所有java提交中击败了12.16%的用户。

## 官方解题

&emsp;思路同上，只是从尾开始遍历。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] ans = new int[T.length];
        Stack<Integer> stack = new Stack();
        for (int i = T.length - 1; i >= 0; --i) {
            while (!stack.isEmpty() && T[i] >= T[stack.peek()]) stack.pop();
            ans[i] = stack.isEmpty() ? 0 : stack.peek() - i;
            stack.push(i);
        }
        return ans;
    }
}
```

> 社区运行时间较少的方法不适用数据结构，采用暴力的方法，从尾遍历，然后从当前气温向后遍历查找较大气温，两层循环。