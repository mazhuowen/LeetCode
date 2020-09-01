[toc]

Design and implement a data structure for a compressed string iterator. The given compressed string will be in the form of each letter followed by a positive integer representing the number of this letter existing in the original uncompressed string.

Implement the StringIterator class:

* `next()` Returns **the next character** if the original string still has uncompressed characters, otherwise returns a **white space**.
* `hasNext()` Returns true if there is any letter needs to be uncompressed in the original string, otherwise returns `false`.



**Constraints**:

* $1 \le \text{compressedString.length} \le 1000$
* `compressedString` consists of lower-case an upper-case English letters and digits.
* The number of a single character repetitions in `compressedString` is in the range $[1, 10^9]$
* At most `100` calls will be made to `next` and `hasNext`.



## 题目解读

&emsp;给定压缩字符串，设计迭代器。

```java
class StringIterator {

    public StringIterator(String compressedString) {

    }
    
    public char next() {

    }
    
    public boolean hasNext() {

    }
}

/**
 * Your StringIterator object will be instantiated and called as such:
 * StringIterator obj = new StringIterator(compressedString);
 * char param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

## 程序设计

* 最基本的方法可以将压缩字符串还原为原字符串，然后迭代，但还原后的原字符串可能太长，效率较低。不妨指针指向当前字符，并计数该字符的剩余遍历数目；当数目为$0$时表示遍历完成，迭代到下个字符。

```java
class StringIterator {
    char[] seqs;
    // 当前索引
    int idx = -1;
    int count = 0;
    // 下一个字符索引
    int nextIdx = 0;

    public StringIterator(String compressedString) {
        this.seqs = compressedString.toCharArray();
    }
    
    public char next() {
        if (hasNext()) {
            count--;
            return seqs[idx];
        }
        // 遍历结束，返回空格
       	return ' ';
    }
    
    public boolean hasNext() {
        // 当前字符有值
        if (count > 0) return true;
        
        // 获取下一个字符的数目（如果下一个字符数目是0，则迭代直到不为0或遍历结束）
        while (count == 0 && nextIdx < seqs.length) {
            idx = nextIdx;
            count = 0;
            int i = idx + 1;
            // 寻找下一个字符，计数当前字符
            while (i < seqs.length && Character.isDigit(seqs[i])) {
                count = count * 10 + (seqs[i++] - '0');
            }
            nextIdx = i;
        }
        
        // nextIdx小于数组长度或等于数组长度且计数大于0，说明存在元素
        return nextIdx < seqs.length || count > 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：15ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.4MB，在所有java提交中击败了38.23%的用户。

## 官方解题

&emsp;官方思路类似。