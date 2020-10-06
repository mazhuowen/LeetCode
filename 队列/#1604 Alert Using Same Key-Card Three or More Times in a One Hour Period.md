[toc]

Leetcode company workers use key-cards to unlock office doors. Each time a worker uses their key-card, the security system saves the worker's name and the time when it was used. The system emits an **alert** if any worker uses the key-card **three or more times** in a one-hour period.

You are given a list of strings `keyName` and `keyTime` where `[keyName[i], keyTime[i]]` corresponds to a person's name and the time when their key-card was used **in a single day**.

Access times are given in the **24-hour time format "HH:MM"**, such as `"23:51"` and `"09:49"`.

Return a list of unique worker names who received an alert for frequent keycard use. Sort the names in **ascending order alphabetically**.

Notice that `"10:00"` - `"11:00"` is considered to be within a one-hour period, while `"23:51"` - `"00:10"` is not considered to be within a one-hour period.



**Constraints**:

* $1 \le \text{keyName.length, keyTime.length} \le 10^5$
* $\text{keyName.length} == \text{keyTime.length}$
* `keyTime` are in the format `"HH:MM"`.
* `[keyName[i]`, keyTime[i]]` is unique.
* $1 \le \text{keyName[i].length} \le 10$
* `keyName[i]` contains only lowercase English letters.



## 题目解读

&emsp;求在一个小时内打卡超过三次的员工。

```java
class Solution {
    public List<String> alertNames(String[] keyName, String[] keyTime) {

    }
}
```

## 程序设计

* 基本思路为根据姓名和时间排序，然后滑动窗口滑动判断。

```java
class Solution {
    public List<String> alertNames(String[] keyName, String[] keyTime) {
        int n = keyName.length;
        Integer[] index = new Integer[n];
        for (int i = 0; i < n; i++) index[i] = i;
        // 根据人和时间排序
        Arrays.sort(index, (a, b) -> keyName[a].equals(keyName[b]) ? keyTime[a].compareTo(keyTime[b]) : keyName[a].compareTo(keyName[b]));

        List<String> res = new LinkedList<>();
        // 用于排重
        String pre = null;
        // 滑动窗口
        int left = 0, right = 0;
        while (right < n) {
            int idxL = index[left], idxR = index[right];
            // 姓名不同
            if (!keyName[idxL].equals(keyName[idxR])) {
                if (right - left >= 3 && !keyName[idxL].equals(pre)) res.add(pre = keyName[idxL]);
                left = right++;
            } 
            // 超过一小时
            else if(!lessThanOneHour(keyTime[idxL], keyTime[idxR])) {
                if (right - left >= 3 && !keyName[idxL].equals(pre)) res.add(pre = keyName[idxL]);
                left++;
            } else right++;
        }
        // 最后一段
        if (n - left >= 3 && !keyName[index[left]].equals(pre)) res.add(keyName[index[left]]);
        return res;
    }

    private boolean lessThanOneHour(String t1, String t2) {
        int h1 = Integer.parseInt(t1.substring(0, 2)), h2 = Integer.parseInt(t2.substring(0, 2));
        if (h1 > h2) throw new IllegalArgumentException("inalid param");
        // 小时相等，必然在一小时内
        if (h1 == h2) return true;
        // 小时差距为1且分钟m1大于等于m2
        if (h1 == h2 - 1 && Integer.parseInt(t1.substring(3, 5)) >= Integer.parseInt(t2.substring(3, 5))) return true;
        else return false; 
    }
}
```

> 由于字符串处理缘故性能较低。

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：244ms，在所有java提交中击败了13.68%的用户。

内存消耗：56.1MB，在所有java提交中击败了94.81%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用哈希表保存各自的记录然后排序滑动窗口判断。