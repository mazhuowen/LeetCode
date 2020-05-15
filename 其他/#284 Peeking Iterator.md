[toc]

Given an Iterator class interface with methods: `next()` and `hasNext()`, design and implement a PeekingIterator that support the `peek()` operation -- it essentially `peek()` at the element that will be returned by the next call to `next()`.



Follow up: How would you extend your design to be generic and work with all types, not just integer?



## 题目解读

&emsp;&emsp;设计迭代器，支持`peek`操作。

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    
	}
	
	@Override
	public boolean hasNext() {
	    
	}
}
```

## 程序设计

* 使用变量寄存迭代值。

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {
	// 保存临时值
	Integer temp;
	Iterator<Integer> iterator;

	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    this.iterator = iterator;
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        if (temp == null && iterator.hasNext()) temp = iterator.next();
		return temp;
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    if (temp != null) {
			Integer res = temp;
			temp =null;
			return res;
		}
		return iterator.next();
	}
	
	@Override
	public boolean hasNext() {
	    return temp != null || iterator.hasNext();
	}
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了92.37%的用户。

内存消耗：39.5MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。