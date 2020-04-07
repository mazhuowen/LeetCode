[toc]

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.



## 题目解读

&emsp;给定无运算优先级的表达式，给出加了括号后计算出的所有结果。

```java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {

    }
}
```

## 程序设计

* 最简单的思路是遍历当前字符串操作符，然后将操作符两侧二分递归；如果当前字符串没有操作符，表明是一个数字，返回即可。

```java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> res = new LinkedList<>();
        if (input == null || input.isEmpty()) return res;

        int len = input.length();
        char[] chars = input.toCharArray();
        return diffWaysToCompute(chars, 0, len - 1);
    }

    private List<Integer> diffWaysToCompute(char[] input, int start, int end) {
        List<Integer> res = new LinkedList<>();
        for (int i = start; i <= end; i++) {
            char c = input[i];
            // 数字则跳过
            if (c <= '9' && c >= '0') continue;

            List<Integer> left = diffWaysToCompute(input, start, i - 1);
            List<Integer> right = diffWaysToCompute(input, i + 1, end);
            // 计算左右组合
            cacul(c, left, right, res);
        }
        // 递归终止条件，此时字符串只包含一个数字
        if (res.size() == 0) {
            int num = 0;
            while (start <= end) {
                num = num * 10 + (input[start++] - '0');
            }
            res.add(num);
        }
        return res;
    }

    private void cacul(char op, List<Integer> nums1, List<Integer> nums2, List<Integer> res) {
        for (int num1 : nums1) {
            for (int num2 : nums2) {
                if (op == '+') res.add(num1 + num2);                    
                else if (op == '-') res.add(num1 - num2);
                else res.add(num1 * num2);
            }
        }
    }
}
```

* 上述分治法每次递归都存在重复计算，可引入额外空间存储。

```java
class Solution {
    // 记录开始位置到结束位置的计算结果
    List<Integer>[][] record; 

    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> res = new LinkedList<>();
        if (input == null || input.isEmpty()) return res;

        int len = input.length();
        char[] chars = input.toCharArray();
        record = new List[len][len];
        return diffWaysToCompute(chars, 0, len - 1);
    }

    private List<Integer> diffWaysToCompute(char[] input, int start, int end) {
        List<Integer> res = new LinkedList<>();

        if (record[start][end] != null) return record[start][end];

        for (int i = start; i <= end; i++) {
            char c = input[i];
            // 数字则跳过
            if (c <= '9' && c >= '0') continue;

            List<Integer> left = diffWaysToCompute(input, start, i - 1);
            List<Integer> right = diffWaysToCompute(input, i + 1, end);
            // 计算左右组合
            cacul(c, left, right, res);
        }
        // 递归终止条件，此时字符串只包含一个数字
        if (res.size() == 0) {
            int num = 0;
            for (int i = start; i <= end; i++) {
                num = num * 10 + (input[i] - '0');
            }
            res.add(num);
        }
        // 更新结果
        record[start][end] = res;
        return res;
    }

    private void cacul(char op, List<Integer> nums1, List<Integer> nums2, List<Integer> res) {
        for (int num1 : nums1) {
            for (int num2 : nums2) {
                if (op == '+') res.add(num1 + num2);                    
                else if (op == '-') res.add(num1 - num2);
                else res.add(num1 * num2);
            }
        }
    }
}
```

## 性能分析

&emsp;原始递归时间复杂度为$T(N) = 2(T(0) + T(1) + \dots + T(N - 1))$，即$T(N) = 3T(N - 1)$，时间复杂度为$O(3^N)$，空间复杂度低于$O(N!)$，其中$N$为操作符数目。

执行用时：2ms，在所有java提交中击败了89.84%的用户。

内存消耗：39.6MB，在所有java提交中击败了36.74%的用户。

&emsp;加了额外空间的递归方法时间复杂度为$T(N) = (T(0) + T(N - 1)) + \sum_{i = 1}^{N - 1}(1 + 1)$，即$T(N) = T(N - 1) + 2N - 1$，时间复杂度为$O(N^2)$，空间复杂度较为复杂。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB，在所有java提交中击败了67.35%的用户。

## 官方解题

&emsp;暂无，密切关注。
