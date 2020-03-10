[toc]

Given a string `S`, check if the letters can be rearranged so that two characters that are adjacent to each other are not the same.

If possible, output any possible result.  If not possible, return the empty string.

**Note:**

- `S` will consist of lowercase letters and have length in range `[1, 500]`.



## 题目解读

&emsp;给定字符串，检查是否能重新排布其中的字母，使得两相邻的字符不同。如果存在则返回，不存在则返回空。

```java
class Solution {
    public String reorganizeString(String S) {

    }
}
```

## 程序设计

* 最基本的想法是堆根据字符计数排序，每次取出最大的和次大的字符组成两个字符，同时计数减一。

```java
class Solution {
    public String reorganizeString(String S) {
        if(S == null || S.isEmpty()) {
            return "";
        }
        // 计数
        char[] counter = new char[26];
        for(char c : S.toCharArray()) {
            counter[c - 'a']++;
        }
        // 最大堆
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> b[1] - a[1]);
        for (int i = 0; i < 26; i++) {
            if(counter[i] > 0) queue.add(new int[]{i, counter[i]});
        }
        // 有字符超出长度一半，则不符合要求
        if(queue.peek()[1] > (S.length() + 1) / 2) {
            return "";
        }
        StringBuffer sb = new StringBuffer();
        while(!queue.isEmpty()) {
            int[] cur = queue.poll();
            // 队列不为空则取下一个拼接，组成两个不相等的邻接字符
            if(!queue.isEmpty()) {
                int[] next = queue.poll();
                // 拼接，此时两两不相同
                sb.append((char)(cur[0] + 'a')).append((char)(next[0] + 'a'));
                // 减少计数并再次入队
                if(--cur[1] > 0) {
                    queue.add(cur);
                }
                if(--next[1] > 0) {
                    queue.add(next);
                }
            } 
            // 队列为空，则判断cur计数是否为0，为0且队列为空表示遍历完成，拼接即可
            else if(--cur[1] == 0) {
                sb.append((char)(cur[0] + 'a'));
            } 
            // 不是则存在重复，返回空
            else {
                return "";
            }
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_226)$，为$O(N)$；空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了55.10%的用户。

内存消耗：37.6MB，在所有java提交中击败了5.59%的用户。

## 官方解题

&emsp;除了堆的思路，官方还提供了排序然后交替插入的思路。首先统计计数字符，从小到大依次放入奇数索引；如果奇数索引放完，则开始放入偶数索引。这期间要判断字符计数是否大于长度的一半，如果大于则无法组成合法的字符，小于则保证奇数索引放完后从0开始放入偶数数组不会遇到在奇数索引的自己。这样最后的字符串相邻字符不同。

```java
class Solution {
    public String reorganizeString(String S) {
        int N = S.length();
        // 统计，每次加100
        int[] counts = new int[26];
        for (char c: S.toCharArray()) counts[c-'a'] += 100;
        // 每个字符加上其编码，防止在排序后丢失索引对应的字符信息
        for (int i = 0; i < 26; ++i) counts[i] += i;
        Arrays.sort(counts);
		// 新的重组字符数组
        char[] ans = new char[N];
        // 奇数索引
        int t = 1;
        // 计数从小到大
        for (int code: counts) {
            // 获取计数
            int ct = code / 100;
            // 获取字符信息
            char ch = (char) ('a' + (code % 100));
            // 计数大于一半，不合法，返回
            if (ct > (N+1) / 2) return "";
            // 将字符添加到奇数索引上
            for (int i = 0; i < ct; ++i) {
                // 如果添加完，则添加偶数索引
                if (t >= N) t = 0;
                ans[t] = ch;
                t += 2;
            }
        }
        return String.valueOf(ans);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.35%的用户。

内存消耗：37.8MB，在所有java提交中击败了5.59%的用户。