[toc]

Given an equation, represented by `words` on left side and the `result` on right side.

You need to check if the equation is solvable under the following rules:

* Each character is decoded as one digit (0 - 9).
* Every pair of different characters they must map to different digits.
* Each `words[i]` and `result` are decoded as one number **without** leading zeros.
* Sum of numbers on left side (`words`) will equal to the number on right side (`result`). 

Return `True` if the equation is solvable otherwise return `False`.



Constraints:

* $2 \le \text{words.length} \le 5$
* $1 \le \text{words[i].length, result.length} \le 7$
* `words[i]`, `result` contains only upper case English letters.
* Number of different characters used on the expression is at most 10.



## 题目解读

&emsp;给定字符编码的等式，每个字符对应一个数字，判断是否有相应的编码。

```java
class Solution {
    public boolean isSolvable(String[] words, String result) {

    }
}
```

## 程序设计

* 基本的思路是统计每个字符的权重，尝试回溯每个字符的可能取值，逐个替换并计算，但是当字符串多于4个后会超时，需要优化终止条件，避免无用试探。

```java
class Solution {
    // 记录字符与权重
    Map<Character, Integer> weight;
    // 记录最高位
    Map<Character, String> head;
    // 数字字符映射关系
    Map<Integer, Character> dig2char;
    // 用于回溯，标识未分配字符
    PriorityQueue<Character> unused;

    public boolean isSolvable(String[] words, String result) {
        if (words == null || words.length == 0) throw new IllegalArgumentException("invalid param");
        if (words.length == 1) return words[0].equals(result);

        // 构建
        weight = new HashMap<>();
        head = new HashMap<>();
        unused = new PriorityQueue<>();
        dig2char = new HashMap<>();

        // 初始化
        for (String word : words) {
            initParam(word, true);
        }
        initParam(result, false);

        return isSolvable(0);
    }

    private boolean isSolvable(int val) {
        // 已分配完，比较结果
        if (unused.isEmpty()) return val == 0;
        // 当前待分配字符
        char cur = unused.poll();

        for (int num = 0; num < 10; num++) {
            // 数字已分配
            if (dig2char.get(num) != null) continue;
            // 0不能分配为开头数字
            if (num == 0 && head.get(cur) != null && head.get(cur).length() > 1) continue;

            // 试探
            dig2char.put(num, cur);
            // 有可行方案，返回
            if (isSolvable(val + weight.getOrDefault(cur, 0) * num)) return true;
            // 回溯
            dig2char.put(num, null);
        }
        // 回溯
        unused.add(cur);
        return false;
    }

    // flag表示正负权重
    private void initParam(String cur, boolean flag) {
        int base = 1;
        for (int j = cur.length() - 1; j >= 0; j--) {
            char c = cur.charAt(j);
            // 统计权重
            if (flag) weight.put(c, weight.getOrDefault(c, 0) + base);
            else weight.put(c, weight.getOrDefault(c, 0) - base);
            // 加入待分配队列
            if (!unused.contains(c)) unused.add(c);
            // 最高位统计
            if (j == 0) {
                // 冲突则选择长的，单个字符的字符串可以为0，故以长的为主
                if (head.get(c) == null || head.get(c).length() < cur.length()) {
                    head.put(c, cur);
                }
            }
            base *= 10;
        }
    }
}
```

* 在试探时，应该根据权重大小来试探，优先试探权重大的字符，而不是一概而论全都试探；其次每次试探都需要粗略判断后续的值是否满足条件，即当前值加后续估计的最大值、最小值的区间是否包含0，对不满足条件的值不予尝试。

