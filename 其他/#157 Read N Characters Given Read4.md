[toc]

Given a file and assume that you can only read the file using a given method `read4`, implement a method to read n characters.

 

**Method read4**:

The API `read4` reads 4 consecutive characters from the file, then writes those characters into the buffer array `buf`.

The return value is the number of actual characters read.

Note that `read4()` has its own file pointer, much like `FILE *fp` in C.

**Definition of read4**:

    Parameter:  char[] buf
    Returns:    int

Note: `buf[]` is destination not source, the results from `read4` will be copied to `buf[]`



**Method read**:

By using the `read4` method, implement the method `read` that reads n characters from the file and store it in the buffer array `buf`. Consider that you **cannot** manipulate the file directly.

The return value is the number of actual characters read.

**Definition of read**:

    Parameters:	char[] buf, int n
    Returns:	int

Note: `buf[]` is destination not source, you will need to write the results to `buf[]`



Note:

* Consider that you **cannot** manipulate the file directly, the file is only accesible for `read4` but **not** for `read`.
* The `read` function will only be called once for each test case.
* You may assume the destination buffer array, `buf`, is guaranteed to have enough space for storing n characters.



## 题目解读

&emsp;使用`read4`实现读取任意长的字符。

```java
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char[] buf);
 */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    public int read(char[] buf, int n) {
       
    }
}
```

## 程序设计

* 循环读取，直到结束或读到指定长度的字符。

```java
public class Solution extends Reader4 {
    public int read(char[] buf, int n) {
        if (n <= 0) return 0;

        int idx = 0, count = n;
        char[] temp = new char[4];
        while (count > 0) {
            int num = read4(temp);
            if (num == 0) break;
            count -= num;
            for (int i = 0; i < num && idx < n; i++, idx++) {
                buf[idx] = temp[i];
            }
        }
        return idx;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。