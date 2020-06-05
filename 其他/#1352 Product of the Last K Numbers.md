[toc]

Implement the class `ProductOfNumbers` that supports two methods:

* `add(int num)`
  * Adds the number `num` to the back of the current list of numbers.

* `getProduct(int k)`
  * Returns the product of the last `k` numbers in the current list.
  * You can assume that always the current list has **at least** `k` numbers.

At any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.



Constraints:

* There will be at most `40000` operations considering both `add` and `getProduct`.
* $0 \le \text{num} \le 100$
* $1 \le k \le 40000$



## 题目解读

&emsp;设计数据结构，支持计算后`k`位的数字乘积。

```java
class ProductOfNumbers {

    public ProductOfNumbers() {

    }
    
    public void add(int num) {

    }
    
    public int getProduct(int k) {

    }
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers obj = new ProductOfNumbers();
 * obj.add(num);
 * int param_2 = obj.getProduct(k);
 */
```

## 程序设计

* 最基本的思路是前缀积，这样求区间内的乘积只需结束、起始的前缀积相除即可；考虑到区间内存在0的数字，会导致分母为0且后续乘积为0，可修改机制，使用另一个数组记录值为0的索引，遇到0在数组中记录并在前缀积中加入1。
* 每次先检索待求区间是否存在0，没有再计算。

```java
class ProductOfNumbers {
    int size;
    List<Integer> zeroIdx;
    List<Integer> numProduct;

    public ProductOfNumbers() {
        this.zeroIdx = new ArrayList<>();
        this.numProduct = new ArrayList<>();
        // 前缀积起始为1
        this.size = 1;
        this.numProduct.add(1);
    }

    public void add(int num) {
        if (num == 0) {
            zeroIdx.add(size);
            numProduct.add(1);
        } else {
            int preProduct = 1;
            if (size != 0 && numProduct.get(size - 1) != 0) preProduct = numProduct.get(size - 1);
            numProduct.add(preProduct * num);
        }
        size++;
    }

    public int getProduct(int k) {
        if (size < k + 1) throw new IllegalArgumentException("invalid param");

        int start = size - k, end = size - 1;
        // 区间存在0
        if (isZero(start, end)) return 0;

        return numProduct.get(end) / numProduct.get(start - 1);
    }

    private boolean isZero(int min, int max) {
        if (zeroIdx.isEmpty()) return false;

        int left = 0, right = zeroIdx.size() - 1;
        // 寻找大于min的第一个索引
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (zeroIdx.get(mid) < min) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        //　校验索引
        return zeroIdx.get(left) >= min && zeroIdx.get(left) <= max;
    }
}
```

* 上述思路对任意区间通用。在本题中由于是尾部区间，每当遇到0时可以重置重新计算前缀和，因为只要包含当前0的位置的尾部区间，乘积为0。

```java
class ProductOfNumbers {
    List<Integer> numProduct;

    public ProductOfNumbers() {
        this.numProduct = new ArrayList<>();
        this.numProduct.add(1);
    }

    public void add(int num) {
        if (num == 0) {
            numProduct.clear();
            numProduct.add(1);
        }
        else numProduct.add(numProduct.get(numProduct.size() - 1) * num);
    }

    public int getProduct(int k) {
        if (k >= numProduct.size()) return 0;
        
        int end = numProduct.size() - 1, start = end - k;
        return numProduct.get(end) / numProduct.get(start);
    }
}
```

## 性能分析

&emsp;通用思路时间复杂度为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：25ms，在所有java提交中击败了53.38%的用户。

内存消耗：64MB，在所有java提交中击败了100.00%的用户。

&emsp;简化思路时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：21ms，在所有java提交中击败了86.47%的用户。

内存消耗：62.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了暴力搜索的思路。