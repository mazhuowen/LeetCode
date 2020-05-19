[toc]

Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?



## 



```java
class Solution {
    public int singleNumber(int[] nums) {

    }
}	
```

##




```java
class Solution {
    public int singleNumber(int[] nums) {
        long mask = 1;
        int res = 0, count = 0, epoch = 0;

        while (epoch < 32) {
            for (int num : nums) {
                if ((num & mask) == mask) count++;
            }
            // 说明此位为1
            if (count % 3 == 1) res |= mask;
            mask <<= 1;
            epoch++;
            count = 0;
        }
        return res;
    }
}
```

##



执行用时 :4ms,在所有java提交中击败了47.15%的用户

内存消耗 :39.6MB, 在所有java提交中击败了14.29%的用户

##