```java
class Solution {
    // 记录字符与权重
    Map<Character, Integer> weight;
    // 记录最高位
    Map<Character, String> head;
    // 数字字符映射关系
    Map<Integer, Character> dig2char;
    // 用于回溯，标识未分配字符，按照权重绝对值排序
    PriorityQueue<Pair> unused;

    public boolean isSolvable(String[] words, String result) {
        if (words == null || words.length == 0) throw new IllegalArgumentException("invalid param");
        if (words.length == 1) return words[0].equals(result);

        // 构建
        weight = new HashMap<>();
        head = new HashMap<>();
        dig2char = new HashMap<>();
        // 最大堆
        unused = new PriorityQueue<>((a, b) -> Math.abs(b.weight) - Math.abs(a.weight));

        // 初始化
        for (String word : words) {
            initParam(word, true);
        }
        initParam(result, false);

        for (Map.Entry<Character, Integer> entry : weight.entrySet()) {
            unused.add(new Pair(entry.getKey(), entry.getValue()));
        }

        return isSolvable(0);
    }

    private boolean isSolvable(int val) {
        // 已分配完，比较结果
        if (unused.isEmpty()) return val == 0;
        // 当前待分配字符
        Pair cur = unused.poll();

        for (int num = 0; num < 10; num++) {
            // 数字已分配
            if (dig2char.get(num) != null) continue;
            // 0不能分配为开头数字
            if (num == 0 && head.get(cur.c) != null && head.get(cur.c).length() > 1) continue;

            // 试探
            dig2char.put(num, cur.c);
            // 有可行方案，返回
            int newVal = val + weight.getOrDefault(cur.c, 0) * num;
            // 提前判断当前数值与剩余数值之和是否为0，不满足条件则不回溯
            if (newVal >= -getMax() && newVal <= -getMin() && isSolvable(newVal)) return true;
            // 回溯
            dig2char.put(num, null);
        }
        // 回溯
        unused.add(cur);
        return false;
    }

    // 剩余字符可分配的最大值
    private int getMax() {
        int max = 0;
        int left = 0, right = 9;
        Set<Pair> temp = new HashSet<>();
        while (!unused.isEmpty()) {
            Pair cur = unused.poll();
            if (cur.weight > 0) {
                while (dig2char.get(right) != null) {
                    right--;
                }
                max += right-- * cur.weight;
            } else {
              while (dig2char.get(left) != null) {
                  left++;
              }
              max += left++ * cur.weight;
            }

            temp.add(cur);
        }
        unused.addAll(temp);
        return max;
    }

    // 剩余字符可分配的最小值
    private int getMin() {
        int min = 0;
        int left = 0, right = 9;
        Set<Pair> temp = new HashSet<>();
        while (!unused.isEmpty()) {
            Pair cur = unused.poll();
            if (cur.weight < 0) {
                while (dig2char.get(right) != null) {
                    right--;
                }
                min += right-- * cur.weight;
            } else {
                while (dig2char.get(left) != null) {
                    left++;
                }
                min += left++ * cur.weight;
            }

            temp.add(cur);
        }
        unused.addAll(temp);
        return min;
    }

    // flag表示正负权重
    private void initParam(String cur, boolean flag) {
        int base = 1;
        for (int j = cur.length() - 1; j >= 0; j--) {
            char c = cur.charAt(j);
            // 统计权重
            if (flag) weight.put(c, weight.getOrDefault(c, 0) + base);
            else weight.put(c, weight.getOrDefault(c, 0) - base);
            // 最高位统计
            if (j == 0) {
                // 冲突则选择长的，单个字符的字符串可以为0，故以长的为主
                if (head.get(c) == null || head.get(c).length() < cur.length()) {
                    head.put(c, cur);
                }
            }
            base *= 10;
        }
        // 超过10个字母，无法分配唯一的数字
        if (weight.size() > 10) throw new IllegalArgumentException("invalid param");
    }
}

class Pair {
    char c;
    int weight;

    Pair(char c, int w) {
        this.c = c;
        this.weight = w;
    }
}
```

## 性能分析

&emsp;时间复杂度粗略统计为$O(10!)$，空间复杂度为$O(1)$，因为字符不超过10个。

执行用时：22ms，在所有java提交中击败了81.18%的用户。

内存消耗：40MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;上述思路参考官方剪枝策略。