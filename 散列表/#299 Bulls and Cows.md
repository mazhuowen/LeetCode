[toc]

You are playing the following `Bulls and Cows` game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

Write a function to return a hint according to the secret number and friend's guess, use `A` to indicate the bulls and `B` to indicate the cows. 

Please note that both secret number and friend's guess may contain duplicate digits.

Note: You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.



## 题目解读

&emsp;你正在和你的朋友玩 猜数字（Bulls and Cows）游戏：你写下一个数字让你的朋友猜。每次他猜测后，你给他一个提示，告诉他有多少位数字和确切位置都猜对了（称为Bulls, 公牛），有多少位数字猜对了但是位置不对（称为Cows, 奶牛）。你的朋友将会根据提示继续猜，直到猜出秘密数字。请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。请注意秘密数字和朋友的猜测数都可能含有重复数字。

```java
class Solution {
    public String getHint(String secret, String guess) {

    }
}
```

## 程序设计



```java
class Solution {
    public String getHint(String secret, String guess) {
        if(secret == null || secret.isEmpty()) {
            return "";
        }
        int bulls = 0, cows = 0;
        // 计数
        Map<Character, Integer> record = new HashMap<>();
        for(int i = 0; i < secret.length(); i++) {
            // 计数
            record.put(secret.charAt(i), record.getOrDefault(secret.charAt(i), 0) + 1);
            // 统计位置对的数目
            if(secret.charAt(i) == guess.charAt(i)) {
                bulls++;
            }
        }
        for(int i = 0; i < guess.length(); i++) {
            // 统计存在于guess的数目，每次计数减一
            if(record.getOrDefault(guess.charAt(i), 0) > 0) {
                record.put(guess.charAt(i), record.get(guess.charAt(i)) - 1);
                cows++;
            }
        }
        // 减去重复统计的值
        cows -= bulls;
        return bulls + "A" + cows + "B";
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了28.11%的用户。

内存消耗：38.6MB，在所有java提交中击败了5.21%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路相同，时间性能较快的方法采用数组而非集合类。