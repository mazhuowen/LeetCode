[toc]

You are asked to design a file system that allows you to create new paths and associate them with different values.

The format of a path is one or more concatenated strings of the form: `/` followed by one or more lowercase English letters. For example, `"/leetcode"` and `"/leetcode/problems"` are valid paths while an empty string `""` and `"/"` are not.

Implement the FileSystem class:

* `bool createPath(string path, int value)` Creates a new `path` and associates a `value` to it if possible and returns `true`. Returns `false` if the path **already exists** or its parent path **doesn't exist**.
* `int get(string path)` Returns the value associated with `path` or returns `-1` if the path doesn't exist.



**Constraints**:

* The number of calls to the two functions is less than or equal to $10^4$ in total.
* $2 \le \text{path.length} \le 100$
* $1 \le \text{value} \le 10^9$



## 题目解读

&emsp;设计文件系统。

```java
class FileSystem {

    public FileSystem() {

    }
    
    public boolean createPath(String path, int value) {

    }
    
    public int get(String path) {

    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem obj = new FileSystem();
 * boolean param_1 = obj.createPath(path,value);
 * int param_2 = obj.get(path);
 */
```

## 程序设计

* 类似字典树设计文件系统。

```java
class FileSystem {
    File root;

    public FileSystem() {
        this.root = new File("/", -1);
    }
    
    public boolean createPath(String path, int value) {
        String[] paths = path.split("/");
        File parent = find(paths, paths.length - 1);
        if (parent == null || parent.children.containsKey(paths[paths.length - 1])) return false;
        parent.children.put(paths[paths.length - 1], new File(paths[paths.length - 1], value));
        return true;
    }
    
    public int get(String path) {
        String[] paths = path.split("/");
        File target = find(paths, paths.length);
        if (target == null) return -1;
        return target.value;
    }

    private File find(String[] paths, int end) {
        File tmp = this.root;
        for (int i = 1; i < end; i++) {
            if (tmp.children.get(paths[i]) == null) return null;
            tmp = tmp.children.get(paths[i]);
        }
        return tmp;
    }
}

class File {
    String name;
    int value;
    Map<String, File> children;

    File(String name, int value) {
        this.name = name;
        this.value = value;
        this.children = new HashMap<>();
    }
}
```

## 性能分析

&emsp;创建文件路径得时间复杂度为$O(N)$，查找的时间复杂度为$O(N)$，空间复杂度为$O(M)$。

执行用时：132ms，在所有java提交中击败了33.93%的用户。

内存消耗：48.9MB，在所有java提交中击败了8.33%的用户。

## 官方解题

&emsp;官方思路类似，社区则直接保存路径到哈希表，时间性能更快。

```java
class FileSystem {
    private Map<String, Integer> fileSystem;
    
    public FileSystem() {
        fileSystem  =new HashMap<>();
        // 根路径
        fileSystem.put("", -1);
    }

    public boolean createPath(String path, int value) {
        if (fileSystem.containsKey(path)) return false;

        int lastIndex = path.lastIndexOf("/");
        String parentPath = path.substring(0, lastIndex);
        // 父路径不存在
        if (!fileSystem.containsKey(parentPath)) return false;

        fileSystem.put(path,value);
        return true;
    }

    public int get(String path) {
        return fileSystem.getOrDefault(path,-1);
    }
}
```

