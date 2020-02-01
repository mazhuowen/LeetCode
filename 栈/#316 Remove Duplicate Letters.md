[toc]

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appears once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.



## 题目解读

&emsp;要求去除给定小写字母序列中的重复元素，且最后的顺序是在不打乱原来的相对位置的情况下的字典序。

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        
    }
}
```

## 程序设计

* 分析题目给出的例子，可知重点在于原本不重复的字母，它们的相对位置是不变的；重复的字母需要以升序的形式在不重复的字母框架下。一种可行的思路是遍历字符串，栈中保存的就是当前字符前的不重复且字典序的字符串；如果这个字符串包含当前字符，跳过；不包含且当前字符大于栈顶字符，即是升序的，则加入栈；否则不包含当前字符，且当前字符小于栈顶字符，则从栈顶依次判断栈中元素后续是否还有，有则出栈，让这个小的字符排列到前面，无则小的字符入栈，因为栈字符虽然比当前字符大，但是不重复，位置是固定的，且这个固定字符前的字符即使后面有重复的也不会再操作，因为这使得固定的不重复字符的字典序最大。
* 分析完后可知我们需要一个数组记录26个字母的最后的位置，需要栈来保存字典序最大的序列。

<img src="/project/LeetCode/images/#316.png"  />

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        if(s == null || s.length() <= 1) {
            return s;
        }
        char[] letters = s.toCharArray();
        // 记录字母最后的位置
        int[] lastIndex = new int[26];
        for(int i = 0; i < s.length(); i++) {
            lastIndex[letters[i] - 'a'] = i;
        }

        Stack<Character> stack = new Stack<>();
        for(int i = 0; i < s.length(); i++) {
            // 已包含
            if(stack.contains(letters[i])) {
                continue;
            }
            // 栈空或当前字母未包含且升序，加入
            if(stack.isEmpty() || stack.peek() < letters[i]) {
                stack.push(s.charAt(i));
                continue;
            }
            // 当前字母小于栈顶字母，栈中去除后面还会出现的直到碰到后面不会出现的
            // 比如acbac中假设当前为b，栈中为ac，有c大于b且后面还会出现，出栈，a小于b停止，b入栈。
            // 比如cbacdcbc假设当前为第二个b，此时栈中为acd，d比b大，但d出现1次，故停止，b入栈
            while(!stack.isEmpty() && stack.peek() > letters[i] && lastIndex[stack.peek() - 'a'] > i) {
                stack.pop();
            }
            stack.push(letters[i]);
        }
        StringBuffer sb = new StringBuffer();
        while(!stack.isEmpty()) {
            sb.insert(0, stack.pop());
        }
        return sb.toString();
    }
}
```

测试样例：输入`bcabc`输出`abc`测试重复字母的位置；`acbac`输出`abc`测试字母小于栈顶时的出栈判断；`cbacdcbc`测试综合功能。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：5ms，在所有java提交中击败了72.35%的用户。

内存消耗：36.8MB，在所有java提交中击败了13.48%的用户。

## 官方解题

&emsp;暂无，密切关注。