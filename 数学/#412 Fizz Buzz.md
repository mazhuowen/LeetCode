[toc]

Write a program that outputs the string representation of numbers from $1$ to $n$.

But for multiples of three it should output `"Fizz"` instead of the number and for the multiples of five output `"Buzz"`. For numbers which are multiples of both three and five output `"FizzBuzz"`.



## 题目解读

&emsp;3与5及其公倍数需要替换为特定字符串，其它都为数字本身。

```java
class Solution {
    public List<String> fizzBuzz(int n) {

    }
}
```

## 程序设计

* 先填充所有值，再填充指定的数字。

```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            res.add(Integer.toString(i + 1));
        }

        // 填充3、5、及公倍数15
        for (int i = 3; i <= n; i += 3) {
            res.set(i - 1, "Fizz");
        }
        for (int i = 5; i <= n; i += 5) {
            res.set(i - 1, "Buzz");
        }
        for (int i = 15; i <= n; i += 15) {
            res.set(i - 1, "FizzBuzz");
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.1MB，在所有java提交中击败了8.70%的用户。

## 官方解题

&emsp;官方提供的思路在循环内判断。