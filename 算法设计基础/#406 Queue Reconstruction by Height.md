[toc]

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers `(h, k)`, where `h` is the height of the person and `k` is the number of people in front of this person who have a height greater than or equal to `h`. Write an algorithm to reconstruct the queue.

Note:
The number of people is less than `1100`.



## 题目解读

&emsp;假设有打乱顺序的一群人站成一个队列。每个人由一个整数对`(h, k)`表示，其中`h`是这个人的身高，`k`是排在这个人前面且身高大于或等于`h`的人数。编写一个算法来重建这个队列。

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {

    }
}
```

## 程序设计

* 队列起始必然是计数为$0$，高度最低的人，找到这个人，然后出发判断符合条件的人，在这些候选者中选择则高度最低的人。以`[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]`为例，起始必然是`[5,0]`，其后符合条件的人有`[7,0]`，选择，继续查找；其后符合条件的候选者有`[7,1]`、`[6,1]`、`[5,2]`，选择个子矮的`[5,2]`；继续查找选择`[6,1]`；继续查找选择`[4,4]`，最后选择`[7,1]`。
* 时间复杂度为$O(N^3)$会超时。

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if (people == null || people.length == 0) return people;

        // 查找起始人（在计数为0中身高最低的人）
        for (int i = 0; i < people.length; i++) {
            boolean flag = true;
            for (int j = i; j < people.length; j++) {
                if (count(people, i, people[j][0]) == people[j][1]) {
                    if (flag || people[j][0] < people[i][0]) {
                        swap(people, i, j);
                        flag = false;
                    }
                }
            }
        }
        return people;
    }

    private int count(int[][] people, int end, int target) {
        int count = 0;
        for (int i = 0; i < end; i++) {
            if (people[i][0] >= target) count++;
        }
        return count;
    }

    private void swap(int[][] people, int i, int j) {
        int[] temp = people[i];
        people[i] = people[j];
        people[j] = temp;
    }
}
```

* 官方根据身高降序计数增序排序，将身高较高的人先插入维护位置，然后将身高较低的人插入，不影响之前插入的较高的人的计数。

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if (people == null || people.length == 0) return people;

        Arrays.sort(people, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);

        List<int[]> temp = new LinkedList<>();
        for (int[] p : people) temp.add(p[1], p);
        return temp.toArray(new int[people.length][2]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：10ms，在所有java提交中击败了68.19%的用户。

内存消耗：41.1MB，在所有java提交中击败了83.33%的用。

## 官方解题

&emsp;上述思路参考官方。