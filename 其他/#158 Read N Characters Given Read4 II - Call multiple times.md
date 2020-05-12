[toc]

Given a file and assume that you can only read the file using a given method `read4`, implement a method `read` to read n characters. Your method `read` **may be called multiple times**.



Note:

* Consider that you **cannot** manipulate the file directly, the file is only accesible for `read4` but not for `read`.
* The `read` function may be called **multiple times**.
* Please remember to **RESET** your class variables declared in Solution, as static/class variables are **persisted across multiple test cases**. Please see here for more details.
* You may assume the destination buffer array, `buf`, is guaranteed to have enough space for storing n characters.
* It is guaranteed that in a given test case the same buffer `buf` is called by `read`.



## 题目解读

&emsp;`read4`一次可以读取4个字符，通过`read4`实现读取任意字符的方法。

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

* 由于$n$不一定是4的整数倍，需要寄存部分结果在数据结构中，等下次查询再利用。

```java
public class Solution extends Reader4 {
    // 寄存上次查询剩余的数据
    int start = 0, end = 0;
    char[] buffer = new char[4];

    public int read(char[] buf, int n) {
        if (n <= 0) return 0;

        // 将旧的结果加入
        int idx1, len = end - start;
        for (idx1 = 0; idx1 < len && idx1 < n; idx1++, start++) {
            buf[idx1] = buffer[start];
        }
        // 缓存中还有剩余，返回
        if (start < end) return idx1;

        // 缓存已加入新的结果，清空缓存
        start = end = 0;
        // 更新还需要的字符数，循环读取
        int count = n - idx1;
        int idx2 = 0, num = 0;
        char[] temp = new char[4];
        while (count > 0) {
            num = read4(temp);
            // 已读取完成
            if (num == 0) break;
            count -= num;

            // 赋值
            for (idx2 = 0; idx1 < n && idx2 < num; idx1++, idx2++) {
                buf[idx1] = temp[idx2];
            }
            // 超出n退出循环
            if (idx1 == n) break;
        }
        // 将剩余字符放入缓存，更新索引
        if (idx1 == n && idx2 < num) {
            for (; idx2 < num; end++, idx2++) {
                buffer[end] = temp[idx2];
            }
        }
        return idx1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。