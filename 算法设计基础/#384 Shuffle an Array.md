[toc]

Shuffle a set of numbers without duplicates.



## 题目解读

&emsp;设计洗牌算法。

```java
class Solution {

    public Solution(int[] nums) {

    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {

    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {

    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```

## 程序设计

* 最基本的思路是随机交换随机数个位置，但这样有缺陷，没法交换每一个位置的牌。

```java
class Solution {
    int size;
    int[] origin;
    int[] shuffle;
    Random random;

    public Solution(int[] nums) {
        this.size = nums.length;
        this.origin = nums;
        this.shuffle = Arrays.copyOf(nums, nums.length);
        this.random = new Random();
    }
    
    public int[] reset() {
        return origin;
    }
    
    public int[] shuffle() {
        int count = 0;
        // 随机交换随机数个位置的元素
        while (count < size) {
            swap(shuffle, random.nextInt(size), random.nextInt(size));
            // 随机张牌
            count += random.nextInt(size + 1);
        }
        return shuffle;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

* 从数学的角度考虑，不同的牌的组合有$O(N!)$种，上述暴力随机可能抽到重复牌，且不能保证每种组合出现的概率相等。Fisher–Yates算法利用抽牌的次序，在每次迭代中，生成一个范围在当前下标到数组末尾元素下标之间的随机整数，然后将当前元素和随机选出的下标所指的元素互相交换，这一步模拟了每次从剩余的牌中选择一张的过程；而抽到的牌不能再次被抽到。

```java
class Solution {
    int size;
    int[] origin;
    Random random;

    public Solution(int[] nums) {
        this.size = nums.length;
        this.origin = nums;
        this.random = new Random();
    }
    
    public int[] reset() {
        return origin;
    }
    
    public int[] shuffle() {
        int[] shuffle = Arrays.copyOf(origin, size);
        // 从n张牌开始，抽取到最后一张为止
        for (int i = size - 1; i >= 0; i--) {
            // 从n张牌中随机抽一张，将抽到的牌放到结尾，避免造成重复抽取，从而每一张牌都参与到抽牌过程中
            swap(shuffle, random.nextInt(i + 1), i);
        }
        return shuffle;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

## 性能分析

&emsp;基本思路时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：100ms，在所有java提交中击败了86.13%的用户。

内存消耗：47.7MB，在所有java提交中击败了100.00%的用户。

&emsp;洗牌算法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：96ms，在所有java提交中击败了91.48%的用户。

内存消耗：47.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。