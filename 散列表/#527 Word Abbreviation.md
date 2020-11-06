[toc]

Given an array of n distinct non-empty strings, you need to generate **minimal** possible abbreviations for every word following rules below.

* Begin with the first character and then the number of characters abbreviated, which followed by the last character.
* If there are any conflict, that is more than one words share the same abbreviation, a longer prefix is used instead of only the first character until making the map from word to abbreviation become unique. In other words, a final abbreviation cannot map to more than one original words.
* If the abbreviation doesn't make the word shorter, then keep it as original.



**Note**:

* Both n and the length of each word will not exceed 400.
* The length of each word is greater than 1.
* The words consist of lowercase English letters only.
* The return answers should be in the same order as the original array.



## 题目解读

&emsp;给定单词字典，按次序返回单词的缩写。缩写为首字符加后续长度加尾字符，但是当缩写发生冲突时，前缀可由一个字符拓展为两个字符，依次类推；如果缩写长度不比单词本身短，则缩写就是单词本身。

```java
class Solution {
    public List<String> wordsAbbreviation(List<String> dict) {

    }
}
```

## 程序设计

* 最基本的思路是，使用字典保存缩写和单词的映射，按照规则生成缩写，当缩写冲突时表示当前的两个单词都需要重新生成缩写。这个过程可以使用队列实现，遇到冲突，则将字典中保存的单词入队，当前单词继续生成，并检查是否冲突，直到没有冲突，加入字典。
* 以单词`internal`和`interval`为例，首先单词`internal`生成缩写`i6l`加入字典；单词`interval`生成`i6l`缩写冲突，于是将`internal`加入队列，同时字典`i6l`置空但不删除，标识有冲突；`interval`继续生成缩写`in5l`，没有冲突，加入字典；队列下一个单词是`internal`，生成缩写`i6l`发生冲突，继续生成缩写`in5l`发生冲突，继续生成`int4l`。以此类推，直到队列为空。

```java
class Solution {
    public List<String> wordsAbbreviation(List<String> dict) {
        List<String> res = new LinkedList<>();
        if(dict == null || dict.isEmpty()) {
            return res;
        }
        // 记录缩写单词关系
        Map<String, String> record = new HashMap<>();
        // 记录单词缩写关系
        Map<String, String> relation = new HashMap<>();
        Queue<String> queue = new LinkedList<>(dict);
        while(!queue.isEmpty()) {
            String word = queue.poll();
            int len = word.length();
            for(int i = 1; i < len; i++) {
                // 缩写
                String key;
                // 如果两个字符之间的距离小于等于1，就是单词本身
                if(len - i - 1 <= 1) {
                    key = word;
                } 
                // 否则截取前面部分和最后一个字符，与数字拼接成新的缩写
                else {
                    key = word.substring(0, i) + (len - i - 1) + word.charAt(len - 1);
                }
                // 存在缩写冲突，则将已有缩写的单词重新放入链表，同时当前单词继续生成缩写
                if(record.containsKey(key)) {
                    if(record.get(key) != null) {
                        // 重新放入链表，同时置空
                        queue.add(record.get(key));
                        record.put(key, null);
                    }
                } else {
                    // 暂无缩写占用，放入字典，并遍历下一个单词
                    record.put(key, word);
                    relation.put(word, key);
                    break;
                }
            }
        }
        // 按次序转换为缩写
        for(String word : dict) {
            res.add(relation.get(word));
        }
        return res;
    }
}
```

## 性能分析

&emsp;设$N$为单词数，$m$为平均单词长度。贪心法最好的情况没有缩写冲突，只需遍历$N$个单词，截取首尾字符需要遍历$m$长度，时间复杂度为$O(Nm)$，最坏的情况为单词表里单词全部都冲突，缩写就是单词本身，此时由于每一位都有冲突，每个单词需要入队$m$次，共$N$个单词，每个单词出队后又要遍历$m$次，每次遍历都比较缩写，且缩写截取需要花费$m$，总的时间复杂度为$O(Nm^3)$；空间复杂度为$O(Nm)$。

执行用时：44ms，在所有java提交中击败了43.33%的用户。

