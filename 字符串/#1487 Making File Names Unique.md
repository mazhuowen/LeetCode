[toc]

Given an array of strings `names` of size $n$. You will create $n$ folders in your file system **such that**, at the `ith` minute, you will create a folder with the name `names[i]`.

Since two files **cannot** have the same name, if you enter a folder name which is previously used, the system will have a suffix addition to its name in the form of `(k)`, where, $k$ is the **smallest positive integer** such that the obtained name remains unique.

Return an array of strings of length $n$ where `ans[i]` is the actual name the system will assign to the `ith` folder when you create it.

 

**Example 1**:

```
Input: names = ["pes","fifa","gta","pes(2019)"]
Output: ["pes","fifa","gta","pes(2019)"]
Explanation: Let's see how the file system creates folder names:
"pes" --> not assigned before, remains "pes"
"fifa" --> not assigned before, remains "fifa"
"gta" --> not assigned before, remains "gta"
"pes(2019)" --> not assigned before, remains "pes(2019)"
```

**Example 2**:

```
Input: names = ["gta","gta(1)","gta","avalon"]
Output: ["gta","gta(1)","gta(2)","avalon"]
Explanation: Let's see how the file system creates folder names:
"gta" --> not assigned before, remains "gta"
"gta(1)" --> not assigned before, remains "gta(1)"
"gta" --> the name is reserved, system adds (k), since "gta(1)" is also reserved, systems put k = 2. it becomes "gta(2)"
"avalon" --> not assigned before, remains "avalon"
```

**Example 3**:

```
Input: names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]
Output: ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]
Explanation: When the last folder is created, the smallest positive valid k is 4, and it becomes "onepiece(4)".
```

**Example 4**:

```
Input: names = ["wano","wano","wano","wano"]
Output: ["wano","wano(1)","wano(2)","wano(3)"]
Explanation: Just increase the value of k each time you create folder "wano".
```

**Example 5**:

```
Input: names = ["kaido","kaido(1)","kaido","kaido(1)"]
Output: ["kaido","kaido(1)","kaido(2)","kaido(1)(1)"]
Explanation: Please note that system adds the suffix (k) to current name even it contained the same suffix before.
```



**Constraints**:

* $1 \le \text{names.length} \le 5 * 10^4$
* $1 \le \text{names[i].length} \le 20$
* `names[i]` consists of lower case English letters, digits and/or round brackets.



## 题目解读

&emsp;给定字符串，求不重复的字符串编号。

```java
class Solution {
    public String[] getFolderNames(String[] names) {

    }
}
```

## 程序设计

* 最基本的思路是字典树插入判断，如果插入值存在，则从编号`(1)`开始判断，直到遇到不存在的编号，分配即可。

```java
class Solution {
    public String[] getFolderNames(String[] names) {
        String[] res = new String[names.length];
        Trie root = new Trie();
        for (int i = 0; i < names.length; i++) {
            res[i] = root.insert(names[i]);
        }
        return res;
    }
}

class Trie {
    boolean isEnding;
    Map<String, Trie> children = new HashMap<>();

    public String insert(String word) {
        Trie tmp = this;
        char[] seq = word.toCharArray();
        for (int i = 0; i < seq.length; i++) {
            String cur;
            // 提取括号中的数字
            if (seq[i] == '(') {
                cur = "";
                i++;
                while (i < seq.length && seq[i] != ')') {
                    cur += Character.toString(seq[i++]);
                }
            } else {
                cur = Character.toString(seq[i]);
            }
            if (tmp.children.get(cur) == null) tmp.children.put(cur, new Trie());
            tmp = tmp.children.get(cur);
        }

        // 冲突，拼接数字继续尝试
        if (tmp.isEnding) {
            int no = 1;
            // 已存在编号，编号递增
            while (tmp.children.containsKey(Integer.toString(no)) && tmp.children.get(Integer.toString(no)).isEnding) no++;
            // 避免覆盖
            if (!tmp.children.containsKey(Integer.toString(no))) tmp.children.put(Integer.toString(no), new Trie());
            tmp = tmp.children.get(Integer.toString(no));
            word += "(" + no + ")";
        } 
        tmp.isEnding = true;
        // 返回最终插入的字符串
        return word;
    }
}
```

* 上述思路在数据量大时会超时，因为在插入中分配编号时，从低到高遍历，可提前记录之前连续分配的最大值，减少遍历时间。

```java
class Solution {
    public String[] getFolderNames(String[] names) {
        String[] res = new String[names.length];
        Trie root = new Trie();
        for (int i = 0; i < names.length; i++) {
            res[i] = root.insert(names[i]);
        }
        return res;
    }
}

class Trie {
    boolean isEnding;
    Map<String, Trie> children = new HashMap<>();
    // 子节点连续分配最大编号
    int maxNo = 1;

    public String insert(String word) {
        Trie tmp = this;
        char[] seq = word.toCharArray();
        for (int i = 0; i < seq.length; i++) {
            String cur;
            // 提取括号中的数字
            if (seq[i] == '(') {
                cur = "";
                i++;
                while (i < seq.length && seq[i] != ')') {
                    cur += Character.toString(seq[i++]);
                }
            } else {
                cur = Character.toString(seq[i]);
            }
            if (tmp.children.get(cur) == null) tmp.children.put(cur, new Trie());
            tmp = tmp.children.get(cur);
        }

        // 冲突，拼接数字继续尝试
        if (tmp.isEnding) {
            // 已存在编号，编号递增
            while (tmp.children.containsKey(Integer.toString(tmp.maxNo)) && tmp.children.get(Integer.toString(tmp.maxNo)).isEnding) tmp.maxNo++;
            // 避免覆盖
            if (!tmp.children.containsKey(Integer.toString(tmp.maxNo))) tmp.children.put(Integer.toString(tmp.maxNo), new Trie());
            word += "(" + tmp.maxNo + ")";
            tmp = tmp.children.get(Integer.toString(tmp.maxNo));
        } 
        tmp.isEnding = true;
        // 返回最终插入的字符串
        return word;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(L)$，空间复杂度为$O(L)$，$L$为字符串总长度。

执行用时：79 ms, 在所有 Java 提交中击败了56.25%的用户

内存消耗：61.5 MB, 在所有 Java 提交中击败了5.23%的用户。

## 官方解题

&emsp;暂无，密切关注。可用哈希表记录字符串出现次数，

```java
class Solution {
    public String[] getFolderNames(String[] names) {
        Map<String, Integer> nameCache = new HashMap<>();
        String[] result = new String[names.length];
        for (int i = 0; i < names.length; i++) {
            String name = names[i];
            int time = nameCache.getOrDefault(names[i], 0);
            while (nameCache.containsKey(name)) {
                name = names[i] + "(" + ++time + ")";
            }
            nameCache.put(names[i], time);
            nameCache.put(name, 0);
            result[i] = name;
        }
        return result;
    }
}
```

执行用时：47 ms, 在所有 Java 提交中击败了98.96%的用户。

内存消耗：54.1 MB, 在所有 Java 提交中击败了34.55%的用户。