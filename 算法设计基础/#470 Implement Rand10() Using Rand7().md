[toc]

Given a function `rand7` which generates a uniform random integer in the range $1$ to $7$, write a function `rand10` which generates a uniform random integer in the range $1$ to $10$.

Do NOT use system's `Math.random()`.



**Note**:

* `rand7` is predefined.
* Each testcase has one argument: $n$, the number of times that `rand10` is called.



**Follow up**:

* What is the `expected value` for the number of calls to `rand7()` function?
* Could you minimize the number of calls to `rand7()`?



## 题目解读

&emsp;使用`rand7`实现`rand10`。

```java
/**
 * The rand7() API is already defined in the parent class SolBase.
 * public int rand7();
 * @return a random integer in the range 1 to 7
 */
class Solution extends SolBase {
    public int rand10() {
        
    }
}
```

## 程序设计

* 首先想到的是随机三次，将三个数相加，这样得到的数值范围在$3 \sim 21$区间内，除以$2$得到$1 \sim 10$。但这个思路是错误的，因为简单叠加后每个数字不是等概率的，比如$1$只能是三个$1$相加再除去$2$，而$5$则可以是$1 + 7 + 2$或$2 + 4 + 4$等，明显概率不再相等。
* 随机数相加不行则考虑相乘，产生$1 \sim 49$的数，都是等概率分布，可以拒绝大于$10$的数，直到随机相乘生成小于$10$的数。该方法需要多次循环生成，效率较低。可以考虑将数字取余，这样只需拒绝$41 \sim 49$的$9$个数字。

```java
class Solution extends SolBase {
    public int rand10() {
        // 为了使得概率均匀，随机数乘以常数7，得到的数据是均匀
        int num = (rand7() - 1) * 7 + rand7();
        // 大于40则拒绝采样
        while (num > 40) num = (rand7() - 1) * 7 + rand7();

        return 1 + num % 10;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。期望为$E = 2 * \frac{40}{49} + 4 * \frac{9}{49} * \frac{40}{49} + \dots$，即$E = 2 * \frac{40}{49}\sum_{i = 1}^\infty i * \frac{9}{49}^{i - 1}$，则：
$$
E - \frac{9}{49}E = 2 * \frac{40}{49}\sum_{i = 1}^\infty i * \frac{9}{49}^{i - 1} - 2 * \frac{40}{49}\sum_{i = 1}^\infty i * \frac{9}{49}^i\\
 = 2 * \frac{40}{49}\sum_{i = 0}^\infty (i + 1) * \frac{9}{49}^i - 2 * \frac{40}{49}\sum_{i = 1}^\infty i * \frac{9}{49}^i\\
 = 2 * \frac{40}{49} + 2 * \frac{40}{49}\sum_{i = 1}^\infty\frac{9}{49}^i = 2 * \frac{40}{49}\sum_{i = 0}^\infty\frac{9}{49}^i \\
  = 2 * \frac{40}{49} * \frac{1 * (1 - \frac{9}{49}^\infty)}{1 - \frac{9}{49}} = 2 * \frac{40}{49} * \frac{49}{40}\\
  \implies E = \frac{49}{20} = 2.45
$$
执行用时：7ms，在所有java提交中击败了76.23%的用户。

内存消耗：45.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方进一步优化，将拒绝的$9$个数乘$7$，得到$1 \sim 63$范围的数，此时将拒绝的$3$个数乘$7$得到$1 \sim 21$范围的数，只需拒绝$1$。

```java
class Solution extends SolBase {
    public int rand10() {
        while (true) {
            // 为了使得概率均匀，随机数乘以常数7，得到的数据是均匀
            int num = (rand7() - 1) * 7 + rand7();
            if (num <= 40) return 1 + num % 10;

            // 拒绝的9个数乘7得到63范围的数字
            num = (num - 40 - 1) * 7 + rand7();
            if (num <= 60) return 1 + num % 10;

            // 拒绝的3个数乘7得到21范围的数
            num = (num - 60 - 1) * 7 + rand7();
            if (num <= 20) return 1 + num % 10;
            // 最后拒绝1，继续循环
        }
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了98.82%的用户。

内存消耗：45.3MB，在所有java提交中击败了100.00%的用户。