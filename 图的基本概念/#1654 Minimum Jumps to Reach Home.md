[toc]

A certain bug's home is on the x-axis at position `x`. Help them get there from position $0$.

The bug jumps according to the following rules:

* It can jump exactly `a` positions **forward** (to the right).
* It can jump exactly `b` positions **backward** (to the left).
* It cannot jump backward twice in a row.
* It cannot jump to any `forbidden` positions.

The bug may jump forward **beyond** its home, but it **cannot jump** to positions numbered with **negative** integers.

Given an array of integers `forbidden`, where `forbidden[i]` means that the bug cannot jump to the position `forbidden[i]`, and integers `a`, `b`, and `x`, return the minimum number of jumps needed for the bug to reach its home. If there is no possible sequence of jumps that lands the bug on position `x`, return $-1$.

 

**Example 1**:

```
Input: forbidden = [14,4,18,1,15], a = 3, b = 15, x = 9
Output: 3
Explanation: 3 jumps forward (0 -> 3 -> 6 -> 9) will get the bug home.
```

**Example 2**:

```
Input: forbidden = [8,3,16,6,12,20], a = 15, b = 13, x = 11
Output: -1
```

**Example 3**:

```
Input: forbidden = [1,6,2,14,5,17,4], a = 16, b = 9, x = 7
Output: 2
Explanation: One jump forward (0 -> 16) then one jump backward (16 -> 7) will get the bug home.
```



**Constraints**:

* $1 \le \text{forbidden.length} \le 1000$
* $1 \le \text{a, b, forbidden[i]} \le 2000$
* $0 \le x \le 2000$
* All the elements in `forbidden` are distinct.
* Position `x` is not forbidden.



## 题目解读

&emsp;一只虫子可向前跳，也可向后跳，向后跳不能连续跳两次，求可到达指定位置的最小跳跃次数。

```java
class Solution {
    public int minimumJumps(int[] forbidden, int a, int b, int x) {

    }
}
```

## 程序设计

* 最基本的思路是使用广度优先搜索来计算，问题在于终结条件的判断，否则会无限循环；首先可使用贝祖定理初步筛选；由于虫子只能连续向后跳一次，则分如下情况：
  * 若$a \ge b$，每次只能后跳一次，即总体虫子必然是向前的，若超过$x + a$则无法回到$x$；
  * 若$a < b$，假设可以到达$x$，如果存在超出$x$的路径，假设超出$x$的第一个点为$P$，则$\lvert xP\rvert < a$，假设超出$x$的最后一个点是$Q$，此时$Q$可能直接跳回$x$，或者跳回到$x$前面在经过数次跳跃到达$x$；分析第二种情况，假设经过$m$次前跳和$n$次后跳到达$x$，$ma - nb > 0$，必然$m > n$，完全可以将这段操作加入到$Q$前的操作，这样把多种情况统一为$Q$回退到$x$一种情况，这样$\lvert xQ\rvert = b$；
  * 继续分析$a < b$的情况，上述分析可知$\lvert xQ\rvert = b$，$\lvert xP\rvert < a$，$P \to Q$的路径都在$x$右侧，由于$a < b$，$P$必然要向前跳动，若跳动超过$Q$则向后跳动，假设经过$m$次前跳和$n$次后跳到达$Q$，则$ma - nb > 0$，$m > n$，前跳的次数大于后跳的次数，可知上述思路完全可行，这样超过$x$的路径就限定在$b + a$这段；
* 得到限定条件后进行广度优先搜索，需要记录后跳的次数，避免连续两次后跳，同时遍历过的位置避免第二次遍历。

```java
class Solution {
    public int minimumJumps(int[] forbidden, int a, int b, int x) {
        // 贝祖定理检测
        if (x % gcd(a, b) != 0) return -1;
        
        Set<Integer> forbid = new HashSet<>(), visited = new HashSet<>();
        for (int num : forbidden) forbid.add(num);

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        int step = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cur = queue.poll();
                int pos = cur[0], backNum = cur[1];
				
                // 到达终点
                if (pos == x) return step;
                // 向前跳
                int forward = pos + a;
                if (forward <= 6000 && !forbid.contains(forward) && !visited.contains(forward)) {
                    queue.add(new int[]{forward, 0});
                    visited.add(forward);
                }
                // 向后跳
                int backward = pos - b;
                if (backNum == 0 && backward >= 0 && backward <= 6000 && !forbid.contains(backward) && !visited.contains(backward)) {
                    queue.add(new int[]{backward, 1});
                    visited.add(backward);
                }
            }
            step++;
        }
        return -1;
    }

    private int gcd(int a, int b) {
        return a == 0 ? b : gcd(b % a, a);
    }
}
```

* 上述思路测试用例不通过，由于到达的位置都需要记录到`visited`中，假设回退到$pos$，加入队列$[pos,1]$，并在`visited`中记录，同时可以前跳到$pos$，本来队列中加入$[pos,0]$，然而由于`visited`导致无法入队从而漏掉这种情况（`backNum == 0`会导致$[pos,1]$的情况无法再次回退，漏掉从$pos$回退的情况）；
* 每次从$pos$出发，$pos + a$和$pos - b$还需要多一个状态表示是回退还是前跳；换个思路，使用$pos + a$和$pos + a - b$由于只能回退一次，巧妙地表示了这两个状态，即使后续出现相同的值，状态也不相同（本次回退后续出现的是前跳）；

```java
class Solution {
    public int minimumJumps(int[] forbidden, int a, int b, int x) {
        if (x % gcd(a, b) != 0) return -1;
        
        Set<Integer> forbid = new HashSet<>(), visited = new HashSet<>();
        for (int num : forbidden) forbid.add(num);

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        int step = 1;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int pos = cur[0], count = cur[1];
            // 排重
            if (visited.contains(pos)) continue;
            visited.add(pos);

            if (pos == x) return count;
            // 前跳
            int forward = pos + a;
            if (forward <= 6000 && !forbid.contains(forward)) queue.add(new int[]{forward, count + 1});
                
            // 回退
            int backward = pos + a - b;
            if (backward >= 0 && backward <= 6000 && !forbid.contains(forward) && !forbid.contains(backward)) queue.add(new int[]{backward, count + 2});
        }
        return -1;
    }

    private int gcd(int a, int b) {
        return a == 0 ? b : gcd(b % a, a);
    }
}
```

## 性能分析

执行用时：30 ms, 在所有 Java 提交中击败了48.01%的用户。

内存消耗：38.9 MB, 在所有 Java 提交中击败了57.23%的用户。

## 官方解题

&emsp;暂无，密切关注。