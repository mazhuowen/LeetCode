[toc]

Design and implement an iterator to flatten a 2d vector. It should support the following operations: `next` and `hasNext`.

 

**Notes**:

* Please remember to **RESET** your class variables declared in Vector2D, as static/class variables are **persisted across multiple test cases**. Please see `here` for more details.
* You may assume that `next()` call will always be valid, that is, there will be at least a next element in the 2d vector when `next()` is called.



## 题目解读

&emsp;设计迭代器打印二维数组，注意数组元素长度不等。

```java
class Vector2D {

    public Vector2D(int[][] v) {

    }
    
    public int next() {

    }
    
    public boolean hasNext() {

    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D obj = new Vector2D(v);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

## 程序设计

* 需要注意空数组。

```java
class Vector2D {
    int r, c;
    int[][] v;

    public Vector2D(int[][] v) {
        this.v = v;
    }
    
    public int next() {
        if (!hasNext()) throw new IllegalArgumentException("invalid operation");

        return v[r][c++];
    }
    
    public boolean hasNext() {
        while (r < v.length && c >= v[r].length) {
            r++;
            c = 0;
        }
        return r < v.length;
    }
}
```

## 性能分析

&emsp;`next`时间复杂度为$O(1)$，`hasNext`时间复杂度最坏为$O(M)$；空间复杂度为$O(1)$。

执行用时：12ms，在所有java提交中击败了99.33%的用户。

内存消耗：46.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。