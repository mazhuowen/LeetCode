[toc]

There are n bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the i-th round, you toggle every i bulb. For the n-th round, you only toggle the last bulb. Find how many bulbs are on after n rounds.



## 题目解读

&emsp;初始时有$n$个灯泡关闭。 第$1$轮打开所有的灯泡；第$2$轮每两个灯泡关闭一次；第3轮每三个灯泡切换一次开关（如果关闭则开启，如果开启则关闭）；第$i$轮每$i$个灯泡切换一次开关。找出$n$轮后有多少个亮着的灯泡。

```java
class Solution {
    public int bulbSwitch(int n) {

    }
}
```

## 程序设计

* 从$1$到$9$列举，发现$1$、$2$、$3$有一个，$4$、$5$、$6$、$7$、$8$有两个，$9$、$10$、$11$、$12$、$13$、$14$、$15$有三个；规律为$3$、$5$、$7$，验证可知$16$后$9$个数有四个，$25$后$11$个数有五个。从而得到如下算法：

```java
class Solution {
    public int bulbSwitch(int n) {
        if (n <= 0) return 0;
        int count = 0;
        // 依次减去3、5、7……
        for (int i = 3; n > i; i += 2) {
            n -= i;
            count++;
        }
        return count + 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.3MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区从数学的角度解决。每一轮$i$在前一轮的基础上增加了一个灯泡，终点在于这个灯泡究竟是亮还是暗。以$7$为例，可操作到$7$的轮数为$1$、$7$，有两个，最后灯是暗的；以$9$为例，可操作$9$的轮数为$1$、$3$、$9$，有三个，灯泡最后是亮的。更多例子表明操作轮数为因子，操作轮数为偶数则灯暗，奇数则灯亮。问题转化为因子的数目。又由于平方数的因子是奇数个，其它数因子都是偶数个，故只需统计$n$内的平方数有多少个。

```java
class Solution {
    public int bulbSwitch(int n) {
        return (int)Math.sqrt(n);
    }
}
```

&emsp;时间复杂度为$O(\sqrt{N})$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了50.00%的用户。