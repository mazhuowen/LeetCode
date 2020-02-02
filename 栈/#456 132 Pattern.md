[toc]

Given a sequence of n integers $a_1, a_2, \dots, a_n$, a 132 pattern is a subsequence $a_i, a_j, a_k$ such that $i < j < k$ and $a_i < a_k < a_j$. Design an algorithm that takes a list of n numbers as input and checks whether there is a 132 pattern in the list.

Note: n will be less than 15,000.



## 题目解读

&emsp;根据给定的链表，判断是否有132模式，即三个值，第一个最小，第三个最大，这三个值不必相邻，但相对位置必须遵守链表顺序。

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        
    }
}
```

## 程序设计

* 考虑到$a_2$，其前面必须存在一个小于$a_2$的数$a_1$，$a_1$越小越好，因为$a_2$后面的$a_3$要大于$a_1$。可以给每个结点维护一个值表示其前面的最小值，如下图。这样查找$a_3$皆可以从表尾遍历：如果当前结点的最小值小于当前结点值，表示存在$a_1 < a_2$，进一步判断$a_3$。此时$a_2$后面的结点保存在栈中，从栈中遍历如果$a_1 >= a_3$显然不符合情况，删除；判断栈顶的$a_3$值是否小于$a_2$，是则存在132模式，否则将$a_2$入栈，继续遍历。

<img src="/project/LeetCode/images/#456.png"  />

* 由以上分析可知栈中是降序的，栈顶值小，栈底值大。以上图为例，结点20的最小值为3，满足$a_2 > a_1$，但栈为空，没有$a_3$，入栈；结点11也满足$a_2 > a_1$，遍历栈，栈中$a_3 > a_1$但$a_3 > a_2$，不满足要求，故入栈；同理4、6依次入栈；结点3由于不满足$a_2 > a_1$，直接跳过；结点12满足$a_2 > a_1$，此时$a_2 = 12$，$a_1 = 6$，遍历栈，栈中$a_3 = 4$小于$a_1$不满足条件，弹出4，同理弹出6；此时栈顶是11，满足所有条件，返回true。

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        if(nums == null || nums.length < 3) {
            return false;
        }
        // 记录当前位置的最小值
        int[] min = new int[nums.length];
        min[0] = nums[0];
        for(int i = 1; i < nums.length; i++) {
            min[i] = Math.min(min[i - 1], nums[i]);
        }
        Stack<Integer> stack = new Stack<>();
        // 记录了结点及其对应的最小值，意味着记录了a1、a2，需要从后面遍历找到a3 
        for(int i = nums.length - 1; i >= 0; i--) {
           // a2 > a1，查找a3
           if(nums[i] > min[i]) {
               // 首先删除栈中小于等于a1的值，使得剩余的栈中值都大于a1
               // 越往前遍历，结点对应的最小值会越大，此处删除的不满足条件的值，在前面的点也不满足条件，因为此处栈中的值都小于当前结点的最小值，而前面结点的最小值只会越大，更不满足条件
               while(!stack.isEmpty() && stack.peek() <= min[i]) {
                   stack.pop();
               }
               // a3 < a2，符合要求
               if(!stack.isEmpty() && stack.peek() < nums[i]) {
                   return true;
               }
               // 当前结点不符合要求，当前值都小于等于栈中的值，入栈
               stack.push(nums[i]);
           }
        }
        // 遍历结束，未找到
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：14ms，在所有java提交中击败了86.06%的用户。

内存消耗：40.6MB，在所有java提交中击败了8.10%的用户。

## 官方解题

&emsp;参考官方解题。