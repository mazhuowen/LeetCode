[toc]

The `i`-th person has weight `people[i]`, and each boat can carry a maximum weight of `limit`.

Each boat carries at most 2 people at the same time, provided the sum of the weight of those people is at most `limit`.

Return the minimum number of boats to carry every given person.  (It is guaranteed each person can be carried by a boat.)



**Note**:

- $1 \le \text{people.length} \le 50000$
- $1 \le \text{people[i]} \le \text{limit} \le 30000$



## 题目解读

&emsp;每个救生艇每次最多携带两人，总重量不超过`limit`。求所需救生艇的最小数目。

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {

    }
}
```

## 程序设计

* 由于每次载客只能载两人，显然根据贪婪法每次判断体重最大和最小的人，如果低于限制，则载两人，超过限制则载一人。

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int left = 0, right = people.length - 1;
        if (people[right] > limit) throw new IllegalArgumentException("invalid param");
        int res = 0;
        while (left < right) {
            // 载一人
            if (people[left] + people[right] > limit) right--;
            // 载两人
            else {
                left++;
                right--;
            }
            res++;
        }
        // 最后剩余一人
        if (left == right) res++;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：18ms，在所有java提交中击败了67.13%的用户。

内存消耗：47.5MB，在所有java提交中击败了43.05%的用户。

## 官方解题

&emsp;官方思路类似。