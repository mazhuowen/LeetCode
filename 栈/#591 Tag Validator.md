[toc]

Given a string representing a code snippet, you need to implement a tag validator to parse the code and return whether it is valid. A code snippet is valid if all the following rules hold:

* The code must be wrapped in a **valid closed tag**. Otherwise, the code is invalid.
* A **closed tag** (not necessarily valid) has exactly the following format : `<TAG_NAME>TAG_CONTENT</TAG_NAME>`. Among them,` <TAG_NAME>` is the start tag, and `</TAG_NAME>` is the end tag. The TAG_NAME in start and end tags should be the same. A closed tag is **valid** if and only if the TAG_NAME and TAG_CONTENT are valid.
* A **valid** `TAG_NAME` only contain **upper-case letters**, and has length in range `[1,9]`. Otherwise, the TAG_NAME is invalid.
* A **valid** `TAG_CONTENT` may contain other **valid closed tags**, **cdata** and any characters (see note1) **EXCEPT** unmatched <, unmatched start and end tag, and unmatched or closed tags with invalid TAG_NAME. Otherwise, the `TAG_CONTENT` is invalid.
* A start tag is unmatched if no end tag exists with the same TAG_NAME, and vice versa. However, you also need to consider the issue of unbalanced when tags are nested.
* A < is unmatched if you cannot find a subsequent >. And when you find a < or </, all the subsequent characters until the next > should be parsed as TAG_NAME (not necessarily valid).
* The cdata has the following format : `<![CDATA[CDATA_CONTENT]]>`. The range of `CDATA_CONTENT` is defined as the characters between `<![CDATA[ and the first subsequent ]]>`.
* `CDATA_CONTENT` may contain **any characters**. The function of cdata is to forbid the validator to parse `CDATA_CONTENT`, so even it has some characters that can be parsed as tag (no matter valid or invalid), you should treat it as **regular characters**.



Note:
For simplicity, you could assume the input code (including the any characters mentioned above) only contain letters, digits, '`<`','`>`','`/`','`!`','`[`','`]`' and '` `'.



## 题目解读

&emsp;判断一段代码是否有效。代码必须被合法的闭标签包围，标签必须由`<..>`开头，`<\...>`结尾，标签中字母都是大写且长度不超过10。被包围的代码可以包含其它开闭标签、cdata、任意字符。cdata格式为`<![CDATA[CDATA_CONTENT]]>`，功能是阻止解析CDATA_CONTENT，即CDATA_CONTENT可以是任意字符。

&emsp;为了简化，题目假设字符为数字、字母、`<`、`>`、`[`、`]`、`!`、`/`、` `。

```java
class Solution {
    public boolean isValid(String code) {
        
    }
}
```

## 程序设计

* 根据题目，一个`<`必须要有对应的`>`，而`>`可以在代码中出现，故遍历顺序只能是从左至右，且以`<`为主；这样问题可以转化为：`<`后是`!`则只能是cdata，否则格式错误；`<`后是`/`则只能是闭标签；`<`后是字母，则只能是开标签。
* 对于cdata，需要检测`<`后紧跟的是不是`![CDATA[`，然后检测其后的`]]>`第一个位置（规则7决定了cdata内容里不许有`]]>`，`]]>`只能是结束标识）。
* 对于开闭标签，检测其内容是否大写字母，长度是否满足条件；为了区分开闭标签，可将`/`入栈，当栈顶是`/`号时标识当前是闭标签，需要对应的开标签，即出栈检测是否匹配；否则是开标签，进栈。
* 需要注意，隐含的一个要求是整段代码必须包在一个开闭标签里，即如果没有遍历结束，栈里是不能为空的；具体的，除了位置0的开标签，其它标签都要检查栈是否为空，为空则不合法。如`<A></A><B></B>`是不合法的。此外若遍历完所有的开闭标签，还有其它字符，也是不合法的，如`<A></A>>>`。

