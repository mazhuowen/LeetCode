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

* 暴力法两层循环计数小于当前值的数目，但是该方法会超时。

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new LinkedList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        for (int i = 0; i < nums.length - 1; i++) {
            int count = 0;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[i]) {
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

* 由于问题本质是逆序对问题，可以采用归并排序，在合并数组的步骤中做改动统计数目。

```java
class Solution {
    // 记录索引并排序索引（不动数据）
    private int[] idx;
    // 计数右侧小于当前值的数
    private int[] counter;
    // 临时排序数组
    private int[] temp;

    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new LinkedList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        // 记录索引并排序索引（不动数据）
        idx = new int[nums.length];
        // 计数右侧小于当前值的数
        counter = new int[nums.length];
        // 临时排序数组
        temp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            idx[i] = i;
        }
        mergeCount(nums, 0, nums.length - 1);
        for (int i = 0; i < nums.length; i++) {
            res.add(counter[i]);
        }
        return res;
    }

    private void mergeCount(int[] nums, int start, int end) {
        if (start >= end) {
            return;
        }
        //划分点
        int mid = start + (end - start) / 2;
        // 递归
        mergeCount(nums, start, mid);
        mergeCount(nums, mid + 1, end);
        // 合并
        // 记录起始索引
        int index = start;
        // 两个区间已排序，归并，更新前一个区间的数值的右侧较小数
        int i = start, j = mid + 1;
        // 记录当前点大于后半部分点的数目
        int count = 0;
        while (i <= mid && j <= end) {
            // 有序，i加上之前无序的数目即（start～i）大于（mid+1～j）的数目
            if (nums[idx[i]] <= nums[idx[j]]) {
                // 更新计数，此处保证了后半部分不再有值小于当前值，避免叠加更新
                counter[idx[i]] += count;
                // 放入temp中
                temp[index++] = idx[i++];
            } 
            // 无序，计数加一（注意此处不更新计数数组，避免叠加更新，如1、7、9和5、8，若在此处更新会造成9重复叠加计数）
            else {
                count++;
                temp[index++] = idx[j++];
            }
        }
        // 后面的序列已遍历完，前面还没遍历完，即前面的这部分大于后面所有的元素值
        while (i <= mid) {
            // 此时count肯定等于end-mid
            counter[idx[i]] += count;
            temp[index++] = idx[i++];
        }
        // 前面的已遍历完成，后面的还未完成，加入temp
        while (j <= end) {
            temp[index++] = idx[j++];
        }
        // 复制到idx
        for (int k = start; k <= end; k++) {
            idx[k] = temp[k];
        }
    }
}
```

* 可以继续优化上述代码，首先加入归并排序的提前判断结束，当前后是有序的时，不必继续排序；其次我们在更新计数时采用一个整形，可以不需要直接使用$j - mid - 1$代替，优化得：

```java
class Solution {
    // 记录索引并排序索引（不动数据）
    private int[] idx;
    // 计数右侧小于当前值的数
    private int[] counter;
    // 临时排序数组
    private int[] temp;

    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new LinkedList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        // 记录索引并排序索引（不动数据）
        idx = new int[nums.length];
        // 计数右侧小于当前值的数
        counter = new int[nums.length];
        // 临时排序数组
        temp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            idx[i] = i;
        }
        mergeCount(nums, 0, nums.length - 1);
        for (int i = 0; i < nums.length; i++) {
            res.add(counter[i]);
        }
        return res;
    }

    private void mergeCount(int[] nums, int start, int end) {
        if (start >= end) {
            return;
        }
        //划分点
        int mid = start + (end - start) / 2;
        // 递归
        mergeCount(nums, start, mid);
        mergeCount(nums, mid + 1, end);
        // 合并，（如果是有序的则直接返回）
        if (nums[idx[mid]] <= nums[idx[mid + 1]]) {
            return;
        }
        // 记录起始索引
        int index = start;
        // 两个区间已排序，归并，更新前一个区间的数值的右侧较小数
        int i = start, j = mid + 1;
        while (i <= mid && j <= end) {
            // 有序，i加上之前无序的数目即（start～i）大于（mid+1～j）的数目
            if (nums[idx[i]] <= nums[idx[j]]) {
                // 更新计数，此处保证了后半部分不再有值小于当前值，避免叠加更新
                counter[idx[i]] += j - mid - 1;
                // 放入temp中
                temp[index++] = idx[i++];
            } 
            // 无序，计数加一（注意此处不更新计数数组，避免叠加更新，如1、7、9和5、8，若在此处更新会造成9重复叠加计数）
            else {
                temp[index++] = idx[j++];
            }
        }
        // 后面的序列已遍历完，前面还没遍历完，即前面的这部分大于后面所有的元素值
        while (i <= mid) {
            // 此时count肯定等于end-mid
            counter[idx[i]] += end - mid;
            temp[index++] = idx[i++];
        }
        // 前面的已遍历完成，后面的还未完成，加入temp
        while (j <= end) {
            temp[index++] = idx[j++];
        }
        // 复制到idx
        for (int k = start; k <= end; k++) {
            idx[k] = temp[k];
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：7ms，在所有java提交中击败了89.50%的用户。

内存消耗：41.7MB，在所有java提交中击败了5.18%的用户。

## 官方解读

&emsp;思路同上。除了上述思路，社区还有一种方法巧妙的使用数组索引实现。首先介绍`val & -val`操作，当`val`为奇数时结果为1，当`val`为2的次方值时，结果就是`val`，当`val`为其它偶数时，结果是2。该方法利用这个特性，先将数组转化为大于0的数值，然后创建数组，索引表示当前右侧小于该数值的数目。该方法的核心在于更新，当遍历到`val`值后，如果更新以后所有的值，则是线性的，该方法利用`val & -val`操作将奇数化为偶数，将偶数化为2的次方数，这样最后的操作是对数级的。获取操作类似。

```java
class Solution {
    public  List<Integer> countSmaller(int[] nums) {
        if (nums.length == 0) {
            return new ArrayList<>();
        }
        // 将数组转化为正数数组
        int min = Integer.MAX_VALUE;
        for (int value : nums) {
            if (value < min) min = value;
        }
        for (int i = 0; i < nums.length; i++) nums[i] = nums[i] - min + 1;
        
        int max = Integer.MIN_VALUE;
        for (int value : nums) {
            if (value > max) max = value;
        }
        // 数组记录小于当前数的个数
        int[] record = new int[max + 1];
        // 初始化
        record[0] = 0;
        int[] countArr = new int[nums.length];
        // 从后往前遍历
        for (int i = nums.length - 1; i >= 0; i--) {
            // 获取小于num[i]的数的个数
            int count = getSum(nums[i] - 1, record);
            countArr[i] = count;
            // 维护num[i]
            update(nums[i], record);
        }
        List<Integer> result = new ArrayList<>();
        for (int value : countArr) {
            result.add(value);
        }
        return result;
    }
    
    private int getSum(int value, int[] record) {
        int sum = 0;
        while (value > 0) {
            sum += record[value];
            value -= (value & -value);
        }
        return sum;
    }
    // 更新策略是关键，大于value的数（从索引value开始加一，但此处巧妙的更新logN次）
    private void update(int value, int[] record) {
        while (value <= record.length - 1) {
            record[value] += 1;
            value += (value & -value);
        }
    }
}
```

&emsp;时间复杂度为$O(N\log_2(max - min))$，空间复杂度为$O(max - min)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.6MB，在所有java提交中击败了5.18%的用户。

> 除了上述思路，社区还有利用二叉查找树来做，但是因为二叉查找树的平衡性不能保证，时间复杂度不是最优。