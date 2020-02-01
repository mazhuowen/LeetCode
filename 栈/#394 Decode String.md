[toc]

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like `3a` or `2[4]`.



## 题目解读

&emsp;对编码的字符串解码，例如`3[a2[c]]`返回`accaccacc`，其中`k[str]`表示将`str`重复`k`次。题目规定输入是有效的格式。

```java
class Solution {
    public String decodeString(String s) {
        
    }
}
```

## 程序设计

* 首先分析编码公式，可以发现有效的编码`[`前面必然是数字，`]`必然有对应的`[`；字符前面要么是字符要么是开括号，字符后面要么是字符要么是闭括号或数字。
* 从左至右遍历字符串，遇到开括号入栈，遇到闭括号需要将栈顶的字符串做复制运算，最终弹出开括号，将计算出的字符串与栈顶合并会入栈（根据栈顶元素是字符串还是开括号或空栈）；遇到数字或字母都需要遍历找到整个连续的数字或字母，入栈；字母还需要判断是否需要合并。

```java
class Solution {
    public String decodeString(String s) {
        // 符合要求的编码公式至少是4位
        if(s == null || s.length() < 4) {
            return s;
        }
        char[] chars = s.toCharArray();
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < s.length(); i++) {
            switch(chars[i]) {
                // 子字符串开始标识
                case '[':
                    stack.push("[");
                    break;
                // 子字符串结束标识，出栈并做复制（开括号必然对应倍数）
                case ']':
                    String str = ""; 
                    String childStr = stack.pop();
                    // 删除开括号
                    stack.pop();
                    // 运算
                    for(int j = Integer.valueOf(stack.pop()); j > 0 ; j--) {
                        str += childStr;
                    }
                    // 栈为空或栈顶是开括号，则直接压栈
                    if(stack.isEmpty() || "[".equals(stack.peek())) {
                        stack.push(str);
                    } 
                    // 与前面的字符串合并
                    else {
                        stack.push(stack.pop() + str);
                    }
                    break;
                default :
                    // 数字
                    if(Character.isDigit(chars[i])) {
                        int val = 0;
                        while(Character.isDigit(chars[i]) && i < s.length()) {
                            val = val * 10 + (chars[i++] - '0');
                        }
                        stack.push(val + "");
                        // 针对for循环结束的i++。由于数字肯定不在字符串最后，所以不必判断是否遍历到字符串结尾。
                        i --;
                    } 
                    // 字母
                    else {
                        String charStr = "";
                        while(i < s.length() && Character.isLetter(chars[i])) {
                            charStr += chars[i++];
                        }
                        if(stack.isEmpty() || "[".equals(stack.peek())) {
                            stack.push(charStr);
                        } else {
                            stack.push(stack.pop() + charStr);
                        }
                        // 继续遍历
                        if(i != s.length()) {
                            i--;
                        }
                    }
                    break;
            }
        }
        return stack.pop();
    }
}
```

测试样例：空字符串、全字母字符串测试特殊情况；其它测试正常功能。

## 性能设计

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：2ms，在所有java提交中击败了33.56%的用户。

内存消耗：36MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;暂无，密切关注。