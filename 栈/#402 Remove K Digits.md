[toc]

Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.



**Note**:

* The length of num is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.



## 题目解读

&emsp;给定非负整数的字符串形式，去除$k$位，使得剩余的字符串表示的整数最小，不能改动数字之间的相对顺序。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        
    }
}
```

## 程序设计

* 分析`1432219`，$k=3$，也就是说删除三个数字后剩余四个数字，剩余的数字相对位置不变，高位仍然是高位。假设开始第一次选择，由于需要剩余4位，目前在选择第一位，可知第一位的选择区间`219`之前的子字符串，这样最极端的情况，高位最小值在区间末尾，也能保证最后的结果是4位；公式化的，对于要删除$k$的字符串，第一次选择区间为`0～k`的闭区间，即在前$k + 1$个元素中选择，由于数组索引缘故就是`0~k`；在得到第一个区间最小值后，这个值就是最优的高位，假设索引为`minIndex`，在`0~k`的闭区间内，则第二次选择的区间为`minIndex+1~k+1`；依次类推，直到选择完需要的位数。
* 特别需要注意高位全部是$0$的情况，需要拼接字符串时特殊处理。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        // 特殊情况判断
        if(k == 0) {
            return num;
        } else if(k == num.length()) {
            return "0";
        }

        char[] chars = num.toCharArray();       
        StringBuffer sb = new StringBuffer();
        // 最终字符串的长度
        int len = num.length() - k;
        // 检查窗口，在这个区间选择一个最小的值。
        int start = 0, end = k;
        // 窗口内最小值的索引
        int minIndex;
        while(len > 0) {
            minIndex = start;
            while(start <= end) {
                if(chars[start] < chars[minIndex]) {
                    // 记录最小值
                    minIndex = start;
                }
                start++;
            }
            // 拼接最小值到字符串，如果高位是0则不添加
            if(sb.length() != 0 || chars[minIndex] != '0') {
                sb.append(chars[minIndex]);
            }
            // 下一轮遍历，开始位置为选中数值的后面一位
            start = minIndex + 1;
            // 结束位置为上一轮的后一位
            end++;
            // 需要选择的位数减一
            len--;
        }
        // 对应高位全部是0的情况，即最后结果就是0
        if(sb.length() == 0) {
            return "0";
        }
        return sb.toString();
    }
}
```

测试样例：顺序字符串如`112`，$k=1$，测试窗口选择机制；`10200`，$k=1$测试高位为0的显示处理；`1432219`，$k=3$测试常规功能。

## 性能分析

&emsp;整体流程最外层循环$N - k$次，内层最好情况循环1次，最坏$k + 1$次，总的时间复杂度为$O(kN)$。由于维护了字符串方便拼接，空间复杂度为$O(N)$。

执行用时：10ms，在所有Java提交中击败了60.06%的用户。

内存消耗：36MB，在所有Java提交中击败了89.15%的用户。

## 官方解题

&emsp;使用栈来实现，遍历字符串入栈，当前值小于栈顶，则出栈删除，相应的$k$值减少，直到$k = 0$。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        if(k == 0) {
            return num;
        }
        if(k == num.length()) {
            return "0";
        }
        Stack<Character> stack = new Stack<>();
        // 遍历
        for(char cur : num.toCharArray()) {
            // 栈顶大于当前值，出栈删除，k相应减少
            while(!stack.isEmpty() && stack.peek() > cur && k > 0) {
                stack.pop();
                k--;
            }
            // 入栈当前值（高位是0则不如栈）
            if(!stack.isEmpty() || cur != '0') {
                stack.push(cur);
            }
        }
        // 为删除完，此时栈中是升序，出栈删除后面的值，直到k=0
        while(k > 0) {
            stack.pop();
            k--;
        }
        StringBuffer sb = new StringBuffer();
        while(!stack.isEmpty()) {
            sb.insert(0, stack.pop());
        }
        // 高位全部是0，则字符串为空
        return "".equals(sb.toString()) ? "0" : sb.toString();
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：22ms，在所有java提交中击败了37.09%的用户。

内存消耗：37.1MB，在所有java提交中击败了17.19%的用户。

> 社区中用LinkedList或数组实现的栈要稍微快一些，思想都一致。