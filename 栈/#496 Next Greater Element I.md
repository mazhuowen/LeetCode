[toc]

You are given two arrays (**without duplicates**) `nums1` and `nums2` where `nums1`'s elements are subset of `nums2`. Find all the next greater numbers for `nums1`'s elements in the corresponding places of `nums2`.

The Next Greater Number of a number x in `nums1` is the first greater number to its right in `nums2`. If it does not exist, output -1 for this number.



Note:

* All elements in `nums1` and `nums2` are unique.
* The length of both `nums1` and `nums2` would not exceed 1000.



## 题目解读

&emsp;给定不重复整数数组`num2`，及其子集构成的数组`num1`，返回`num1`中的数在`num2`中下一个比它大的数的索引，没有则是-1。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        
    }
}
```

## 程序设计

* 遍历数组`nums2`，设计一个栈，当当前数字大于栈顶元素时，当前数字就是栈顶数字的较大后继，可以创建字典将这个关系维护起来，栈顶出栈加入字典；依次类推，栈中比当前数字小的数字都出栈加入字典；继续遍历直到遍历结束。遍历结束后若栈中还有数字，这些数字没有较大后继，统一赋值为-1。
* 在得到`num2`中所有数字的较大后继后，遍历`num1`得到结果。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Stack<Integer> stack = new Stack<>();
        // 维护数字与比其大的后继数字关系
        Map<Integer, Integer> map = new HashMap<>();
        for(int cur : nums2) {
            // 弹出比当前数字小的数字，并维护进字典
            while(!stack.isEmpty() && stack.peek() < cur) {
                map.put(stack.pop(), cur);
            }
            // 入栈
            stack.push(cur);
        }
        // 剩余数字后续没有比它大的
        while(!stack.isEmpty()) {
            map.put(stack.pop(), -1);
        }
        
        int[] index = new int[nums1.length];
        for(int i = 0; i < nums1.length; i++) {
            index[i] = map.get(nums1[i]);
        }
        return index;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：5ms，在所有java提交中击败了65.78%的用户。

内存消耗：37.1MB，在所有java提交中击败了39.24%的用户。

## 官方解题

&emsp;思路同上。

> 社区一些时间性能较好的方法没有使用数据结构，首先在数组2遍历找到数组1中数字所在，然后遍历数组中2找到其较大后继。