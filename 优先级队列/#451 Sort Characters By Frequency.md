[toc]

Given a string, sort it in decreasing order based on the frequency of characters.



## 题目解读

&emsp;给定字符串，根据频率降序重组字符串，频率相同则根据原来字符串中的位置排列。注意区分大小写。

```java
class Solution {
    public String frequencySort(String s) {
        
    }
}
```

## 程序设计

* 统计字母符号频率，然后入队最大堆，最后从最大堆遍历拼接即可。由于题目要求在频率相等的情况下按照相对顺序，所以需要在优先级队列中实现比较。

```java
class Solution {
    public String frequencySort(String s) {
        // 最大堆，频率相同则比较相对位置
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> {
            if(b[1] == a[1]) {
                return s.indexOf((char)a[0]) - s.indexOf((char)b[0]);
            } else {
                return b[1] - a[1];
            }
        });
        // ASCII编码共128个，统计频率
        int[] count = new int[128];
        for(char c : s.toCharArray()) {
            count[c] += 1;
        }
        // 入队
        for(int i = 0; i < count.length; i++) {
            if(count[i] > 0) {
                queue.add(new int[]{i, count[i]});
            }
        }
        // 拼接
        StringBuffer sb = new StringBuffer();
        while(!queue.isEmpty()) {
            int[] cur = queue.poll();
            for(int i = 0; i < cur[1]; i++) {
                sb.append((char)cur[0]);
            }
        }
        return sb.toString();
    }
}
```

测试样例：`aAbb`输出`bbaA`。

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$；空间复杂度为$O(N)$。

执行用时：8ms，在所有java提交中击败了91.35%的用户。

内存消耗：45.7MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;暂无，密切关注。