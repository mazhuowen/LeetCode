[toc]

You are given some lists of `regions` where the first region of each list includes all other regions in that list.

Naturally, if a region `X` contains another region `Y` then `X` is bigger than `Y`. Also by definition a region `X` contains itself.

Given two regions `region1`, `region2`, find out the **smallest** region that contains both of them.

If you are given regions `r1`, `r2` and `r3` such that `r1` includes `r3`, it is guaranteed there is no `r2` such that `r2` includes `r3`.

It's guaranteed the smallest region exists.

Constraints:

* $2 \le \text{regions.length} \le 10^4$
* $\text{region1} != \text{region2}$
* All strings consist of English letters and spaces with at most 20 letters.



## 题目解读

&emsp;树的最小公共父节点问题。

```java
class Solution {
    public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {

    }
}
```

## 程序设计

* 使用集合记录一个结点到父节点的路径，从低至上遍历另一个结点路径，遇到的在集合中的第一个值就是最近父节点。

```java
class Solution {
    public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {
        Map<String, String> parents = new HashMap<>();
        for(List<String> region : regions) {
            for (int i = 1; i < region.size(); i++) {
                parents.put(region.get(i), region.get(0));
            }
        }
        // 加入区域1的路径
        Set<String> path = new HashSet<>();
        String traverse = region1;
        while (parents.get(traverse) != null) {
            path.add(traverse);
            traverse = parents.get(traverse);
        }
        // 比较区域2的路径
        traverse = region2;
        while (parents.get(traverse) != null) {
            if(path.contains(traverse)) {
                return traverse;
            }
            traverse = parents.get(traverse);
        }
        // 注意上述路径未加入根节点，如果运行到此处，说明公共区域是根节点
        return traverse;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了100.00%的用户。

内存消耗：44.1MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;暂无，密切关注。