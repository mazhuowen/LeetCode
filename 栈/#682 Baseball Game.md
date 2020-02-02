[toc]

You're now a baseball game point recorder.

Given a list of strings, each string can be one of the 4 following types:

* Integer (one round's score): Directly represents the number of points you get in this round.
* "+" (one round's score): Represents that the points you get in this round are the sum of the last two valid round's points.
* "D" (one round's score): Represents that the points you get in this round are the doubled data of the last valid round's points.
* "C" (an operation, which isn't a round's score): Represents the last valid round's points you get were invalid and should be removed.

Each round's operation is permanent and could have an impact on the round before and the round after.

You need to return the sum of the points you could get in all the rounds.

Note:
* The size of the input list will be between 1 and 1000.
* Every integer represented in the list will be between -30000 and 30000.



## 题目解读

&emsp;给定一个字符串列表，其中整数表示本轮得分，`+`表示本轮得分是前两轮有效得分之和，`D`表示本轮得分是前一轮得分的两倍，`C`表示前一轮得分无效，需要移除。根据这一系列规则，计算最后的得分。

```java
class Solution {
    public int calPoints(String[] ops) {
        
    }
}
```

## 程序设计

* 从左至右遍历数组，遇到数字在总分中加上即可，并入栈，遇到`C`则在总分中减去栈顶的分数，并出栈，遇到`D`、`C`做相应计算，将计算出的本轮分数入栈，并加入总分。

```java
class Solution {
    public int calPoints(String[] ops) {
        int points = 0;
        Stack<Integer> stack = new Stack<>();
        for(String cur : ops) {
            switch(cur) {
                // 加栈顶部的两个数值并入栈
                case "+":
                    int num1 = stack.pop();
                    int num2 = num1 + stack.peek();
                    stack.push(num1);
                    stack.push(num2);
                    points += num2;
                    break;
                // 栈顶数值翻倍并入栈
                case "D":
                    stack.push(2 * stack.peek());
                    points += stack.peek();
                    break;
                // 删除栈顶，并减去分数
                case "C":
                    points -= stack.pop();
                    break;
                // 数字直接入栈
                default:
                    stack.push(Integer.valueOf(cur));
                    points += stack.peek();
                    break;
            }
        }
        return points;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：3ms，在所有java提交中击败了83.79%的用户。

内存消耗：35.9MB，在所有java提交中击败了57.61%的用户。

## 官方解题

&emsp;思路同上。