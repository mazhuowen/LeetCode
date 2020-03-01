[toc]

Given a **non-empty** array of integers, every element appears twice except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?



## 题目解读

&emsp;给定的数组中除了一个数，其它数都出现两次。找到这个数，题目限定了时间复杂度和空间复杂度。

```java
class Solution {
    public int singleNumber(int[] nums) {

    }
}
```

## 程序设计

* 可以使用集合保存出现一次的数字，如果数字第二次出现则从集合删去。这样最后集合中只剩一个数字就是待求解的。

```java
class Solution {
    public int singleNumber(int[] nums) {
        Set<Integer> counter = new HashSet<>();
        for(int num : nums) {
            if(counter.contains(num)) {
                counter.remove(num);
            } else {
                counter.add(num);
            }
        }
        for(int k : counter) {
            return k;
        }
        // 不会走到这儿
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了29.27%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;官方提供了数学的方式，第一种方式是得到数组的集合，然后集合中的数字之和乘2减去数组之和得到的就是结果，java实现需要注意整数溢出。第二种方式采用亦或的方式：相同的数字亦或结果为0，0与任意数字亦或得到的是数字本身。利用这个特性，遍历数组并亦或即可。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int res = 0;
        for(int num : nums) {
            res ^= num;
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.60%的用户。

内存消耗：42.2MB，在所有java提交中击败了5.01%的用户。