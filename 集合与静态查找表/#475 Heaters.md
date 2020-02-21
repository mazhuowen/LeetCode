[toc]

Winter is coming! Your first job during the contest is to design a standard heater with fixed warm radius to warm all the houses.

Now, you are given positions of houses and heaters on a horizontal line, find out minimum radius of heaters so that all houses could be covered by those heaters.

So, your input will be the positions of houses and heaters seperately, and your expected output will be the minimum radius standard of heaters.

Note:

* Numbers of houses and heaters you are given are non-negative and will not exceed 25000.
* Positions of houses and heaters you are given are non-negative and will not exceed $10^9$.
* As long as a house is in the heaters' warm radius range, it can be warmed.
* All the heaters follow your radius standard and the warm radius will the same.



## 题目解读

&emsp;设计一个有固定加热半径的供暖器向所有房屋供暖，即使得加热器的位置加上半径后可以覆盖所有房子位置。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        
    }
}
```

## 程序设计

* 仔细查看示例，可以发现最短半径是所有房子到热水器最近距离集合的最大值。如房屋位置为`[1,2,3,4]`，热水器位置为`[1,4]`，则房屋到热水器的最近距离为`[0,1,1,0]`，最大值为1就是最短半径。有了上述发现可以使用暴力法实现。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        int radius = 0;
        for(int i = 0; i < houses.length; i++) {
            int min = Integer.MAX_VALUE;
            for(int j = 0; j < heaters.length; j++) {
                // 更新房子与热水器的距离
                min = Math.min(min, Math.abs(houses[i] - heaters[j]));
            }
            // 更新半径
            radius = Math.max(radius, min);
        }
        return radius;
    }
}
```

* 可对上述暴力法优化，提前排序热水器，内层循环使用二分查找。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        // 排序
        Arrays.sort(heaters);
        int radiu = 0;
        // 遍历房屋查找最近距离
        for(int i = 0; i < houses.length; i++) {
            // 需处理特殊情况，即热水器为空（题目中返回最大整数）
            int min = heaters.length == 0 ? Integer.MAX_VALUE : binarySearch(heaters, houses[i]);
            radiu = Math.max(radiu, min);
        }
        return radiu;
    }

    // 查找target（若有则距离为0），否则返回右边界
    private int binarySearch(int[] heaters, int target) {
        int left = 0, right = heaters.length;
        while(left < right) {
            int mid = left + (right - left) / 2;
            // 最小距离为0
            if(heaters[mid] == target) {
                return 0;
            }
            // right持有右边界
            if(heaters[mid] > target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // 结束循环left为大于等于房子位置的热水器
        // 表示没有比target大的，取前一个小的值，之差就是最短距离
        if(left == heaters.length) {
            return target - heaters[left - 1];
        } else if(left - 1 >= 0) {
            return Math.min(target - heaters[left - 1], heaters[left] - target);
        } else {
            return heaters[left] - target;
        }
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：1780ms，在所有java提交中击败了5.76%的用户。

内存消耗：43.6MB，在所有java提交中击败了5.00%的用户。

&emsp;二分法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：20ms，在所有java提交中击败了38.56%的用户。

内存消耗：43.6MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区运行时间最快的方法采用排序两个数组然后比遍历比较的思路。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        // 排序
        Arrays.sort(houses);
        Arrays.sort(heaters);
        int radiu = 0;
        int i = 0, j = 0;
        while(i < houses.length) {
            int min = Integer.MAX_VALUE;
            // 处理特殊情况，热水器为空
            if(heaters.length == 0) {
                radiu = min;
                break;
            }
            // 找到上界，判断区间，根据区间更新值
            if(j < heaters.length && houses[i] <= heaters[j]) {
                min = Math.min(min, heaters[j] - houses[i]);
                if(j - 1 >= 0) {
                    min = Math.min(min, houses[i] - heaters[j - 1]);
                }
                radiu = Math.max(radiu, min);
                i++;
            } 
            // 没有上界，则取左边边界更新
            else if(j >= heaters.length) {
                min = Math.min(min, houses[i] - heaters[j - 1]); 
                radiu = Math.max(radiu, min);
                i++;
            } 
            // 继续迭代
            else {
                j++;
            }
        }
        return radiu;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：9ms，在所有java提交中击败了100.00%的用户。

内存消耗：43MB，在所有java提交中击败了5.28%的用户。