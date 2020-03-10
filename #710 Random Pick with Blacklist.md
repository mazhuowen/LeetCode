[toc]





## 题目解读



```java

```

## 程序设计

* 最基本的思路是维护白名单，但是会内存超出题目限制。

```java
class Solution {
    private List<Integer> whiteList;
    private Random r;

    public Solution(int N, int[] blacklist) {
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < N; i++) {
            set.add(i);
        }
        for(int b : blacklist) {
            set.remove(b);
        }
        this.whiteList = new ArrayList<>(set);
        this.r = new Random();
    }
    
    public int pick() {
        return whiteList.get(r.nextInt(whiteList.size()));
    }
}
```

* 可以换个思路，已知总数和黑名单数目，则可以知道剩余的白名单数目，假设黑名单数目为$M$，则选择白名单数目小于$N - M$，可以随机生成小于$N - M$的数，假设为$k$。则我们需要查询第$k$个白名单数。假设一个不重复且连续的序列，其索引等于数值。若存在重复且不连续，如果当前位置的值大于索引值，则表示前面的区域必然有空余，小于则表示后半段必然有缺失。
* 根据这个思路，如果当前值减去索引大于等于$k$表示前面前面区间有超过$k$个缺失值，小于则需要在后半区间查找。可使用二分查找。最后查找到的界限表示前面有$k$个缺失值的最小索引，记为索引$i$，$i$前面的索引$i-1$存在小于$k$个缺失值，则第$k$个黑名单缺失值必然在$num[i-1]$和$num[i]$之间；此时$num[i - 1] - i + 1 - k < 0$，而$num[i] - i - k = 0$，即$num[i]$前的一个值就是$i + k - 1$，也就是答案。
* 还需要考虑二分查找的边界问题。

```java
class Solution {
    private int n;
    private int[] blackList;
    private Random r;

    public Solution(int N, int[] blacklist) {
        this.n = N;
        this.r = new Random();
        // 排序
        Arrays.sort(blacklist);
        this.blackList = blacklist;
    }
    
    public int pick() {
        // 随机数[0, N - M)，加1表示缺失的第[0, N -M]个数
        int k = r.nextInt(n - blackList.length) + 1;
        int left = 0, right = blackList.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 如果不重复且连续，则此时索引处的数值和索引一致；如果存在重复且不连续，nums[mid] - mid大于0表示前半段有缺失，否则后半段有缺失，不密集
            // 如果nums[mid] - mid 大于等于k，表示前半段至少缺失k个值，否则后半段缺失k个值
            if(blackList[mid] - mid - k >= 0) {
                right = mid;
            }
            // 后半段缺失k个值
            else {
                left = mid + 1;
            }
        }
        // 黑名单整体缺失数值小于k，此处left为黑名单长度，黑名单最大值小于left - 1 +ｋ
        // 由于k范围是[1, size]，则k + left - 1不超过[size, N-1]，符合要求，是可行解
        if(left == blackList.length) {
            return k + left - 1;
        }
        // 最后得到的left表示黑名单该位置前的区间缺失k个值
        else {
            // 最小值等于k，表示所有黑名单的值都大于等于k，此时在位置0缺失的数值就是从0～k-1，即第k个数值就是k-1
            if(left == 0) {
                return k - 1;
            }
            // 此时left为第一个满足b[left] = left + k条件的值，说明b[left - 1] < left - 1 + k，left和left-1之间缺失的就是left+k-1，返回即可
            else {
                return k + left - 1;
            }
        }
    }
}
```

> 上述思路在不存在重复值的情况下，可以精准的查到缺失的第$k$个值，此时数组中的每个值大于等于索引，且值与索引的差递增；如果存在重复值，若还能保持值小于等于索引则也能得到准确的第$k$个数，如在mid处有值大于索引的存在，如`0,2,2,2`，缺失1、3、4、5，则二分只会返回4或5。但是由于$k$是随机的，符合题目条件。

* 可将上面二分后的返回操作优化为：

```java
public int pick() {
        // 随机数[0, N - M)，加1表示缺失的第[0, N -M]个数
        int k = r.nextInt(n - blackList.length) + 1;
        int left = 0, right = blackList.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 如果不重复且连续，则此时索引处的数值和索引一致；如果存在重复且不连续，nums[mid] - mid大于0表示前半段有缺失，否则后半段有缺失，不密集
            // 如果nums[mid] - mid 大于等于k，表示前半段至少缺失k个值，否则后半段缺失k个值
            if(blackList[mid] - mid - k >= 0) {
                right = mid;
            }
            // 后半段缺失k个值
            else {
                left = mid + 1;
            }
        }
        return k + left - 1;
    }
```

* 可以结合白名单和二分查找，充分利用空间和效率。

```java
class Solution {
    private int n;
    // 二分查找还是白名单
    private boolean flag;
    private List<Integer> whiteList;
    private Random r;

    public Solution(int N, int[] blacklist) {
        this.n = N;
        this.r = new Random();
        // 黑名单超过一半，采用白名单
        if(blacklist.length > N / 2) {
            this.flag = true;
            Set<Integer> set = new HashSet<>();
            for(int i = 0; i < N; i++) {
                set.add(i);
            }
            for(int b : blacklist) {
                set.remove(b);
            }
            this.whiteList = new ArrayList<>(set);
        }
        // 黑名单少于一半，采用二分查找
        else {
            this.flag = false;
            this.whiteList = new ArrayList<>();
            // 排序
            Arrays.sort(blacklist);
            for(int b : blacklist) {
                this.whiteList.add(b);
            }
        }
    }
    
    public int pick() {
        if(flag) return whiteList.get(r.nextInt(whiteList.size()));
        else {
            int k = r.nextInt(n - whiteList.size()) + 1;
            int left = 0, right = whiteList.size();
            while (left < right) {
                int mid = left + (right - left) / 2;
                if(whiteList.get(mid) - mid - k >= 0) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            return k + left - 1;
        }
    }
}
```

## 性能分析



## 官方解题