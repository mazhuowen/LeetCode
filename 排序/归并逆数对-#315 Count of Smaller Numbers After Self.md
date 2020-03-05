[toc]

You are given an integer array `nums` and you have to return a new `counts` array. The `counts` array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.



## 题目解读

&emsp;给定数组，统计每个数右侧小于其值的元素数目。

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        
    }
}
```

## 程序设计

* 暴力法两层循环计数小于当前值的数目。

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new LinkedList<>();
        if(nums == null || nums.length == 0) {
            return res;
        }
        for(int i = 0; i < nums.length - 1; i++) {
            int count = 0;
            for(int j = i + 1; j < nums.length; j++) {
                if(nums[j] < nums[i]) {
                    count++;
                }
            }
            res.add(count);
        }
        res.add(0);
        return res;
    }
}
```

* 上述方法最超时。

```java

```

## 性能分析



## 官方解读



