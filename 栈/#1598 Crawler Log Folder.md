[toc]

The Leetcode file system keeps a log each time some user performs a change folder operation.

The operations are described below:

* `"../"` : Move to the parent folder of the current folder. (If you are already in the main folder, **remain in the same folder**).
* `"./"` : Remain in the same folder.
* `"x/"` : Move to the child folder named `x` (This folder is **guaranteed to always exist**).

You are given a list of strings `logs` where `logs[i]` is the operation performed by the user at the `i`th step.

The file system starts in the main folder, then the operations in `logs` are performed.

Return the minimum number of operations needed to go back to the main folder after the change folder operations.

 

**Constraints**:

* $1 \le \text{logs.length} \le 10^3$
* $2 \le \text{logs[i].length} \le 10$
* `logs[i]` contains lowercase English letters, digits, `'.'`, and `'/'`.
* `logs[i]` follows the format described in the statement.
* Folder names consist of lowercase English letters and digits.



## 题目解读

&emsp;给定文档系统路径操作顺序，求最后返回主目录的步数。

```java
class Solution {
    public int minOperations(String[] logs) {

    }
}
```

## 程序设计

* 遍历记录当前操作所在文件系统层级即可。

```java
class Solution {
    public int minOperations(String[] logs) {
        int level = 0;
        for (String log : logs) {
            // 返回上级目录
            if ("../".equals(log)) {
                if (level > 0) level--;
            }
            // 进入下层目录，层数加一
            else if (!"./".equals(log)) level++;
            // 保持当前目录，层数不变
        }
        return level;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.73%的用户。

内存消耗：38.7MB，在所有java提交中击败了27.95%的用户。

## 官方解题

&emsp;暂无，密切关注。