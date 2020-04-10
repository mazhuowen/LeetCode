[toc]

A string such as `"word"` contains the following abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```


Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the **smallest possible** length such that it does not conflict with abbreviations of the strings in the dictionary.

Each **number** or letter in the abbreviation is considered length = 1. For example, the abbreviation `"a32bc"` has length = 4.

Note:

* In the case of multiple answers as shown in the second example below, you may return any one of them.
* Assume length of target string = **m**, and dictionary size = **n**. You may assume that $m \le 21$, $n \le 1000$, and $log_2n + m \le 20$.



## 题目解读

&emsp;给定一个词，获取其最短的缩写，要求不能是给定字典中的单词的最短缩写。

```java
class Solution {
    public String minAbbreviation(String target, String[] dictionary) {

    }
}
```

## 程序设计

* 本题是[#320 Generalized Abbreviation](./#320 Generalized Abbreviation.md)和[#408 Valid Word Abbreviation](../字符串/#408 Valid Word Abbreviation.md)结合的题目，具体思路为先回溯生成所有的缩写，然后根据长度排序，最后从头开始依次校验缩写是否是字典中的缩写，第一个未匹配到的缩写就是答案。
* 分析该思路是否可行，首先回溯生成所有缩写最坏为$M2^M$，最多有$2^M$个组合，排序需要花费$2^M\log_22^M = M2^M$的时间，循环比较，假设最坏情况遍历到最后一位，则需要花费$2^MN$，每次遍历比较需要花费$M$，即比较环节总的时间复杂度为$2^MMN$。这样总的时间复杂度为$O(M2^M + M^2N)$。由题目知$M2^M \le 21 * 2^{21} = 44040192$，由于$\log_2n + m \le 20 \implies 2^MN \le 2^{20}$，故$2^MMN \le 21 * 2^20 = 22020096$，可见时间复杂度可以接受。
* 生成方法在[#320 Generalized Abbreviation](./#320 Generalized Abbreviation.md)的基础上只需记录每个缩写的长度，方便排序；比较方法参考[#408 Valid Word Abbreviation](../字符串/#408 Valid Word Abbreviation.md)无需修改。

```java
class Solution {
    public String minAbbreviation(String target, String[] dictionary) {
        if (target == null) throw new IllegalArgumentException("invalid param");
        if (dictionary == null || dictionary.length == 0) return target.length() + "";

        // 生成所有的缩写词
        List<Pair<String, Integer>> apprs = generateAbbreviations(target);
        // 排序
        apprs.sort((a, b) -> a.getValue() - b.getValue());

        String res = null;
        // 比较排除
        for (Pair<String, Integer> pair : apprs) {
            String appr = pair.getKey();
            boolean flag = true;
            for (String word : dictionary) {
                // 匹配到，表示当前缩写被占用，继续下一个缩写比较
                if (validWordAbbreviation(word, appr)) {
                    flag = false;
                    break;
                }
            }
            // 当前缩写没有匹配到，就是答案，返回
            if (flag) {
                res = appr;
                break;
            }
        }
        return res;
    }

    
    private List<Pair<String, Integer>> generateAbbreviations(String target) {
        List<Pair<String, Integer>> res = new LinkedList<>();
        generateAbbreviations(target.toCharArray(), 0, new StringBuffer(), 0, 0, res);
        return res;
    }

    private void generateAbbreviations(char[] chars, int start, StringBuffer sb, int num, int size, List<Pair<String, Integer>> res) {
        int len = sb.length();
        // 找到一组可行解
        if (start >= chars.length) {
            if (num > 0) {
                size++;
                sb.append(num);
            }
            res.add(new Pair(sb.toString(), size));
        } else {
            // 尝试数字
            generateAbbreviations(chars, start + 1, sb, num + 1, size, res);

            // 尝试字母
            if (num > 0) {
                sb.append(num);
                size++;
            }
            sb.append(chars[start]);
            generateAbbreviations(chars, start + 1, sb, 0, size + 1, res);
        }
        // 回溯
        sb.setLength(len);
    }

    private boolean validWordAbbreviation(String word, String appr) {
        if (word.isEmpty()) return appr.isEmpty();

        char[] words = word.toCharArray();
        char[] apprs = appr.toCharArray();
        int wIdx = 0, aIdx = 0;

        while (wIdx < words.length) {
            int num = 0, temp = aIdx;
            while (aIdx < apprs.length && Character.isDigit(apprs[aIdx])) {
                num = num * 10 + (apprs[aIdx] - '0');
                // 以0开头的数字
                if (aIdx > temp && num == 0) return false;
                aIdx++;
            }
            if (num == 0) {
                if (aIdx == temp + 1 || aIdx >= apprs.length || words[wIdx++] != apprs[aIdx++]) return false;
            } else {
                wIdx += num;
            }
        }
        return wIdx == words.length && aIdx == apprs.length;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M2^M + M^2N)$，空间复杂度为$O(2^M)$。

执行用时：99ms，在所有java提交中击败了72.73%的用户。

内存消耗：61.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。