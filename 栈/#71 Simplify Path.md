[toc]

Given an **absolute path** for a file (Unix-style), simplify it. Or in other words, convert it to the **canonical path**.

In a UNIX-style file system, a period . refers to the current directory. Furthermore, a double period .. moves the directory up a level. For more information, see: Absolute path vs relative path in Linux/Unix

Note that the returned canonical path must always begin with a slash /, and there must be only a single slash / between two directory names. The last directory name (if it exists) **must not** end with a trailing /. Also, the canonical path must be the **shortest** string representing the absolute path.



## 题目解读

&emsp;给定Linux系统的文件绝对路径，将其化简。题目中提到`.`表示当前目录，`..`表示上级目录。

```java
class Solution {
    public String simplifyPath(String path) {
        
    }
}
```

## 程序设计

* 将路径以`/`拆分为数组，从左至右遍历入栈，当遇到`.`时，丢弃；当遇到`..`时，弹出栈顶元素。需注意根目录`/`的上级目录仍然是根目录。

```java
class Solution {
    public String simplifyPath(String path) {
        String[] paths = path.split("/");
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < paths.length; i++) {
            // 弹出
            if(!stack.isEmpty() && "..".equals(paths[i])) {
                stack.pop();
            } 
            // 对不是当前目录、上级目录、\符号的值压栈
            else if(!".".equals(paths[i]) && !"..".equals(paths[i]) && !"".equals(paths[i])) {
                stack.push(paths[i]);
            }
        }
        
        if(stack.size() == 0) {
            return "/";
        }
        // 拼接
        StringBuffer sb = new StringBuffer();
        for(int i = 0; i < stack.size(); i++) {
            sb.append("/" + stack.get(i));
        }
        return sb.toString();
    }
}
```

测试样例：$/../$输出$/$；$/\text{home}/$输出$/\text{home}$。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：6ms，在所有java提交中击败了77.76%的用户。

内存消耗：36.5MB，在所有java提交中击败了65.16%的用户。

## 官方解题

&emsp;暂无，密切关注。