[toc]

We are given an array asteroids of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.



**Note**:

* The length of asteroids will be at most $10000$.
* Each asteroid will be a non-zero integer in the range $[-1000, 1000]$.



## 题目解读

&emsp;给定一个整数数组，负号表示向左移动，正号表示向右移动；整数的大小表示星星的大小。假设星星移动的速度一致，数组的索引代表了相对位置，只有当数组前的星星向右移动，数组后的星星向左移动时会发生碰撞。碰撞后体积小的星星会爆炸，若两者体积相同，则同时爆炸。

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        
    }
}
```

## 程序设计

* 从左至右遍历数组，当当前数字是负数，即向左移动，栈顶是正数，向右移动时，会发生碰撞。
* 当发生碰撞时，如果栈顶体积大于当前星星，则不操作；若相等，则出栈即可；否则要迭代出栈直到栈顶体积大于等于当前星星或者栈顶移动方向与当前星星一致。

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Stack<Integer> stack = new Stack<>();
        for(int cur : asteroids) {
            // 栈为空、星星反向相同、栈中向左，当前向右，则不发生碰撞，入栈
            if(stack.isEmpty() || stack.peek() * cur > 0 || (stack.peek() < 0 && cur > 0)) {
                stack.push(cur);
            } 
            // 发生碰撞
            else {
                // 栈顶星星小，栈顶爆炸，继续遍历
                while(!stack.isEmpty() && stack.peek() > 0 && stack.peek() < -cur) {
                    stack.pop();
                }
                if(stack.isEmpty() || stack.peek() < 0) {
                    stack.push(cur);
                }
                // 栈顶相等，两个都爆炸
                else if(stack.peek() == -cur) {
                    stack.pop();
                }
                // 栈顶不变，当前星星爆炸
            }
        }
        int[] res =new int[stack.size()];
        for(int i = 0; i < stack.size(); i++) {
            res[i] = stack.get(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：21ms，在所有java提交中击败了28.51%的用户。

内存消耗：39.3MB，在所有java提交中击败了67.69%的用户。

## 官方解题

&emsp;思路相似，官方从右至左遍历。