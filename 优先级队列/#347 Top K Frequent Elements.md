[toc]

Given a non-empty array of integers, return the **k** most frequent elements.



Note:

* You may assume $k$ is always valid, $1 \le k \le \text{number}$ of unique elements.
* Your algorithm's time complexity must be better than O(n log n), where n is the array's size.



## 题目解读

&emsp;给定数组，返回最常出现的$k$个元素。

```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        
    }
}
```

## 程序设计

* 从左至右遍历数组，统计频率然后加入到最大堆，块后出队$k$个。另一种思路是固定容量为$k$最小堆，当加入的元素频率大于堆顶时，出队并将新元素入队。

```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> record = new HashMap<>();
        // 统计频率
        for(int num : nums) {
            record.put(num, record.getOrDefault(num, 0) + 1);
        }
        // 最小堆
        PriorityQueue<Map.Entry<Integer, Integer>> queue = new PriorityQueue<>(k, (a,b) -> a.getValue() - b.getValue());
        // 遍历
        for(Map.Entry<Integer, Integer> entry : record.entrySet()) {
            // 容量达到k需要判断入队，如果比当前堆顶频率小，则丢弃
            if(queue.size() == k) {
                // 频率比当前堆顶大或一样但数字比堆顶小，则出队旧值，入队新值
                if(entry.getValue() > queue.peek().getValue() || 
                (entry.getValue() == queue.peek().getValue() && entry.getKey() < queue.peek().getKey())) {
                    queue.poll();
                    queue.add(entry);
                }
            } else {
                queue.add(entry);
            }
        }
        List<Integer> res = new LinkedList<>();
        while(!queue.isEmpty()) {
            res.add(queue.poll().getKey());
        }
        // 需要翻转，频率大的在前面
        Collections.reverse(res);
        return res;
    }
}
```

## 性能分析

&emsp;遍历数组需要$O(N)$的时间复杂度，每次都需要判断入队，时间复杂度为$O(\log_2k)$，总的时间复杂度为$O(N\log_2k)$。空间复杂度为$O(N)$。

执行用时：21ms，在所有java提交中击败了71.61%的用户。

内存消耗：48.4MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;思路同上。