内存消耗：43MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;官方也提供了基础的贪心法。其思路较为简单，生成所有的缩写，然后遍历检测当前单词后后续单词是否有重复，有则循环生成缩写直到没有重复。

```java
class Solution {
    public List<String> wordsAbbreviation(List<String> words) {
        int N = words.size();
        String[] ans = new String[N];
        int[] prefix = new int[N];

        // 初始生成以首尾字符为缩写的缩写
        for (int i = 0; i < N; ++i)
            ans[i] = abbrev(words.get(i), 0);

        // 当前单词
        for (int i = 0; i < N; ++i) {
            while (true) {
                Set<Integer> dupes = new HashSet<>();
                // 遍历以后存在重复的单词，并加入集合
                for (int j = i + 1; j < N; j++) {
                    if (ans[i].equals(ans[j])) dupes.add(j);
                }
                // 不存在重复，就是当前缩写
                if (dupes.isEmpty()) break;
                dupes.add(i);
                // 对每个重复的单词重新生成索引，然后继续while循环检测，直到不存在循环break脱出
                for (int k: dupes)
                    ans[k] = abbrev(words.get(k), ++prefix[k]);
            }
        }

        return Arrays.asList(ans);
    }

    public String abbrev(String word, int i) {
        int N = word.length();
        if (N - i <= 3) return word;
        return word.substring(0, i + 1) + (N - i - 2) + word.charAt(N - 1);
    }
}
```

&emsp;时间复杂度为$O(N^2m^2)$，空间复杂度为$O(Nm)$。

执行用时：44ms，在所有java提交中击败了43.33%的用户。

内存消耗：42.8MB，在所有java提交中击败了25.00%的用户。

&emsp;官方还提供了一种思路计算最短公共前缀。首先计算缩写并加入字典，使用链表解决冲突。然后对每个链表排序并依次计算和前一个单词的最短前缀，然后重新计算缩写。

```java
class Solution {
    public List<String> wordsAbbreviation(List<String> words) {
        Map<String, List<IndexedWord>> groups = new HashMap<>();
        String[] ans = new String[words.size()];
        // 初始缩写，有冲突则放入同一个链表
        int index = 0;
        for (String word : words) {
            String ab = abbrev(word, 0);
            if (!groups.containsKey(ab)) groups.put(ab, new ArrayList<>());
            groups.get(ab).add(new IndexedWord(word, index));
            index++;
        }

        // 遍历冲突的缩写单词，根据字典序排序（必然长度相等，首尾字符相等）
        for (List<IndexedWord> group: groups.values()) {
            Collections.sort(group, (a, b) -> a.word.compareTo(b.word));
            // 查找最短公共前缀
            int[] lcp = new int[group.size()];
            for (int i = 1; i < group.size(); i++) {
                int p = longestCommonPrefix(group.get(i - 1).word, group.get(i).word);
                lcp[i] = p;
                lcp[i - 1] = Math.max(lcp[i - 1], p);
            }
            // 得到最短公共前缀后，重新计算
            for (int i = 0; i < group.size(); i++) {
                ans[group.get(i).index] = abbrev(group.get(i).word, lcp[i]);
            }
        }

        return Arrays.asList(ans);
    }
	// 截取前i个字符和最后一个字符组成缩写
    public String abbrev(String word, int i) {
        int N = word.length();
        if (N - i <= 3) return word;
        return word.substring(0, i + 1) + (N - i - 2) + word.charAt(N - 1);
    }
	// 计算相同前缀字符数
    public int longestCommonPrefix(String word1, String word2) {
        int i = 0;
        while (i < word1.length() && i < word2.length()
                && word1.charAt(i) == word2.charAt(i))
            i++;
        return i;
    }
}
// 记录索引，方便后续按顺序输出
class IndexedWord {
    String word;
    int index;
    
    IndexedWord(String w, int i) {
        word = w;
        index = i;
    }
}
```

&emsp;时间复杂度为$O(Nm\log_2Nm)$，空间复杂度为$O(NM)$。

执行用时：29ms，在所有java提交中击败了80.00%的用户。

内存消耗：42.8MB，在所有java提交中击败了25.00%的用户。