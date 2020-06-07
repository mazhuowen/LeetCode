[toc]

Suppose we abstract our file system by a string in the following manner:

The string `"dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"` represents:

```
dir
    subdir1
    subdir2
        file.ext
```

The directory `dir` contains an empty sub-directory `subdir1` and a sub-directory `subdir2` containing a file `file.ext`.

The string `"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"` represents:

```
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
```

The directory `dir` contains two sub-directories `subdir1` and `subdir2`. `subdir1` contains a file `file1.ext` and an empty second-level sub-directory `subsubdir1`. `subdir2` contains a second-level sub-directory `subsubdir2` containing a file `file2.ext`.

We are interested in finding the longest (number of characters) absolute path to a file within our file system. For example, in the second example above, the longest absolute path is `"dir/subdir2/subsubdir2/file2.ext"`, and its length is 32 (not including the double quotes).

Given a string representing the file system in the above format, return the length of the longest absolute path to file in the abstracted file system. If there is no file in the system, return 0.

Note:

* The name of a file contains at least a `.` and an extension.
* The name of a directory or sub-directory will not contain a `..`

Time complexity required: $O(n)$ where n is the size of he input string.

Notice that `a/aa/aaa/file1.txt` is not the longest file path, if there is another path `aaaaaaaaaaaaaaaaaaaaa/sth.png`.



## 题目解读

&emsp;给定文件系统所有路径，返回路径字符串的最长长度。

```java
class Solution {
    public int lengthLongestPath(String input) {

    }
}
```

## 程序设计

* 最基本的，文件系统返回的是树的前序遍历，每个文件前有几个`\t`就是第几层。

```java
class Solution {
    public int lengthLongestPath(String input) {
        if (input == null || input.isEmpty()) return 0;
        // 路径最长长度，当前长度，遍历文件层数
        int maxLen = 0;
        int[] lens = new int[(input.length() + 1) / 2];
        String[] paths = input.split("\n");
        for (String path : paths) {
            int level = getFileLevel(path);
            // 当前路径长度
            lens[level] = path.length() - level + (level == 0 ? 0 : lens[level - 1]);
            // 文件，统计路径长度
            if (path.indexOf(".") != -1) {
                // 拼接路径需要level个/号
                maxLen = Math.max(maxLen, lens[level] + level);
            }
        }
        return maxLen;
    }

    private int getFileLevel(String path) {
        // \t为一个字符，有几个\t就是第几级，没有则是第0层
        return path.lastIndexOf('\t') + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;暂无，密切关注。