[toc]

Given an integer array, return the k-th smallest **distance** among all the pairs. The distance of a pair `(A, B)` is defined as the absolute difference between A and B.



**Note**:

* $2 \le \text{len(nums)} \le 10000$.
* $0 \le \text{nums[i]} < 1000000$.
* $1 \le k \le \text{len(nums) * (len(nums) - 1) / 2}$.



## 题目解读

&emsp;给定整数数组，返回第$k$小的距离。

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {

    }
}
```

## 程序设计

* 最容易想到的是双层循环然后计算距离并入队，最后返回堆顶元素。但是该方法会超时，时间复杂度为$O(N^2\log_2k)$。

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        // 记录距离，采用容量为k的最大堆
        PriorityQueue<Integer> queue = new PriorityQueue<>(k, (a, b) -> b - a);
        for(int i = 0; i < nums.length - 1; i++) {
            for(int j = i + 1; j < nums.length; j++) {
                int distance = nums[i] - nums[j] > 0 ? nums[i] - nums[j] : nums[j] - nums[i];
                // 判断
                if(queue.size() == k) {
                    if(distance < queue.peek()) {
                        queue.poll();
                        queue.add(distance);
                    }
                } else {
                    queue.add(distance);
                }
            }
        }
        // 返回最大堆堆顶元素
        return queue.poll();
    }
}
```

* 由于上面两个循环，可以继续优化。可以先排序，然后入最小堆，通过出队时的扩展操作得到结果。时间复杂度为$O(N\log_2N)$，但是该方法还是超时。

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        // 最小堆
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> {
            return Math.abs(nums[a[1]] - nums[a[0]]) - Math.abs(nums[b[1]] - nums[b[0]]);
        });
        // 排序
        Arrays.sort(nums);
        // 入队(n-1个)
        for(int i = 0; i < nums.length - 1; i++) {
            queue.add(new int []{i, i + 1});
        }
        // 边出队边扩展
        while(k > 1) {
            int[] temp = queue.poll();
            if(temp[1] + 1 < nums.length) {
                temp[1] += 1;
                queue.add(temp);
            }
            k--;
        }
        int dis = Math.abs(nums[queue.peek()[1]] - nums[queue.poll()[0]]);
        return dis;
    }
}
```

* 针对上述超时算法，官方巧妙的使用二分查找来处理，二分查找距离区间内是否有大于等于k个，返回大于等于k的最小值，否则缩小区间继续查找。实质上转化为二分查找第一个大于等于k的值的问题。

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        // 距离最小为0，最大为最大值减最小值，在这个区间二分查找
        int left = 0, right = nums[nums.length - 1] - nums[0];
       
        while(left < right) {
            int mid = left + (right - left) / 2;
            // 统计0～mid区间有几个值
            int count = 0;
            // i为左边界，j为右边界
            int j = 0;
            for(int i = 0; i < nums.length; i++) {
                // 定位到区间j～i，使得区间内的距离小于等于mid（如果不存在则i=j）
                while(nums[i] - nums[j] > mid) j++;
                // 计数
                count += i - j;
            }
            if(count < k) {
                left = mid + 1;
            } 
            // right保持大于等于k的边界
            else {
                right = mid;
            }
        }
        return left;
    }
}
```

## 性能分析

&emsp;排序花费$O(N\log_2N)$，二分查找外层时间复杂度为$\log_2M$，$M$最大距离，二分查找内部虽然是两个循环，但是两个循环是一起遍历的，时间复杂度为$O(N)$，最终时间复杂度为$O(N\log_2N)$和$O(N\log_2M)$中的较大值。空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.3MB，在所有java提交中击败了5.79%的用户。

## 官方解题

&emsp;见上。