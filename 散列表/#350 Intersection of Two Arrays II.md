[toc]

Given two arrays, write a function to compute their intersection.

Note:

* Each element in the result should appear as many times as it shows in both arrays.
* The result can be in any order.

Follow up:

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?



## 题目解读

&emsp;查找两个数组中都有的元素，不去重。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        
    }
}
```

## 程序设计

* 采用哈希表记录计数。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if(nums1 == null || nums1.length == 0 
            || nums2 == null || nums2.length == 0) {
                return new int[]{};
        }
        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> list = new LinkedList<>();
        for(int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        for(int num : nums2) {
            if(map.getOrDefault(num, 0) > 0) {
                map.put(num, map.getOrDefault(num, 0) - 1);
                list.add(num);
            }
        }
        int[] res = new int[list.size()];
        for(int i = 0; i < res.length; i++) {
            res[i] = list.get(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了41.18%的用户。

内存消耗：39.7MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;同上。对于拓展，如果是有序的，可以两个数组同时遍历；如果某个数组特别小，则将其值统计如哈希表，遍历较大的表；如果一个数组在硬盘中，不能一次加载进内存，还是记录另一个数组进哈希表，然后分批读取比较。