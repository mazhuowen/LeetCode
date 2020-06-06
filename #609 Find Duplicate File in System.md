[toc]

Given a list of directory info including directory path, and all the files with contents in this directory, you need to find out all the groups of duplicate files in the file system in terms of their paths.

A group of duplicate files consists of at least **two** files that have exactly the same content.

A single directory info string in the **input** list has the following format:

```
"root/d1/d2/.../dm f1.txt(f1_content) f2.txt(f2_content) ... fn.txt(fn_content)"
```

It means there are $n$ files (`f1.txt`, `f2.txt` ... `fn.txt` with content `f1_content`, `f2_content` ... `fn_content`, respectively) in directory `root/d1/d2/.../dm`. Note that $n \ge 1$ and $m \ge 0$. If $m = 0$, it means the directory is just the root directory.

The **output** is a list of group of duplicate file paths. For each group, it contains all the file paths of the files that have the same content. A file path is a string that has the following format:

```
"directory_path/file_name.txt"
```



**Note**:

* No order is required for the final output.
* You may assume the directory name, file name and file content only has letters and digits, and the length of file content is in the range of `[1,50]`.
* The number of files given is in the range of `[1,20000]`.
* You may assume no files or directories share the same name in the same directory.
* You may assume each given directory info represents a unique directory. Directory path and file info are separated by a single blank space.



**Follow-up beyond contest**:

* Imagine you are given a real file system, how will you search files? DFS or BFS?
* If the file content is very large (GB level), how will you modify your solution?
* If you can only read the file by 1kb each time, how will you modify your solution?
* What is the time complexity of your modified solution? What is the most time-consuming part and memory consuming part of it? How to optimize?
* How to make sure the duplicated files you find are not false positive?



## 题目解读

&emsp;

```java
class Solution {
    public List<List<String>> findDuplicate(String[] paths) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public List<List<String>> findDuplicate(String[] paths) {
        List<List<String>> res = new LinkedList<>();
        if (paths == null || paths.length == 0) return res;

        // 记录文件内容及文件路径
        Map<String, List<String>> record = new HashMap<>();
        for (String path : paths) {
            String[][] files = transfer(path);
            for (String[] file : files) {
                String dir = file[0], content = file[1];

                if (record.get(content) == null) record.put(content, new LinkedList<>());
                record.get(content).add(dir);
            }
        }

        for (Map.Entry<String, List<String>> entry : record.entrySet()) {
            // 不重复，则跳过
            if (entry.getValue().size() <= 1) continue;
            res.add(entry.getValue());
        }

        return res;
    }

    // 拼接当前目录下的文件内容和目录
    private String[][] transfer(String path) {
        if (path == null || path.isEmpty()) return new String[0][2];

        String[] split = path.split(" ");
        // 目录
        String parent = split[0];
        String[][] res = new String[split.length - 1][2];
        for (int i = 1; i < split.length; i++) {
            String str = split[i];
            if (str == null || str.isEmpty()) continue;
            // 分割文件名及内容
            String[] file =str.split("[()]");
            if (file.length != 2) throw new IllegalArgumentException("invalid param");
            res[i - 1] = new String[]{parent + "/" + file[0], file[1]};
        }
        return res;
    }
}
```

## 性能分析

&emsp;

执行用时 :71 ms, 在所有 Java 提交中击败了13.35%的用户

内存消耗 :48.1 MB, 在所有 Java 提交中击败了100.00%的用户

## 官方解题

&emsp;