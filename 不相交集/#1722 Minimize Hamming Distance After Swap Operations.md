[toc]

You are given two integer arrays, `source` and `target`, both of length $n$. You are also given an array `allowedSwaps` where each `allowedSwaps[i] = [ai, bi]` indicates that you are allowed to swap the elements at index `ai` and index `bi` (**0-indexed**) of array `source`. Note that you can swap elements at a specific pair of indices **multiple** times and in **any** order.

The **Hamming distance** of two arrays of the same length, `source` and `target`, is the number of positions where the elements are different. Formally, it is the number of indices $i$ for $0 \le i \le n-1$ where `source[i] != target[i]` (**0-indexed**).

Return the **minimum Hamming distance** of `source` and `target` after performing **any** amount of swap operations on array `source`.

 

**Example 1**:

```
Input: source = [1,2,3,4], target = [2,1,4,5], allowedSwaps = [[0,1],[2,3]]
Output: 1
Explanation: source can be transformed the following way:

- Swap indices 0 and 1: source = [2,1,3,4]
- Swap indices 2 and 3: source = [2,1,4,3]
  The Hamming distance of source and target is 1 as they differ in 1 position: index 3.
```

**Example 2**:

```
Input: source = [1,2,3,4], target = [1,3,2,4], allowedSwaps = []
Output: 2
Explanation: There are no allowed swaps.
The Hamming distance of source and target is 2 as they differ in 2 positions: index 1 and index 2.
```

**Example 3**:

```
Input: source = [5,1,2,4,3], target = [1,5,4,2,3], allowedSwaps = [[0,4],[4,2],[1,3],[1,4]]
Output: 0
```



**Constraints**:

* $n == \text{source.length} == \text{target.length}$
* $1 \le n \le 10^5$
* $1 \le \text{source[i], target[i]} \le 10^5$
* $0 \le \text{allowedSwaps.length} \le 10^5$
* $\text{allowedSwaps[i].length} == 2$
* $0 \le a_i, b_i \le n - 1$
* $a_i \ne b_i$



## 题目解读

&emsp;给定两个等长数组及可交换的索引对，求任意次交换后可得的最小汉明距离，串汉明距离定义为相等索引处数字不相等的数目。

```java
class Solution {
    public int minimumHammingDistance(int[] source, int[] target, int[][] allowedSwaps) {

    }
}
```

## 程序设计

* 核心点在于交换对组成的一个集合内的所有索引两两之间都是可以交换的，这样就可以使用不相交集得到每个集合，然后在每个集合判断相等的数字和不等的数字即可。

```java
class Solution {
    public int minimumHammingDistance(int[] source, int[] target, int[][] allowedSwaps) {
        int n = source.length;
        DisJoint disJoint = new DisJoint(n);
        for (int[] swap : allowedSwaps) disJoint.union(disJoint.find(swap[0]), disJoint.find(swap[1]));
        
        int res = 0;
        // 遍历每个不相交集，即连通图
        for (Map.Entry<Integer, List<Integer>> entry : disJoint.getGroup().entrySet()) {
            List<Integer> index = entry.getValue();
            // 差值统计
            Map<Integer, Integer> record = new HashMap<>();
            for (int idx : index) {
                record.put(source[idx], record.getOrDefault(source[idx], 0) + 1);
                record.put(target[idx], record.getOrDefault(target[idx], 0) - 1);
            } 
            // 统计相差的值
            for (int diff : record.values()) {
                if (diff > 0) res += diff;
            }
        }
        return res;
    }
}

class DisJoint {
    private int[] parent;
    private Map<Integer, List<Integer>> group;
    
    DisJoint(int size) {
        this.parent = new int[size];
        this.group = new HashMap<>();
        Arrays.fill(parent, -1);
        for (int i = 0; i < size; i++) {
            List<Integer> tmp = new LinkedList<>();
            tmp.add(i);
            group.put(i, tmp);
        }
    }
    
    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;
        
        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
            group.get(r1).addAll(group.remove(r2));
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
            group.get(r2).addAll(group.remove(r1));
        }
    }
    
    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
    
    public Map<Integer, List<Integer>> getGroup() {
        return this.group;
    }
}
```

* 进行性能优化，将不相交集中的哈希表操作分离开来。

```java
class Solution {
    public int minimumHammingDistance(int[] source, int[] target, int[][] allowedSwaps) {
        int n = source.length;
        DisJoint disJoint = new DisJoint(n);
        for (int[] swap : allowedSwaps) disJoint.union(disJoint.find(swap[0]), disJoint.find(swap[1]));

        // 统计每个不相交集内source的数字计数
        Map<Integer, Map<Integer, Integer>> record = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int root = disJoint.find(i);
            if (!record.containsKey(root)) record.put(root, new HashMap<>());
            Map<Integer, Integer> counter = record.get(root);
            counter.put(source[i], counter.getOrDefault(source[i], 0) + 1);
            counter.put(target[i], counter.getOrDefault(target[i], 0) - 1);
        }

        // 统计差值
        int res = 0;
        for (Map<Integer, Integer> counter : record.values()) {
            for (int num : counter.values()) {
                if (num > 0) res += num;
            }
        }
        return res;
    }
}

class DisJoint {
    private int[] parent;
    
    DisJoint(int size) {
        this.parent = new int[size];
        Arrays.fill(parent, -1);
    }
    
    public void union(int r1, int r2) {
        if (parent[r1] >= 0 || parent[r2] >= 0) throw new IllegalArgumentException("invalid param");
        if (r1 == r2) return;
        
        if (parent[r1] <= parent[r2]) {
            parent[r1] += parent[r2];
            parent[r2] = r1;
        } else {
            parent[r2] += parent[r1];
            parent[r1] = r2;
        }
    }
    
    public int find(int idx) {
        if (parent[idx] < 0) return idx;
        return parent[idx] = find(parent[idx]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：163 ms, 在所有 Java 提交中击败了9.06%的用户。

内存消耗：87.7 MB, 在所有 Java 提交中击败了13.57%的用户。

&emsp;优化后：

执行用时：74 ms, 在所有 Java 提交中击败了50.30%的用户。

内存消耗：76.8 MB, 在所有 Java 提交中击败了69.18%的用户。

## 官方解题

&emsp;暂无，密切关注。
