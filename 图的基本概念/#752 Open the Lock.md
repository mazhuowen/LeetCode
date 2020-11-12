[toc]

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or $-1$ if it is impossible.




**Note**:

* The length of `deadends` will be in the range `[1, 500]`.
* `target` will not be in the list `deadends`.
* Every string in `deadends` and the string `target` will be a string of 4 digits from the `10000` possibilities `'0000'` to `'9999'`.



## 题目解读

&emsp;初始密码为`0000`，求开锁的最短步骤数。

```java
class Solution {
    public int openLock(String[] deadends, String target) {

    }
}
```

## 程序设计

* 基本思路是利用广度优先搜索，需注意题目陷阱，即每次密码可增加也可减少。

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> deads = new HashSet<>();
        for (String deadend : deadends) deads.add(deadend);

        if (deads.contains("0000") || deads.contains(target)) return -1;

        int step = 0;
        Set<String> visited = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        queue.add("0000");
        visited.add("0000");

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String cur = queue.poll();
                if (target.equals(cur)) return step;

                for (int j = 0; j < 4; j++) {
                    String plus = plusOne(cur, j);
                    if (!visited.contains(plus) && !deads.contains(plus)) {
                        queue.add(plus);
                        visited.add(plus);
                    }
                    
                    String minus =  minusOne(cur, j);
                    if (!visited.contains(minus) && !deads.contains(minus)) {
                        queue.add(minus);
                        visited.add(minus);
                    }
                }
            }
            step++;
        }
        return -1;
    }

    private String plusOne(String s, int idx) {
        char[] strs = s.toCharArray();
        if (strs[idx] == '9') strs[idx] = '0';
        else strs[idx]++;
        return new String(strs);
    }

    private String minusOne(String s, int idx) {
        char[] strs = s.toCharArray();
        if (strs[idx] == '0') strs[idx] = '9';
        else strs[idx]--;
        return new String(strs);
    }
}
```

## 性能分析

&emsp;时间复杂度最坏$O(4^{10} \times 4^2)$，其中$4^{10}$为组合数目，$4^2$为字符串遍历及生成新字符串；空间复杂度为$O(4^{10} \times 4)$。

执行用时：101ms，在所有java提交中击败了61.26%的用户。

内存消耗：43.5MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;官方思路同上。参考社区时间性能较好的方法，采用双向广度优先搜索。

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> deads = new HashSet<>();
        for (String deadend : deadends) deads.add(deadend);

        if (deads.contains("0000") || deads.contains(target)) return -1;

        int step = 0;
        Set<String> visited = new HashSet<>();
        Set<String> queue1 = new HashSet<>();
        Set<String> queue2 = new HashSet<>();
        queue1.add("0000");
        queue2.add(target);

        while (!queue1.isEmpty() && !queue2.isEmpty()) {
            Set<String> temp = new HashSet<>();
            for (String cur : queue1) {
                if (queue2.contains(cur)) return step;

                visited.add(cur);
                for (int j = 0; j < 4; j++) {
                    String plus = plusOne(cur, j);
                    if (!visited.contains(plus) && !deads.contains(plus)) temp.add(plus);
                    
                    String minus =  minusOne(cur, j);
                    if (!visited.contains(minus) && !deads.contains(minus)) temp.add(minus);
                }
            }

            step++;
            // 交换
            if (temp.size() > queue2.size()) {
                queue1 = queue2;
                queue2 = temp;
            } else {
                queue1 = temp;
            }
        }
        return -1;
    }

    private String plusOne(String s, int idx) {
        char[] strs = s.toCharArray();
        if (strs[idx] == '9') strs[idx] = '0';
        else strs[idx]++;
        return new String(strs);
    }

    private String minusOne(String s, int idx) {
        char[] strs = s.toCharArray();
        if (strs[idx] == '0') strs[idx] = '9';
        else strs[idx]--;
        return new String(strs);
    }
}
```

执行用时：33ms，在所有java提交中击败了91.49%的用户。

内存消耗：40.1MB，在所有java提交中击败了44.44%的用户。