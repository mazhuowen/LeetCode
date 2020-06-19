[toc]

On the sea represented by a cartesian plane, each ship is located at an integer point, and each integer point may contain at most 1 ship.

You have a function `Sea.hasShips(topRight, bottomLeft)` which takes two points as arguments and returns `true` if and only if there is at least one ship in the rectangle represented by the two points, including on the boundary.

Given two points, which are the top right and bottom left corners of a rectangle, return the number of ships present in that rectangle.  It is guaranteed that there are **at most 10 ships** in that rectangle.

Submissions making **more than 400 calls** to `hasShips` will be judged Wrong Answer.  Also, any solutions that attempt to circumvent the judge will result in disqualification.



**Constraints**:

* On the input `ships` is only given to initialize the map internally. You must solve this problem "blindfolded". In other words, you must find the answer using the given `hasShips` API, without knowing the `ships` position.
* $0 \le \text{bottomLeft[0]} \le \text{topRight[0]} \le 1000$
* $0 \le \text{bottomLeft[1]} \le \text{topRight[1]} \le 1000$



## 题目解读

&emsp;

```java
/**
 * // This is Sea's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Sea {
 *     public boolean hasShips(int[] topRight, int[] bottomLeft);
 * }
 */

class Solution {
    public int countShips(Sea sea, int[] topRight, int[] bottomLeft) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    public int countShips(Sea sea, int[] topRight, int[] bottomLeft) {
        int a = topRight[0], b = topRight[1];
        int c = bottomLeft[0], d = bottomLeft[1];

        System.out.println(a + "==" + b);
        System.out.println(c + "==" + d);
        if (a < c || b < d || !sea.hasShips(topRight, bottomLeft)) return 0;
        if (a == c && b == d) return 1;

        int midX = (a + c) / 2, midY = (b + d) / 2;
        return countShips(sea, new int[]{midX, midY}, new int[]{c, d}) +
                countShips(sea, new int[]{a, midY}, new int[]{midX + 1, d}) +
                countShips(sea, new int[]{midX, b}, new int[]{c, midY + 1}) +
                countShips(sea, new int[]{a, b}, new int[]{midX + 1, midY + 1});
    }
}
```

## 性能分析

&emsp;

执行用时：42 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：39.7 MB, 在所有 Java 提交中击败了100.00%的用户

## 官方解题

&emsp;