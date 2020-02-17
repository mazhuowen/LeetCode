[toc]

You are given two integer arrays **nums1** and **nums2** sorted in ascending order and an integer **k**.

Define a pair `(u,v)` which consists of one element from the first array and one element from the second array.

Find the k pairs `(u1,v1),(u2,v2) ...(uk,vk)` with the smallest sums.



## 题目解读

&emsp;给定两个有序数组，得到$k$对数，使得和最小。如`[1,7,11]`、`[2,4,6]`，$k=3$，则`[1,2]`、`[1,4]`、`[1,6]`三对和最小。

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        
    }
}
```

## 程序设计

* 题目没有要求元素重复等，只要求得到$k$个最小的数对，可将两个数组的数对优先级队列，最终得到最小的$k$个。

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int count = 0;
        // 最大堆
        PriorityQueue<int[]> queue = new PriorityQueue<>(k, (a, b) -> b[2] - a[2]);
        for(int i = 0; i < nums1.length; i++) {
            for(int j = 0; j < nums2.length; j++) {
                int sum = nums1[i] + nums2[j];
                // 入队
                if(queue.size() < k) {
                    queue.add(new int[]{i, j, sum});
                } 
                // 堆已经是k个，判断
                else {
                    if(sum < queue.peek()[2]) {
                        queue.poll();
                        queue.add(new int[]{i, j, sum});
                    }
                }
            }
        }
        // 拼接
        List<List<Integer>> res = new LinkedList<>();
        while(!queue.isEmpty()) {
            int[] cur = queue.poll();
            List<Integer> temp = new LinkedList<>();
            temp.add(nums1[cur[0]]);
            temp.add(nums2[cur[1]]);
            res.add(temp);
        }
        Collections.reverse(res);
        return res;
    }
}
```

## 性能分析

&emsp;由于遍历两个数组入队，时间复杂度为$O(MN\log_2k)$；空间复杂度为$O(k)$。

执行用时：39ms，在所有java提交中击败了52.96%的用户。

内存消耗：48.7MB，在所有java提交中击败了26.65%的用户。

## 官方解题

&emsp;暂无，密切关注。社区性能较好的方法使用最小堆。由于两个数组是有序数组，不必两个循环，可以优化为一个循环，最小和一定是两个数组的一个元素之和，次小的是数组1的第一个元素与数组2的第二个元素、数组1的第二个元素和数组2的第一个元素等的组合，可以先将数组1与数组2位置0的组合全部入队，后续出队迭代。

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<List<Integer>> res = new LinkedList<>();
        int n1 = nums1.length, n2 = nums2.length;
        if(n1 == 0 || n2 == 0 || k == 0) {
            return res;
        }
        PriorityQueue<int[]> queue = new PriorityQueue<>(k, (a, b) -> nums1[a[0]] + nums2[a[1]] - nums1[b[0]] - nums2[b[1]]);
        // 一遍循环入队，假定和数组1匹配的是数组2索引为0的元素
        for(int i = 0; i < n1; i++) {
            queue.add(new int[]{i, 0});
        }
        while(!queue.isEmpty() && k > 0) {
            int[] cur = queue.poll();
            // 关键地方，将数组2的索引加一入堆
            if(cur[1] + 1 < n2) {
                queue.add(new int[]{cur[0], cur[1] + 1});
            }
            List<Integer> temp = new LinkedList<>();
            temp.add(nums1[cur[0]]);
            temp.add(nums2[cur[1]]);
            res.add(temp);
            k--;
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(k)$。

执行用时：6ms，在所有java提交中击败了91.40%的用户。

内存消耗：45MB，在所有java提交中击败了26.95%的用户。