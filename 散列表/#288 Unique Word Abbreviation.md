[toc]

An abbreviation of a word follows the form `<first letter><number><last letter>`. Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.



## 题目解读

&emsp;假设单词的缩写形式为首字母加中间字符数加结尾字母，给定字典，判断缩写是否唯一。

```java
class ValidWordAbbr {

    public ValidWordAbbr(String[] dictionary) {

    }
    
    public boolean isUnique(String word) {

    }
}

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * ValidWordAbbr obj = new ValidWordAbbr(dictionary);
 * boolean param_1 = obj.isUnique(word);
 */
```

## 程序设计

* 使用字典记录缩写和缩写计数，使用集合记录字典中的单词。查询时判断是否在缩写字典中，不在或次数只有一个且是当前值，则表示是唯一的。

```java
class ValidWordAbbr {
    // 记录缩写数目
    Map<String, Integer> record;
    // 记录字符串
    Set<String> words;

    public ValidWordAbbr(String[] dictionary) {
        record = new HashMap<>();
        words = new HashSet<>();
        for(String s : dictionary) {
            // 长度不符合要求，不记录
            if(s == null || s.length() <= 2) {
                continue;
            }
            String key = getMapKey(s);
            record.put(key, record.getOrDefault(key, 0) + 1);
            words.add(s);
        }
    }
    
    public boolean isUnique(String word) {
        // 长度不符合要求
        if(word == null || word.length() <= 2) {
            return true;
        }
        // 字典中不存在，或存在一个就是当前字符串
        return record.get(getMapKey(word)) == null || (record.get(getMapKey(word)) == 1 && words.contains(word));
    }

    private String getMapKey(String word) {
        StringBuffer sb = new StringBuffer();
        sb.append(word.charAt(0)).append(word.length() - 2).append(word.charAt(word.length() - 1));
        return sb.toString();
    }
}
```

## 性能分析

&emsp;构造方法时间复杂度为$O(N)$，查询时间复杂度为$O(1)$；空间复杂度为$O(N)$。

执行用时：89ms，在所有java提交中击败了50.41%的用户。

内存消耗：53MB，在所有java提交中击败了78.85%的用户。

## 官方解题

&emsp;同上。