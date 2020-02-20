[toc]

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.



## 题目解读

&emsp;给定当前版本，检查是否是错误版本，也就是其祖先版本是否错误。题目提供`isBadVersion`方法查询版本是否错误，需要尽量少调用这个API。

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        
    }
}
```

## 程序设计

* 就是判断版本n之前的版本是否存在错误版本。需注意正确版本前面必然都是正确版本。

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1, right = n;
        int mid;
        while(left < right) {
            mid = left + (right - left) / 2;
            // 如果是错误版本，向前继续查，right要么是最后一位，要么是错误版本
            if(isBadVersion(mid)) {
                right = mid;
            } 
            // mid之前的版本都是正确的，往后查
            else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

> 此题隐含n前一定有错误版本的条件。此外需注意`left+right`的溢出问题导致的死循环。

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：16ms，在所有java提交中击败了19.09%的用户。

内存消耗：36.2MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;和上面一致。