[toc]

Your task is to design the basic function of Excel and implement the function of sum formula. Specifically, you need to implement the following functions:

* `Excel(int H, char W)`: This is the constructor. The inputs represents the height and width of the Excel form. **H** is a positive integer, range from 1 to 26. It represents the height. **W** is a character range from `'A'` to `'Z'`. It represents that the width is the number of characters from `'A'` to **W**. The Excel form content is represented by a height * width 2D integer array **C**, it should be initialized to zero. You should assume that the first row of **C** starts from 1, and the first column of **C** starts from `'A'`.

* `void Set(int row, char column, int val)`: Change the value at `C(row, column)` to be val.

* `int Get(int row, char column)`: Return the value at `C(row, column)`.

* `int Sum(int row, char column, List of Strings : numbers)`: This function calculate and set the value at `C(row, column)`, where the value should be the sum of cells represented by numbers. This function return the sum result at `C(row, column)`. This sum formula should exist until this cell is overlapped by another value or another sum formula.`numbers` is a list of strings that each string represent a cell or a range of cells. If the string represent a single cell, then it has the following format : `ColRow`. For example, `"F7"` represents the cell at `(7, F)`.If the string represent a range of cells, then it has the following format : `ColRow1:ColRow2`. The range will always be a rectangle, and `ColRow1` represent the position of the top-left cell, and `ColRow2` represents the position of the bottom-right cell.



**Note**:

* You could assume that there won't be any circular sum reference. For example, `A1 = sum(B1)` and `B1 = sum(A1)`.
* The test cases are using double-quotes to represent a character.
* Please remember to **RESET** your class variables declared in class Excel, as static/class variables are **persisted across multiple test cases**. Please see here for more details.



## 题目解读

&emsp;设计`Excel`求和公式功能。

```java
class Excel {

    public Excel(int H, char W) {

    }
    
    public void set(int r, char c, int v) {

    }
    
    public int get(int r, char c) {

    }
    
    public int sum(int r, char c, String[] strs) {

    }
}

/**
 * Your Excel object will be instantiated and called as such:
 * Excel obj = new Excel(H, W);
 * obj.set(r,c,v);
 * int param_2 = obj.get(r,c);
 * int param_3 = obj.sum(r,c,strs);
 */
```

## 程序设计

* 需要注意公式可以嵌套，这意味着求和公式里的元素可以是固定的单元格，也可以是之前求和的单元变量。可使用图的结构来保存依赖关系，被依赖点指向依赖点，改动被依赖点则深度优先搜索更新其后的依赖单元格。
* 需注意公式可以依赖同一单元格多次，如`["A1","A1"]`依赖`A1`两次，`["A1","A1:C3"]`依赖`A1`两次，这样边的权重就是依赖次数，在更新值时，需要乘次数。
* 综上，使用二维数组表示`Excel`（数据稀疏的话也可以使用哈希表等），其中的单元是表示依赖关系的图节点，更新则将旧的图节点作标记，插入新的节点，使用懒删除的方法，直到后续节点更新依赖关系，遇到旧节点再会删除。

```java
class Excel {
    Ceil[][] excel;

    public Excel(int H, char W) {
        this.excel = new Ceil[H][W - 'A' + 1];
    }

    public void set(int r, char c, int v) {
        int i = r - 1, j = c - 'A';
        Ceil data = new Ceil(v, false);
        // 修改后的改变值
        int change = v;
        if (excel[i][j] != null) {
            change = v - excel[i][j].val;
            data.dependency = excel[i][j].dependency;
            // 覆盖之前的和，需要将之前的依赖关系删除
            excel[i][j].isOverland = true;
        }
        // 覆盖
        excel[i][j] = data;
        // 更新依赖关系
        if (change != 0) update(change, data);
    }

    public int get(int r, char c) {
        int i = r - 1, j = c - 'A';
        // 格子为空或占位
        return excel[i][j] == null ? 0 : excel[i][j].val;
    }

    public int sum(int r, char c, String[] strs) {
        int i = r - 1, j = c - 'A';

        Ceil ceil = new Ceil(0, true);
        if (excel[i][j] != null) {
            // 覆盖之前的和，需要将之前的依赖关系删除
            if (excel[i][j].isSum) excel[i][j].isOverland = true;

            ceil.dependency = excel[i][j].dependency;
        }

        // 求和
        int sum = 0;
        for (String str : strs) {
            sum += getCeilSumByString(str, ceil);
        }
        ceil.val = sum;
        // 覆盖
        excel[i][j] = ceil;
        return sum;
    }

    // 深度优先遍历更新
    private void update(int change, Ceil node) {
        List<Ceil> remove = new LinkedList<>();
        for (Ceil ceil : node.dependency.keySet()) {
            // 已经不再是求和公式或被覆盖，移除依赖
            if (!ceil.isSum || ceil.isOverland) {
                remove.add(ceil);
            }
            // 深度优先遍历更新
            else {
                ceil.val += change * node.dependency.get(ceil);
                update(change * node.dependency.get(ceil), ceil);
            }
        }

        for (Ceil ceil : remove) node.dependency.remove(ceil);
    }

    private int getCeilSumByString(String str, Ceil dependence) {
        int sum = 0;
        // 单独格子
        if (!str.contains(":")) {
            int[] axis = parseString(str);
            sum += addDependency(axis[0], axis[1], dependence);
        }
        // 区间
        else {
            String[] strs = str.split(":");
            int[] start = parseString(strs[0]);
            int[] end = parseString(strs[1]);
            for (int i = start[0]; i <= end[0]; i++) {
                for (int j = start[1]; j <= end[1]; j++) {
                    sum += addDependency(i, j, dependence);
                }
            }
        }
        return sum;
    }

    private int[] parseString(String str) {
        int i = Integer.parseInt(str.substring(1)) - 1;
        int j = str.charAt(0) - 'A';
        return new int[]{i, j};
    }

    // 维护被依赖格子与依赖格子关系，如果被依赖格子当前无值，初始化为0
    private int addDependency(int i, int j, Ceil dependence) {
        // 为空则初始化为0
        if (excel[i][j] == null) excel[i][j] = new Ceil(0, false);
        // 添加依赖
        Map<Ceil, Integer> dependency = excel[i][j].dependency;
        dependency.put(dependence, dependency.getOrDefault(dependence, 0) + 1);
        return excel[i][j].val;
    }
}

class Ceil {
    int val;
    // 是设置值还是求和公式
    boolean isSum;
    // 是否被覆盖
    boolean isOverland;

    // 影响的求和公式节点（记录指向的节点和权重）
    Map<Ceil, Integer> dependency = new HashMap<>();

    public Ceil(int val, boolean isSum) {
        this.val = val;
        this.isSum = isSum;
    }
}
```

## 性能分析

&emsp;插入、获取时间复杂度为$O(1)$，求和时间复杂度与深度优先搜索有关，最坏$O(HW)$，空间复杂度为$O(HW)$。

执行用时：10ms，在所有java提交中击败了90.28%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方采用栈的形式，每次都遍历整个数组来维护更新求和公式。