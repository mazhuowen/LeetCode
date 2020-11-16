[toc]

Suppose you are at a party with `n` people (labeled from `0` to `n - 1`) and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`. There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.



**Note**:

* The directed graph is represented as an adjacency matrix, which is an $n \times n$ matrix where `a[i][j] = 1` means person `i` knows person `j` while `a[i][j] = 0` means the contrary.
* Remember that you won't have direct access to the adjacency matrix.

## 题目解读

&emsp;判断是否有名人，名人定义为其他人（包括自己）都知道，但不知道其他人。

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        
    }
}
```

## 程序设计

* 根据逻辑，首先名人是不知道其它任何人的，可以根据这个条件找到符合的人选；其次其他人都知道名人，将上面的候选和其他人做比较，如果存在不知道该候选的情况，则说明不存在名人，因为该候选不是名人，但是不知道任何人，这与非名人知道名人矛盾。

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */
public class Solution extends Relation {
    public int findCelebrity(int n) {
        // 表示是否知道其它任何一个人
        boolean[] konw = new boolean[n];

        // 查找不知道其他人的人为候选名人
        int celebrity = -1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) continue;
                if (knows(i, j)) {
                    konw[i] = true;
                    break;
                }
            }
            // i不知道其它任何人，是名人的候选
            if (!konw[i]) {
                celebrity = i;
                break;
            }
        }
        if (celebrity == - 1) return -1;
        // 如果该候选直到其它某个人，则不是名人，不存在名人
        for (int i = 0; i < n; i++) {
            if (!knows(i, celebrity)) return -1;
        }
        return celebrity;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：32ms，在所有java提交中击败了34.52%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区比较巧妙的方法如下：

```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        // 查找候选人
        int candidate = 0;
        // 候选人不知道其他任何人（此处为不知道其后的任何人）
        for (int i = 1; i < n; i++) {
            if (knows(candidate, i)) candidate = i;
        }
		// 验证其前的部分
        for (int j = 0; j < candidate; j++) {
            if (knows(candidate, j)) return -1;
        }
        
        // 验证候选人
        for (int j = candidate + 1; j < n; j++) {
            // 候选人被其他任何人都知道
            if (!knows(j, candidate)) return -1;
        }
        return candidate;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：20ms，在所有java提交中击败了70.41%的用户。

内存消耗：39.8MB，在所有java提交中击败了100.00%的用户。