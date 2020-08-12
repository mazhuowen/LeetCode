[toc]

Given two arrays `A` and `B` of equal size, the advantage of `A` with respect to `B` is the number of indices `i` for which `A[i] > B[i]`.

Return **any** permutation of `A` that maximizes its advantage with respect to `B`.



**Note:**

* $1 \le \text{A.length} = \text{B.length} \le 10000$
* $0 \le \text{A[i]} \le 10^9$
* $0 \le \text{B[i]} \le 10^9$



## 题目解读

&emsp;给定两个大小相等的数组`A`和`B`，`A`相对于`B`的优势可以用满足`A[i] > B[i]`的索引`i`的数目来描述。返回`A`的任意排列，使其相对于`B`的优势最大化。

```java
class Solution {
    public int[] advantageCount(int[] A, int[] B) {

    }
}
```

## 程序设计

* 参考官方思路，采用贪婪法，将两个数组排序，若当前位置分别为$a$和$b$：
  * $a > b$：由于已排序，此时$a$是`A`中大于$b$的最小值，即在数组`A`中$a$是最优解；在数组$B$中，假设$b^\prime$为$b$后的元素，且$a > b^\prime$，则$a$、$b^\prime$也是一组可选解，但考虑到在数组`A`中$a$后都是大于$a$的值，可以和$b^\prime$搭配，即将$a$与$b$搭配时得到的搭配对更多。综合分析，当遍历遇到$a > b$，就是一组解。
  * $a \le b$：此时无法搭配，且$a$小于`B`中剩余的数字，可将$a$保存，最后放入未搭配数字的任意位置。

```java
class Solution {
    public int[] advantageCount(int[] A, int[] B) {
        if (A == null || B == null || A.length != B.length) throw new IllegalArgumentException("invalid param");
        // 数组B索引
        int n = A.length;
        Integer[] idxes = new Integer[n];
        for (int i = 0; i < n; i++) idxes[i] = i;

        // 对数组A和数组B的索引排序
        Arrays.sort(A);
        Arrays.sort(idxes, (a, b) -> B[a] - B[b]);

        int[] res = new int[n];
        Arrays.fill(res, Integer.MAX_VALUE);
        int i = 0;
        // 保存不符合条件的值
        List<Integer> list = new ArrayList<>();
        for (int num : A) {
            int idx = idxes[i];
            // A中的数字大于B中的数字，加入对应索引
            if (num > B[idx]) {
                res[idx] = num;
                i++;
            } 
            // 不满足条件，加入链表
            else list.add(num); 
        }
        // 将不满足条件的值放入数组任意未填充位置
        i = 0;
        for (int j = 0; j < n; j++) {
            if (res[j] == Integer.MAX_VALUE) {
                res[j] = list.get(i++);
            }
        }
        return res; 
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：38ms，在所有java提交中击败了73.64%的用户。

内存消耗：42.1MB，在所有java提交中击败了77.03%的用户。

## 官方解题

&emsp;上述算法参考官方思路。