[toc]

Given a string `s`, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.



## 题目解读

&emsp;给定字符串，返回所有可能的回文字符串。

```java
class Solution {
    public List<String> generatePalindromes(String s) {
        
    }
}
```

## 程序设计

* 回溯尝试。

```java
class Solution {
    public List<String> generatePalindromes(String s) {
        List<String> res = new LinkedList<>();
        if (s == null || s.isEmpty()) return res;

        // 统计字符
        Map<Character, Integer> counter = new HashMap<>();
        for (char c : s.toCharArray()) counter.put(c, counter.getOrDefault(c, 0) + 1);
        
        char oddChar = ' ';
        int oddCount = 0;
        // 奇数字符最多只有一个
        for (Map.Entry<Character, Integer> entry : counter.entrySet()) {
            if ((entry.getValue() & 1) == 1) {
                oddChar = entry.getKey();
                oddCount++;
            }
            if (oddCount > 1) return res;
        }

        char[] palindrome = new char[s.length()];
        if (oddCount == 1) palindrome[palindrome.length / 2] = oddChar;

        generatePalindromes(palindrome, 0, s.length(), counter, res);
        return res;
    }

    private void generatePalindromes(char[] palindrome, int idx, int n, Map<Character, Integer> counter, List<String> res) {
        if (idx >= n / 2) {
            res.add(new String(palindrome));
            return;
        }

        // 当前位置尝试字符i
        for (char c : counter.keySet()) {
            if (counter.get(c) < 2) continue;
            counter.put(c, counter.getOrDefault(c, 0) - 2);
            palindrome[idx] = palindrome[n - 1 - idx] = c;
            generatePalindromes(palindrome, idx + 1, n, counter, res);
            counter.put(c, counter.getOrDefault(c, 0) + 2);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\frac{N}{2}!)$，空间复杂度为$O(N)$.

执行用时：2ms，在所有java提交中击败了92.90%的用户。

内存消耗：40MB，在所有java提交中击败了70.00%的用户。

## 官方解题

&emsp;官方思路类似。