[toc]

Given a list `accounts`, each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are emails representing `emails` of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in any order.

Note:

* The length of `accounts` will be in the range `[1, 1000]`.
* The length of `accounts[i]` will be in the range `[1, 10]`.
* The length of `accounts[i][j]` will be in the range `[1, 30]`.



## 题目解读

&emsp;根据邮箱合并用户，返回用户和其邮箱列表，列表需要按照字母序排序。

```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {

    }
}
```

## 程序设计

* 首先利用字典将邮箱映射为索引，然后进行不相交集的合并操作，最后遍历邮箱，根据不相交集分组，排序后拼接为链表。

```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        List<List<String>> res = new LinkedList<>();
        if(accounts == null || accounts.size() == 0) {
            return res;
        }
        // 邮箱账号的映射，由于存在一个邮箱映射多个用户名的情况，只保存第一个
        Map<String, String> mailToAccount = new HashMap<>();
        // 邮箱及索引映射
        Map<String, Integer> idxMap = new HashMap<>();
        int idx = 0;
        // 维护字典信息
        for(List<String> mailInfo : accounts) {
            for (int i = 1; i < mailInfo.size(); i++) {
                mailToAccount.putIfAbsent(mailInfo.get(i), mailInfo.get(0));
                if(idxMap.get(mailInfo.get(i)) == null) {
                    idxMap.put(mailInfo.get(i), idx++);
                }
            }
        }
        // 将同一链表中的邮箱合并
        DisJoint disJoint = new DisJoint(idxMap.size());
        for(List<String> mailInfo : accounts) {
            for (int i = 2; i < mailInfo.size(); i++) {
                disJoint.union(disJoint.find(idxMap.get(mailInfo.get(1))), disJoint.find(idxMap.get(mailInfo.get(i))));
            }
        }
        // 记录每个不相交集的邮箱链表
        Map<Integer, List<String>> temp = new HashMap<>();
        for(Map.Entry<String, String> entry : mailToAccount.entrySet()) {
            int root = disJoint.find(idxMap.get(entry.getKey()));
            if(temp.get(root) == null) {
                temp.put(root, new LinkedList<>());
                temp.get(root).add(entry.getValue());
            }
            temp.get(root).add(entry.getKey());
        }
        // 拼接答案，注意题目需要将邮箱排序
        for(Map.Entry<Integer, List<String>> entry : temp.entrySet()) {
            Collections.sort(entry.getValue());
            res.add(entry.getValue());
        }
        return res;
    }
}

class DisJoint {
    private int[] tree;

    DisJoint(int size) {
        this.tree = new int[size];
        // 初始化为size个不相交集
        for(int i = 0; i < size; i++) {
            tree[i] = -1;
        }
    }
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) {
            return val;
        }
        return tree[val] = find(tree[val]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM\log_2M)$，空间复杂度为$O(NM)$，其中$N$为账号数，$M$为平均邮箱长度。

执行用时：44ms，在所有java提交中击败了85.91%的用户。

内存消耗：45MB，在所有java提交中击败了54.55%的用户。

## 官方解题

&emsp;除了上述思路还介绍了深度优先遍历的思路。