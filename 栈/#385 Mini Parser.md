[toc]

Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Note: You may assume that the string is well-formed:

* String is non-empty.
* String does not contain white spaces.
* String contains only digits `0-9`, `[` ,`-`,`,`, `]`.



## 题目解读

&emsp;将字符串形式的对象反序列化为NestedInteger对象，一个NestedInteger对象要么只有一个整数属性，要么只有一个NestedInteger列表属性。整数形采用`setInteger(int val)`、`NestedInteger(int cal)`赋值；链表形采用`add(NestedInteger ni)`添加。

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public NestedInteger deserialize(String s) {
        
    }
}
```

## 程序设计

* 对于整数形NestedInteger，序列化格式为`val`，对于列表形NestedInteger，序列化格式为`[...]`，列表元素可以是整数NestedInteger，也可以是NestedInteger列表，数量不限。
* 对于列表形NestedInteger，遇到开括号，就创建空NestedInteger并入栈；向后遍历个元素，遇到列表NestedInteger则同上；遇到整数NestedInteger则创建整数对象，并调用`add`方法加入栈顶对象；列表NestedInteger遇到闭括号表示该对象的链表元素都已经`add`完成，该对象已创建完，可以弹出栈调用`add`方法加入新的栈顶对象。依次类推，直到遍历完栈中只剩一个对象，就是需要反序列化的对象。
* 以`[1,[[3],[4,5],6]]`为例，遍历到第一个开括号表示是一个列表NestedInteger对象，创建入栈；继续遍历，遍历到1表示是一个数字NestedInteger，创建并调用`add`加入栈顶对象；继续遍历，发现是一个开括号，创建NestedInteger并入栈；第三个开括号同理；遍历到3，创建NestedInteger并`add`加入栈顶对象；继续遍历发现闭括号，表示`[3]`这个NestedInteger对象已经创建完成，弹出栈`add`入新的栈顶对象，也就是1后面的这个同级列表NestedInteger；继续遍历，同理得到`[4,5]`、6，`add`入1后的列表NestedInteger；继续遍历，遇到闭括号，表示`[[3],[4,5],6]`已经创建完成，弹出栈加入新的栈顶对象；最后遇到闭括号表示整个对象创建完成。
* 需注意整数符号的判断。

```java
class Solution {
    public NestedInteger deserialize(String s) {
        char[] chars = s.toCharArray();
        // 保存NestedInteger对象
        Stack<NestedInteger> stack = new Stack<>();
        int index = 0;
        // 表示当前数字是否是负数
        boolean flag = false;
        // 遍历字符串
        while(index < s.length()) {
            NestedInteger temp;
            switch(chars[index] ) {
                // 开括号表示创建嵌套NestedInteger对象，入栈
                case '[':
                    temp = new NestedInteger();
                    stack.push(temp);
                    index++;
                    break;
                // 嵌套对象出栈并add入新的栈顶嵌套对象（如果有的话）
                case ']':
                    if(stack.size() > 1) {
                        temp = stack.pop();
                        stack.peek().add(temp);
                    }
                    index++;
                    break;
                // 表示嵌套列表的同级元素，继续向后遍历
                case ',':
                    index++;
                    break;
                // 符号
                case '-':
                    flag = true;
                    index++;
                    break;
                // 数字，表示是一个数字对象，入栈
                default :
                    int val = 0;
                    // 遍历数字
                    while(index < s.length() && Character.isDigit(chars[index])) {
                        val = val * 10 + (chars[index] - '0');
                        index++;
                    }
                    if(flag){
                        val = -val;
                        flag = false;
                    }
                    temp = new NestedInteger(val);
                    if(stack.isEmpty()) {
                        stack.push(temp);
                    } 
                    // 栈不为空表示是嵌套对象，加入
                    else {
                        stack.peek().add(temp);
                    }
                    break;
            }
         
        }
        // 弹出栈顶对象（也是唯一的对象）
        return stack.pop();
    }
}
```

测试样例：整数对象如`365`；一个元素的带符号的嵌套对象`[-1]`；复杂嵌套对象`[2,[4,[4,2],9]]`。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：8ms，在所有java提交中击败了62.93%的用户。

内存消耗：37.3MB，在所有java提交中击败了89.40%的用户。

## 官方解题

&emsp;暂无，密切关注。