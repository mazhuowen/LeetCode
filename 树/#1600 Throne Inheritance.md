[toc]

A kingdom consists of a king, his children, his grandchildren, and so on. Every once in a while, someone in the family dies or a child is born.

The kingdom has a well-defined order of inheritance that consists of the king as the first member. Let's define the recursive function `Successor(x, curOrder)`, which given a person `x` and the inheritance order so far, returns who should be the next person after `x` in the order of inheritance.

```
Successor(x, curOrder):
    if x has no children or all of x's children are in curOrder:
        if x is the king return null
        else return Successor(x's parent, curOrder)
    else return x's oldest child who's not in curOrder
```

For example, assume we have a kingdom that consists of the king, his children Alice and Bob (Alice is older than Bob), and finally Alice's son Jack.

* In the beginning, `curOrder` will be `["king"]`.
* Calling `Successor(king, curOrder)` will return `Alice`, so we append to `curOrder` to get `["king", "Alice"]`.
* Calling `Successor(Alice, curOrder)` will return `Jack`, so we append to `curOrder` to get `["king", "Alice", "Jack"]`.
* Calling `Successor(Jack, curOrder)` will return `Bob`, so we append to `curOrder` to get `["king", "Alice", "Jack", "Bob"]`.
* Calling `Successor(Bob, curOrder)` will return `null`. Thus the order of inheritance will be `["king", "Alice", "Jack", "Bob"]`.

Using the above function, we can always obtain a unique order of inheritance.

Implement the `ThroneInheritance` class:

* `ThroneInheritance(string kingName)` Initializes an object of the `ThroneInheritance` class. The name of the king is given as part of the constructor.
* `void birth(string parentName, string childName)` Indicates that `parentName` gave birth to `childName`.
* `void death(string name)` Indicates the death of `name`. The death of the person doesn't affect the `Successor` function nor the current inheritance order. You can treat it as just marking the person as dead.
* `string[] getInheritanceOrder()` Returns a list representing the current order of inheritance **excluding** dead people.



**Constraints**:

* $1 \le \text{kingName.length, parentName.length, childName.length, name.length} \le 15$
* `kingName`, `parentName`, `childName`, and `name` consist of lowercase English letters only.
* All arguments `childName` and `kingName` are **distinct**.
* All `name` arguments of `death` will be passed to either the constructor or as **childName** to **birth** first.
* For each call to `birth(parentName, childName)`, it is guaranteed that `parentName` is alive.
* At most $10^5$ calls will be made to `birth` and `death`.
* At most $10$ calls will be made to `getInheritanceOrder`.



## 题目解读

&emsp;实现类可维护继承顺序关系。

```java
class ThroneInheritance {

    public ThroneInheritance(String kingName) {

    }
    
    public void birth(String parentName, String childName) {

    }
    
    public void death(String name) {

    }
    
    public List<String> getInheritanceOrder() {

    }
}

/**
 * Your ThroneInheritance object will be instantiated and called as such:
 * ThroneInheritance obj = new ThroneInheritance(kingName);
 * obj.birth(parentName,childName);
 * obj.death(name);
 * List<String> param_3 = obj.getInheritanceOrder();
 */
```

## 程序设计

* 将家族树使用儿子兄弟链表表示法表示，仔细分析可知继承关系就是二叉树的前序遍历。

```java
class ThroneInheritance {
    // 树
    Tree root;
    // 记录节点
    Map<String, Tree> nodes;
    // 记录插入的位置
    Map<String, Tree> inserts;

    public ThroneInheritance(String kingName) {
        this.root = new Tree(kingName);
        this.nodes = new HashMap<>();
        this.inserts = new HashMap<>();
        nodes.put(kingName, root);
        // 插入位置，如果是空，表示当前节点无子节点；如果不是空，表示当前节点存在子节点，字典纸就是兄弟节点
        inserts.put(kingName, null);
    }

    public void birth(String parentName, String childName) {
        Tree insert = inserts.get(parentName);
        Tree child = new Tree(childName);
        // 不存在兄弟，则插入子节点
        if (insert == null) nodes.get(parentName).child = child;
        // 存在兄弟，则插入兄弟节点后
        else insert.brother = child;

        // 维护关系
        nodes.put(childName, child);
        inserts.put(parentName, child);
    }

    public void death(String name) {
        // 查找标记
        Tree node = nodes.get(name);
        node.death = true;
    }

    public List<String> getInheritanceOrder() {
        List<String> res = new LinkedList<>();
        // 前序遍历
        getInheritanceOrder(root, res);
        return res;
    }

    private void getInheritanceOrder(Tree cur, List<String> res) {
        if (cur == null) return;
        // 将死亡的人过滤
        if (!cur.death) res.add(cur.name);
        getInheritanceOrder(cur.child, res);
        getInheritanceOrder(cur.brother, res);
    }
}

class Tree {
    // 姓名
    String name;
    // 死亡状态
    boolean death;
    // 树的父子节点表示法
    Tree child;
    Tree brother;

    Tree(String name) {
        this.name = name;
    }
}
```

## 性能分析

&emsp;插入、标记死亡状态时间复杂度为$O(1)$，得到继承顺位时间复杂度为$O(N)$。

执行用时：320ms，在所有java提交中击败了89.06%的用户。

内存消耗：96.3MB，在所有java提交中击败了93.23%的用户。

## 官方解题

&emsp;暂无，密切关注。