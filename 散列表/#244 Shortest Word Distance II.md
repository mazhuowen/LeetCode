[toc]

Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list. Your method will be called repeatedly many times with different parameters. 

Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.



## 题目解读

&emsp;设计功能，返回两个字符串在数组中的最短距离。

```java
class WordDistance {

    public WordDistance(String[] words) {

    }
    
    public int shortest(String word1, String word2) {

    }
}

/**
 * Your WordDistance object will be instantiated and called as such:
 * WordDistance obj = new WordDistance(words);
 * int param_1 = obj.shortest(word1,word2);
 */
```

## 程序设计

* 最简单的思路是在构造函数中维护好所有的距离关系，但是会报超时。

```java
class WordDistance {
    Map<String, Integer> distance;

    public WordDistance(String[] words) {
        this.distance = new HashMap<>();
        for(int i = 1; i < words.length; i++) {
            for(int j = 0; j < i; j++) {
                String key1 = words[i] + "#" + words[j];
                String key2 = words[j] + "#" + words[i];
                distance.put(key1, Math.min(i - j, distance.getOrDefault(key1, Integer.MAX_VALUE)));
                distance.put(key2, Math.min(i - j, distance.getOrDefault(key2, Integer.MAX_VALUE)));
            }
        }
    }
    
    public int shortest(String word1, String word2) {
        return distance.get(word1 + "#" + word2);
    }
}
```

* 转变思路，在查询时更新距离：

```java
class WordDistance {
    // 记录距离
    private Map<String, Integer> distance;
    // 记录每个字符串的索引
    private Map<String, List<Integer>> idx;

    public WordDistance(String[] words) {
        distance = new HashMap<>();
        idx = new HashMap<>();
        // 加入每个字符串在数组中的索引
        for(int i = 0; i < words.length; i++) {
            if(idx.get(words[i]) == null) {
                idx.put(words[i], new ArrayList<>());
            }
            idx.get(words[i]).add(i);
        }
    }
    
    public int shortest(String word1, String word2) {
        String key1 = word1 + "#" + word2;
        String key2 = word2 + "#" + word1;
        if(distance.get(key1) == null) {
            // 更新距离
            int minDis = Integer.MAX_VALUE;
            List<Integer> l1 = idx.get(word1);
            List<Integer> l2 = idx.get(word2);
            for(int idx1 : l1) {
                for(int idx2 : l2) {
                    minDis = Math.min(minDis, Math.abs(idx1 - idx2));
                }
            }
            distance.put(key1, minDis);
            distance.put(key2, minDis);
        }
        return distance.get(key1);
    }
}
```

## 性能分析

&emsp;构造函数时间复杂度为$O(N)$，查询时间复杂度最坏为$O(N^2)$，平均为$O(1)$。空间复杂度为$O(N)$。

执行用时：40ms，在所有java提交中击败了25.69%的用户。

内存消耗：48.4MB，在所有java提交中击败了6.10%的用户。

## 官方解题

&emsp;官方优化查询时的循环，由于是有序的，可以两条一起遍历。

```java
class WordDistance {
	// 保存索引
    HashMap<String, ArrayList<Integer>> locations;

    public WordDistance(String[] words) {
        this.locations = new HashMap<String, ArrayList<Integer>>();
		// 加入索引
        for (int i = 0; i < words.length; i++) {
            ArrayList<Integer> loc = this.locations.getOrDefault(words[i], new ArrayList<Integer>());
            loc.add(i);
            this.locations.put(words[i], loc);
        }
    }

    public int shortest(String word1, String word2) {
        ArrayList<Integer> loc1, loc2;

        loc1 = this.locations.get(word1);
        loc2 = this.locations.get(word2);
		// 遍历两个链表，由于是有序的同时遍历，其中一条遍历完就结束遍历
        // 另外一条无需遍历，因为后面的索引更大，距离比当前距离大
        int l1 = 0, l2 = 0, minDiff = Integer.MAX_VALUE;
        while (l1 < loc1.size() && l2 < loc2.size()) {
            minDiff = Math.min(minDiff, Math.abs(loc1.get(l1) - loc2.get(l2)));
            if (loc1.get(l1) < loc2.get(l2)) {
                l1++;
            } else {
                l2++;
            }
        }

        return minDiff;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：36ms，在所有java提交中击败了36.24%的用户。

内存消耗：48.7MB，在所有java提交中击败了6.10%的用户。