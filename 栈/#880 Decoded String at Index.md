[toc]

An encoded string `S` is given.  To find and write the `decoded` string to a tape, the encoded string is read **one character at a time** and the following steps are taken:

* If the character read is a letter, that letter is written onto the tape.
* If the character read is a digit (say `d`), the entire current tape is repeatedly written `d-1` more times in total.

Now for some encoded string `S`, and an index `K`, find and return the `K`-th letter (1 indexed) in the decoded string.



Note:

* $2 \le \text{S.length} \le 100$
* `S` will only contain lowercase letters and digits `2` through `9`.
* S` starts with a letter.
* $1 \le K \le 10^9$
* The decoded string is guaranteed to have less than $2^63$ letters.



## 题目解读

&emsp;给定字符串只包含小写字母和个位数整数；整数表示将前面的字符串重复整数次。需要得出字符串第`K`个字母。

```java
class Solution {
    public String decodeAtIndex(String S, int K) {
        
    }
}
```

## 程序设计

* 从左至右遍历字符串，遇到字符串进栈，遇到数字遍历其后是否是数字，是则相乘，最后将栈中字符串重复；重复上述操作，直到遍历结束。但是该方法会超时，不满足要求。

```java
class Solution {
    public String decodeAtIndex(String S, int K) {
        Stack<String> stack = new Stack<>();
        int index = 0;
        while(index < S.length()) {
            char c = S.charAt(index);
            // 是字母，则遍历得到字符串
            if(Character.isLetter(c)) {
                // 拼接连续字符
                StringBuffer sb = new StringBuffer();
                while(index < S.length() && Character.isLetter(S.charAt(index))) {
                    sb.append(S.charAt(index++));
                }
                // 栈中存在字符串，拼接
                if(!stack.isEmpty()) {
                    stack.push(stack.pop() + sb.toString());
                } else {
                    stack.push(sb.toString());
                }
            } 
            // 数字
            else {
                // 相乘连续数字
                int val = S.charAt(index) - '0';
                while(++index < S.length() && Character.isDigit(S.charAt(index))) {
                    val *= S.charAt(index) - '0';
                }
                System.out.println(val);
                // 重复字符串
                String cur = stack.pop();
                cur = cur.repeat(val);
                // 入栈
                stack.push(cur);
            }
        }
        String res =  stack.pop();
        // 返回第K个
        return K > res.length() ? "" : "" + res.charAt(K - 1);
    }
}
```

* 优化上述代码，每次拼接都判断字符串长度是否大于`K`，并返回结果。但是仍旧超时。

```java
class Solution {
    public String decodeAtIndex(String S, int K) {
        Stack<String> stack = new Stack<>();
        int index = 0;
        while(index < S.length()) {
            char c = S.charAt(index);
            String cur;
            // 是字母，则遍历得到字符串
            if(Character.isLetter(c)) {
                // 拼接栈中的字符串
                StringBuffer sb = new StringBuffer();
                if(!stack.isEmpty()) {
                    sb.append(stack.pop());
                }
                // 拼接连续字符
                while(index < S.length() && Character.isLetter(S.charAt(index))) {
                    sb.append(S.charAt(index++));
                }
                cur = sb.toString();
            } 
            // 数字
            else {
                // 相乘连续数字
                int val = S.charAt(index++) - '0'; 
                // 重复字符串
                cur = stack.pop().repeat(val);
            }
            // 当前字符串长度以满足判断要求，返回结果
            if(cur.length() >= K) {
                System.out.println(cur);
                return cur.charAt(K - 1) + "";
            }
            // 入栈
            stack.push(cur);
        }
        String res =  stack.pop();
        // 返回第K个
        return K > res.length() ? "" : "" + res.charAt(K - 1);
    }
}
```

* 转换思路，参考官方解题，只统计出解码后的字符串长度，而不是字符串；然后从右向左遍历，对于一般的字符串，自然是比较`K`与当前遍历字符的长度`size`比较，相同则返回，不同则继续向前遍历；应用到本题中，由于字符串有整数个重复，判断条件变为`k%size`是否为0。

```java
class Solution {
    public String decodeAtIndex(String S, int K) {
        int len = S.length();
        // 长整数
        long size = 0L;
        // 统计解码后的长度，字符加一，数字则相乘
        for(char c : S.toCharArray()) {
            if(Character.isLetter(c)) {
                size++;
            } else {
                size *= c - '0';
            }
        }
        if(size < K) {
            return "";
        }
        // 反序遍历
        for(int i = len - 1; i >= 0; i--) {
            char c = S.charAt(i);
            K %= size;
            // 找到并返回
            if(K == 0 && Character.isLetter(c)) {
                return Character.toString(c);
            }
            // 向前遍历，长度减少
            if(Character.isLetter(c)) {
                size--;
            } else {
                size /= c - '0';
            }
        }
        // 一般不会走这里
        return "";
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.7MB，在所有java提交中击败了6.06%的用户。

## 官方解题

&emsp;官方思路见上。