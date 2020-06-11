[toc]

You have a set of `tiles`, where each tile has one letter `tiles[i]` printed on it.  Return the number of possible non-empty sequences of letters you can make.

 

**Note:**

* $1 \le \text{tiles.length} \le 7$
* `tiles` consists of uppercase English letters.



## 题目解读

&emsp;给定一套活字字模，其中每个字模上都刻有一个字母。返回可以印出的非空字母序列的数目。每个活字字模只能使用一次。

```java
class Solution {
    public int numTilePossibilities(String tiles) {

    }
}
```

## 程序设计

* 最基本的采用回溯尝试所有组合，由于存在重复字符，需要集合去重。

```java
class Solution {
    int count;
    Set<String> set;

    public int numTilePossibilities(String tiles) {
        if (tiles == null || tiles.isEmpty()) return count;

        this.set = new HashSet<>();
        boolean[] visited = new boolean[tiles.length()];
        char[] strs = tiles.toCharArray();
        // 选择起始字符
        for (int i = 0; i < strs.length; i++) {
            numTilePossibilities(strs, i, new StringBuffer(), visited);
        }
        return count;
    }

    private void numTilePossibilities(char[] strs, int start, StringBuffer sb, boolean[] visited) {
        if (visited[start]) return;
        // 标记为已访问
        visited[start] = true;
        sb.append(strs[start]);
        if (!set.contains(sb.toString())) {
            set.add(sb.toString());
            count++;
        }

        for (int i = 0; i < strs.length; i++) {
            if (visited[i]) continue;
            numTilePossibilities(strs, i, sb, visited);
        }

        // 回溯
        visited[start] = false;
        sb.setLength(sb.length() - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(2^N)$。

执行用时：27ms，在所有java提交中击败了35.08%的用户。

内存消耗：40.7MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有两种思路高效去重，第一种统计每个字符数目，根据数目回溯。

```java
class Solution {
    int count;

    public int numTilePossibilities(String tiles) {
        if (tiles == null || tiles.isEmpty()) return count;

        int num = tiles.length();
        int[] counter = new int[26];
        for (char c : tiles.toCharArray()) {
            counter[c - 'A']++;
        }
        numTilePossibilities(counter, 0, num);
        return count;
    }

    private void numTilePossibilities(int[] counter, int cur, int num) {
        if (cur > num) return;

        for (int i = 0; i < 26; i++) {
            if (counter[i] == 0) continue;
            counter[i]--;
            numTilePossibilities(counter, cur + 1, num);
            count++;
            counter[i]++;
        }
    }
}
```

执行用时：4ms，在所有java提交中击败了66.81%的用户。

内存消耗：37.6MB，在所有java提交中击败了60.00%的用户。

&emsp;第二种对字符排序，连续序列`aaa`从起始开始尝试包含所有的情况，若检测到中间断开，则不必重复尝试，如尝试第一个和第三个字符是尝试第一个和第二个的重复情况。

```java
class Solution {
    int count;

    public int numTilePossibilities(String tiles) {
        if (tiles == null || tiles.isEmpty()) return count;

        char[] strs = tiles.toCharArray();
        // 排序
        Arrays.sort(strs);
        numTilePossibilities(strs, 0, new boolean[strs.length]);
        return count;
    }

    private void numTilePossibilities(char[] strs, int size, boolean[] visited) {
        if (size > strs.length) return;

        for (int i = 0; i < strs.length; i++) {
            if (visited[i]) continue;
            // 排除重复，相同字符起始开始尝试，连续的序列包含所有情况，
            // 若中间有间断则是重复序列，不予尝试
            if (i > 0 && strs[i] == strs[i - 1] && !visited[i - 1]) continue;

            visited[i] = true;
            count++;
            numTilePossibilities(strs, size + 1, visited);
            visited[i] = false;
        }
    }
}
```

执行用时：2ms，在所有java提交中击败了97.69%的用户。

内存消耗：37.3MB，在所有java提交中击败了60.00%的用户。