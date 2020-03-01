[toc]

Given an array of strings, group anagrams together.

Note:

* All inputs will be in lowercase.
* The order of your output does not matter.



## 题目解读

&emsp;给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

    }
}
```

## 程序设计

* 直观的方法就是遍历字符串，然后比较并加入相应的链表，如果和目前存在的单词都构不成字母异位词则创建新的链表。

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> res = new LinkedList<>();
        if(strs == null || strs.length == 0) {
            return res;
        }
        for(String str : strs) {
            if(res.size() == 0) {
                List<String> temp = new LinkedList<>();
                temp.add(str);
                res.add(temp);
                continue;
            }
            boolean flag = true;
            for(List<String> l : res) {
                // 找到并插入
                if(anagrams(l.get(0), str)) {
                    l.add(str);
                    flag = false;
                    break;
                }
            }
            // 未找到就新建链表
            if(flag) {
                List<String> temp = new LinkedList<>();
                temp.add(str);
                res.add(temp);
            }
        }
        return res;
    }

    private boolean anagrams(String str1, String str2) {
        if(str1 == null || str2 == null || str1.length() != str2.length())
            return false;
        // 根据单词计数比较
        int[] set1 = new int[26];
        int[] set2 = new int[26];
        for(char c : str1.toCharArray()) {
            set1[c - 'a'] += 1;
        }
        for(char c : str2.toCharArray()) {
            set2[c - 'a'] += 1;
        }
        for(int i = 0; i < 26; i++) {
            if(set1[i] != set2[i]) {
                return false;
            }
        }
        return true;
    }
}
```

* 考虑到上面代码全部用链表，每次都要遍历比较；其次比较方法使用数组，每次都要重新统计，如果以统计数组为键，字符串链表为值，则每次遍历只需统计当前单词数组，然后取字典值更新即可。优化如下：

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> res = new LinkedList<>();
        if(strs == null || strs.length == 0) {
            return res;
        }
        Map<String, List<String>> map = new HashMap<>();
        for(String str : strs) {
            // 统计
            int[] counter = new int[26];
            for(char c : str.toCharArray()) {
                counter[c - 'a'] += 1;
            }
            // 方便最为键，转化为字符串
            StringBuffer sb = new StringBuffer();
            for(int i = 0; i < 26; i++) {
                sb.append('a' + i).append(counter[i]);
            }
            // 没有则创建
            if(map.get(sb.toString()) == null) {
                map.put(sb.toString(), new LinkedList<>());
            }
            // 更新
            map.get(sb.toString()).add(str);
        }
        for(String key : map.keySet()) {
            res.add(map.get(key));
        }
        return res;
    }
}
```

## 性能分析

&emsp;未优化代码时间复杂度为$O(KN)$，空间复杂度为$N$，其中$N$为单词数目，$K$为分组数目。

执行用时：1774ms，在所有java提交中击败了5.01%的用户。

内存消耗：44.6MB，在所有java提交中击败了29.49%的用户。

&emsp;优化后时间复杂度未变。

执行用时：36ms，在所有java提交中击败了16.56%的用户。

内存消耗：46.6MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;同上。