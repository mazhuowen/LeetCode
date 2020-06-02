[toc]

Design an in-memory file system to simulate the following functions:

* `ls`: Given a path in string format. If it is a file path, return a list that only contains this file's name. If it is a directory path, return the list of file and directory names **in this directory**. Your output (file and directory names together) should in **lexicographic order**.

* `mkdir`: Given a **directory path** that does not exist, you should make a new directory according to the path. If the middle directories in the path don't exist either, you should create them as well. This function has void return type.

* `addContentToFile`: Given a **file path** and **file content** in string format. If the file doesn't exist, you need to create that file containing given content. If the file already exists, you need to **append** given content to original content. This function has void return type.

* `readContentFromFile`: Given a **file path**, return its **content** in string format.



Note:

* You can assume all file or directory paths are absolute paths which begin with `/` and do not end with `/` except that the path is just `"/"`.
* You can assume that all operations will be passed valid parameters and users will not attempt to retrieve file content or list a directory or file that does not exist.
* You can assume that all directory names and file names only contain lower-case letters, and same names won't exist in the same directory.



## 题目解读

&emsp;设计文件系统。

```java
class FileSystem {

    public FileSystem() {

    }
    
    public List<String> ls(String path) {

    }
    
    public void mkdir(String path) {

    }
    
    public void addContentToFile(String filePath, String content) {

    }
    
    public String readContentFromFile(String filePath) {

    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem obj = new FileSystem();
 * List<String> param_1 = obj.ls(path);
 * obj.mkdir(path);
 * obj.addContentToFile(filePath,content);
 * String param_4 = obj.readContentFromFile(filePath);
 */
```

## 程序设计

* 文件系统实质就是前缀匹配的过程，使用字典树实现。

```java
class FileSystem {
    // 根目录
    File root;

    public FileSystem() {
        this.root = new File("/", true);
    }

    public List<String> ls(String path) {
        List<String> res = new ArrayList<>();

        String[] paths = checkPath(path);
        File file = findFile(paths);
        if (file == null) throw new IllegalArgumentException("no file");

        // 目录则输出子结构
        if (file.isDir) {
            for (Map.Entry<String, File> entry : file.files.entrySet()) {
                res.add(entry.getKey());
            }
            res.sort((a, b) -> a.compareTo(b));
        }
        // 文件则打印名称
        else {
            res.add(file.name);
        }
        return res;
    }

    public void mkdir(String path) {
        // 根目录已存在
        if (path.equals("/")) return;
        String[] paths = checkPath(path);
        // 添加目录
        addFile(paths, true);
    }

    public void addContentToFile(String filePath, String content) {
        String[] paths = checkPath(filePath);
        File file = findFile(paths);
        if (file == null) file = addFile(paths, false);
        // 添加文本
        file.appendContent(content);
    }

    public String readContentFromFile(String filePath) {
        String[] paths = checkPath(filePath);
        File file = findFile(paths);
        if (file == null || file.isDir) throw new IllegalArgumentException("not a file");
        
        // 读取文本
        return file.getContent();
    }

    // 路径校验处理
    private String[] checkPath(String path) {
        if (path == null || path.length() == 0) throw new IllegalArgumentException("invalid path");
        String[] paths = path.split("/");

        // 校验路径，起始必须为/
        if (!path.startsWith("/")) throw new IllegalArgumentException("not a absolute path");

        // 路径中不能存在连续的/
        for (int i = 1; i < paths.length; i++) {
            if (paths[i].isEmpty()) throw new IllegalArgumentException("invalid path");
        }

        return paths;
    }

    // 字典树搜索
    private File findFile(String[] paths) {
        File temp = root;
        for (int i = 1; i < paths.length; i++) {
            // 路径不存在
            if (!temp.files.containsKey(paths[i])) return null;
            temp = temp.files.get(paths[i]);
        }
        return temp;
    }

    // 字典树添加
    private File addFile(String[] paths, boolean isDir) {
        File temp = root;
        for (int i = 1; i < paths.length; i++) {
            String curPath = paths[i];
            if (!temp.files.containsKey(curPath)) {
                if (i < paths.length - 1) temp.files.put(curPath, new File(curPath, true));
                    // 最后为文件
                else temp.files.put(curPath, new File(curPath, isDir));
            }
            temp = temp.files.get(curPath);
        }
        // 输入路径存在文件或目录与当前类型不符，无法创建文件
        if (temp.isDir != isDir) throw new IllegalArgumentException("already exit a dir or file in the path");
        return temp;
    }
}


class File {
    // 目录或者文件
    boolean isDir;
    // 文件或目录名
    String name;

    // 目录下文件列表
    Map<String, File> files;

    // 文件内容
    private StringBuffer contents;

    public File(String name, boolean isDir) {
        this.isDir = isDir;
        this.name = name;
        if (isDir) this.files = new HashMap<>();
        else this.contents = new StringBuffer();
    }

    // 文本操作
    public void appendContent(String content) {
        contents.append(content);
    }

    public String getContent() {
        return contents.toString();
    }
}
```

## 性能分析

&emsp;单次查询、插入时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：54ms，在所有java提交中击败了60.29%的用户。

内存消耗：40.9MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;官方思路类似。