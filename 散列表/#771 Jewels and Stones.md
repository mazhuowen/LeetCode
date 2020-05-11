[toc]

You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have.  Each character in `S` is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters. Letters are case sensitive, so "a" is considered a different type of stone from `"A"`.



Note:

* `S` and `J` will consist of letters and have length at most 50.
* The characters in `J` are distinct.



## 题目解读

&emsp;判断目标字符串出现要求的字符有多少个。

```java
class Solution {
    public int numJewelsInStones(String J, String S) {
        
    }
}
```

## 程序设计

* 哈希表统计计数。

```java
class Solution {
    public int numJewelsInStones(String J, String S) {
        if (J == null || J.isEmpty() || S == null || S.isEmpty()) return 0;

        // 计数宝石字符串
        int[] counter = new int[52];
        for (char c : J.toCharArray()) {
            if (Character.isLowerCase(c)) {
                counter[c - 'a']++;
            } else {
                counter[c - 'A' + 26]++;
            }
        }

        // 判断目标字符串是否包含宝石
        int count = 0;
        for (char c : S.toCharArray()) {
            if (Character.isLowerCase(c)
                    && counter[c - 'a'] > 0 || Character.isUpperCase(c) && counter[c - 'A' + 26] > 0)
                count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(M,N))$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.61%的用户。

内存消耗：38.4MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;官方思路一致。