[toc]

Given a non-empty string **s** and an integer **k**, rearrange the string such that the same characters are at least distance **k** from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".



## 题目解读

&emsp;&emsp;给定非空的字符串和整数$k$，将字符串中的字母重新排列，使得重排后的字符串中相同字母的位置间隔距离至少为$k$。题目规定所有输入的字符串都由小写字母组成，如果找不到距离至少为$k$的重排结果，返回空字符串。

```java
class Solution {
    public String rearrangeString(String s, int k) {
        
    }
}
```

## 程序设计

* 可以看做是[#621 Task Scheduler](./#621 Task Scheduler.md)的堆思路的延续，首先统计频率并排序，然后入队，每一轮选取$k$个元素，并减少其计数；与[#621 Task Scheduler](./#621 Task Scheduler.md)不同的是没有控任务。

```java
class Solution {
    public String rearrangeString(String s, int k) {
        if(k == 0) {
            return s;
        }
        // 统计频率
        Pair[] counter = new Pair[26];
        for(char c : s.toCharArray()) {
            if(counter[c - 'a'] != null) {
                counter[c - 'a'].count += 1;
            } else {
                counter[c - 'a'] = new Pair(c, 1);
            }
        }
        // 最大堆（按照频率排序，频率相同则字典序靠前的在最前面）
        PriorityQueue<Pair> queue = new PriorityQueue<>((a, b) -> {
            if(a.count != b.count) {
                return b.count - a.count;
            } 
            return a.c - b.c;
        });
        // 入队
        for(Pair cur : counter) {
            if(cur != null) {
                queue.add(cur);
            }
        }
        String res = "";
        while(!queue.isEmpty()) {
            int i = k;
            List<Pair> temp = new LinkedList<>();
            // 取k个字符
            while(i > 0) {
                if(!queue.isEmpty()) {
                    // 出队，加入字符串，频率减一
                    Pair cur = queue.poll();
                    res += cur.c;
                    // 加入链表
                    if(--cur.count > 0) {
                        temp.add(cur);
                    }
                    // 次数减一
                    i--;
                } 
                else {
                    // 堆为空，但队列不为空，表示本轮凑不够k个字符，不符合要求，返回空
                    if(temp.size() > 0) {
                        return "";
                    } 
                    // 堆为空，链表为空，表示已匹配完
                    else {
                        return res;
                    }
                }
            }
             // 将队列中的值重新加入堆
            for(Pair cur : temp) {
                queue.add(cur);
            }
        }
        return res;
    }
}

class Pair {
    char c;
    int count;

    Pair(char c, int count) {
        this.c = c;
        this.count = count;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度为$O(N)$。

执行用时：905ms，在所有java提交中击败了5.17%的用户。

内存消耗：48.3MB，在所有java提交中击败了5.72%的用户。

>  使用StringBuffer拼接能进一步提高性能：

执行用时：32ms，在所有java提交中击败了48.28%的用户。

内存消耗：46.4MB，在所有java提交中击败了5.72%的用户。

## 官方解题

&emsp;暂无，密切关注。