```java
class Solution {
    public boolean isValid(String code) {
        Stack<String> stack = new Stack<>();
        int index = 0;
        while(index < code.length()) {
            // 代码必须被标签包围，而<必须是标签或cdata开始的标识
            // 代码的设计使得下次迭代必然是<的位置
            if(code.charAt(index) == '<') {
                // 不是第一个开标签，但是栈为空，即代码没有被一个总的开闭标签包围
                if(index != 0 && stack.isEmpty()) {
                    return false;
                }
                // <后没有字符，即没有对应的>
                if(index + 1 == code.length()) {
                    return false;
                }
                 // 标签头为<![CDATA[，以后的内容无需检测，需检测结束标签]]>
                if(code.charAt(index + 1) == '!' ) {
                        // 检测<后面是否是cdata开头
                        if(code.indexOf("![CDATA[", index) != index + 1) {
                            return false;
                        } 
                        // 根据第7个规则，只需找到后面的第一个]]>（即cdata内容中不能出现]]>）
                        index = code.indexOf("]]>", index);
                        // 没有对应的闭标志
                        if(index == -1) {
                            return false;
                        }
                        // cdata遍历结束
                        index += 3;
                }
                // 标签检测
                else {
                    // 闭标签
                    if(code.charAt(++index) == '/') {
                        stack.push("/");
                        index++;
                    }
                    String tagName = "";
                    // 遍历可能的标签名
                    while(index < code.length() && code.charAt(index) != '>') {
                        // 标签不是大写字母或长度超出限制
                        if(!Character.isUpperCase(code.charAt(index)) || tagName.length() >= 9) {
                            return false;
                        }
                        // 拼接
                        tagName += code.charAt(index++);
                    }
                    // 遍历到结尾都没找到对应的>
                    if(index == code.length()) {
                        return false;
                    }
                    // <>这种情况是不合法的
                    if(tagName.length() < 1) {
                        return false;
                    }
                    // 栈顶是/，表示是闭标签，需要将开标签出栈
                    if(!stack.isEmpty() && "/".equals(stack.peek())) {
                        // 出栈/
                        stack.pop();
                        if(!stack.isEmpty() && tagName.equals(stack.peek())) {
                            // 开闭标签匹配，出栈
                            stack.pop();
                        } else {
                            // 开闭标签不匹配、没有对应开标签
                            return false;
                        }
                    } 
                    // 开标签入栈
                    else {
                        stack.push(tagName);
                    }
                    // 当前位置是>，移动到下一个位置
                    index++;
                }
                // 遍历无关文本（即代码段）到下一个<号
                // 如果栈空了，结束遍历（由于代码由一个开闭标签包裹，在没遍历完前，栈一定不能为空）
                while(!stack.isEmpty() && index < code.length() && code.charAt(index) != '<') {
                    index++;
                }

            } 
            // 此情况对应：遍历无关代码时，栈已经空了，表示这段代码没有被开闭标签包裹，不符合要求
            else {
                return false;
            }
        }
        // 栈不为空，有开标签，但是遍历结束也没找到闭标签，不符合要求
        return stack.isEmpty();
    }
}
```

测试样例：`<A></A><B></B>`返回`false`，测试统一开闭标签包裹条件；`<![CDATA[...]]>`返回`false`，测试只有类似开标签的cdata的情况；`<A`测试标签括号不匹配的情况；`<A></A>>>`测试遍历结束还有部分字符未被包裹的情况；`<A><B></A></B>`测试标签不对应的情况。

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了59.68%的用户。

内存消耗：35.9MB，在所有java提交中击败了21.15%的用户。

## 官方解题

&emsp;官方解题思路一致，这是不会入栈`/`，因为题目中限定了一个标签中可能含有其它标签，不能嵌套本身，比如`<A><A></A></A>`，这样，就可以不用入栈`/`。

```java
public class Solution {
    Stack < String > stack = new Stack < > ();
    boolean contains_tag = false;
    public boolean isValidTagName(String s, boolean ending) {
        if (s.length() < 1 || s.length() > 9)
            return false;
        for (int i = 0; i < s.length(); i++) {
            if (!Character.isUpperCase(s.charAt(i)))
                return false;
        }
        if (ending) {
            if (!stack.isEmpty() && stack.peek().equals(s))
                stack.pop();
            else
                return false;
        } else {
            contains_tag = true;
            stack.push(s);
        }
        return true;
    }
    public boolean isValidCdata(String s) {
        return s.indexOf("[CDATA[") == 0;
    }
    public boolean isValid(String code) {
        if (code.charAt(0) != '<' || code.charAt(code.length() - 1) != '>')
            return false;
        for (int i = 0; i < code.length(); i++) {
            boolean ending = false;
            int closeindex;
            if(stack.isEmpty() && contains_tag)
                return false;
            if (code.charAt(i) == '<') {
                if (!stack.isEmpty() && code.charAt(i + 1) == '!') {
                    closeindex = code.indexOf("]]>", i + 1);
                    if (closeindex < 0 || !isValidCdata(code.substring(i + 2, closeindex)))
                        return false;
                } else {
                    if (code.charAt(i + 1) == '/') {
                        i++;
                        ending = true;
                    }
                    closeindex = code.indexOf('>', i + 1);
                    if (closeindex < 0 || !isValidTagName(code.substring(i + 1, closeindex), ending))
                        return false;
                }
                i = closeindex;
            }
        }
        return stack.isEmpty() && contains_tag;
    }
}
```