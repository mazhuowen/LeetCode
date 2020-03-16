[toc]

There are `N` workers.  The `i`-th worker has a `quality[i]` and a minimum wage expectation `wage[i]`.

Now we want to hire exactly `K` workers to form a paid group.  When hiring a group of K workers, we must pay them according to the following rules:

* Every worker in the paid group should be paid in the ratio of their quality compared to other workers in the paid group.
* Every worker in the paid group must be paid at least their minimum wage expectation.

Return the least amount of money needed to form a paid group satisfying the above conditions.



Note:

* $1 \le K \le N \le 10000$, where $N = \text{quality.length} = \text{wage.length}$
* $1 \le \text{quality[i]} \le 10000$
* $1 \le \text{wage[i]} \le 10000$
* Answers within $10^{-5}$ of the correct answer will be considered correct.



## 题目解读

&emsp;有`N`名工人，第$i$名工人的工作质量为`quality[i]`，其最低期望工资为`wage[i]`。现在雇佣`K`名工人组成一个工资组。对工资组中的每名工人，应当按其工作质量与同组其他工人的工作质量的比例来支付工资。工资组中的每名工人至少应当得到他们的最低期望工资。返回组成一个满足上述条件的工资组至少需要多少钱。

```java
class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int K) {

    }
}
```

## 程序设计

* 如果使用暴力法，只需每次选择一个工人，按照其期望薪资定义其它员工薪资，最后找出最小的组合，时间复杂度为$O(N^2)$，不符合题目规模要求。
* 注意到一个重要规律，即工人期望薪资除以效率得到的比值，如果一个工人`a`的比值比另一个工人`b`的比值大，则按照`a`的期望薪资计算`b`的薪资，则`b`的薪资是大于其期望薪资的；相反则低于`a`的期望薪资。
* 有了这个发现，可以按照比率排序，每个工人的前面的工人都可以满足薪资要求，这样问题转化为遍历的当前工人的前面的所有工人里面选择$k - 1$个，使得薪资总额最小。由于薪资按照当前工人计算，也就是前面$k - 1$个人的效率总和最小。
* 这部分可以使用最大堆实现。

```java
class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int K) {
        if(quality == null || quality.length < K || quality.length != wage.length) {
            throw new IllegalArgumentException("invalid param");
        }
        // 索引数组
        Integer[] index = new Integer[quality.length];
        for (int i = 0; i < quality.length; i++) {
            index[i] = i;
        }
        // 根据薪酬效益比来排序
        Arrays.sort(index, (a, b) -> Double.compare((double)wage[a] / quality[a], (double) wage[b] / quality[b]));
        // 最大堆，根据效益排序
        PriorityQueue<Integer> queue = new PriorityQueue<>((a, b) -> quality[b] - quality[a]);
        int quaSum = 0;
        double res = Double.MAX_VALUE;
        // idx的工人取最低薪资，其前的工人都可以满足薪资要求（由于薪酬效益比小于当前工人，薪资必然比最低薪资大）
        for(int idx : index) {
            // 加入队列，同时总的效益增加
            queue.add(idx);
            quaSum += quality[idx];
            // 超过K个人，删除效益最大的人（必然薪资也是最大的）
            // 此处可以会将当前入队的工人删除，没关系，因为此时的总薪酬必然不是最小（K个人，每个人薪资都超过最低薪资），不会更新到最终结果
            if(queue.size() > K) {
                quaSum -= quality[queue.poll()];
            }
            // 正好K个人，计算总的薪资
            if(queue.size() == K) {
                res = Math.min(res, quaSum * (double)wage[idx] / quality[idx]);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(Nlog_2N)$，空间复杂度为$O(N)$。

执行用时：38ms，在所有java提交中击败了42.86%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;上述思路借鉴官方解题。