[toc]

Given two arrays, write a function to compute their intersection.

Note:

* Each element in the result must be unique.
* The result can be in any order.



## 题目解读

&emsp;查找两个数组中都包含的数。

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        
    }
}
```

## 程序设计

* 可以使用集合保存一个数组的值，遍历另一个数组的值比较并加入链表。

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if(nums1 == null || nums1.length == 0 
            || nums2 == null || nums2.length == 0) {
                return new int[]{};
        }
        Set<Integer> set = new HashSet<>();
        List<Integer> list = new LinkedList<>();
        for(int num : nums1) {
            set.add(num);
        }
        for(int num : nums2) {
            if(set.contains(num)) {
                set.remove(num);
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

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，$N$为两个数组中较大的长度值。

执行用时：4ms，在所有java提交中击败了66.35%的用户。

内存消耗：39.5MB，在所有java提交中击败了5.13%的用户。

## 官方解题

&emsp;思路同上，采用了两个集合的方式，然后调用`retainAll`方法。