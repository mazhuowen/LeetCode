[toc]

Given two strings `str1` and `str2` of the same length, determine whether you can transform `str1` into `str2` by doing **zero or more** *conversions*.

In one conversion you can convert **all** occurrences of one character in `str1` to **any** other lowercase English character.

Return `true` if and only if you can transform `str1` into `str2`.



**Note:**

* $1 \le \text{str1.length} == \text{str2.length} \le 10^4$
* Both `str1` and `str2` contain only lowercase English letters.



## 题目解读

&emsp;给定两个字符串，判断字符串是否可以转换为另一个字符串。需注意题目规定转换是一次性发生的，即在字符串1中的同一个字母同时转换为另一个字母，这使得转换的顺序非常重要。

```java
class Solution {
    public boolean canConvert(String str1, String str2) {
    
    }
}
```

## 程序设计

* 如果不考虑转换顺序，假设字母是一个个转换，则可以用字典保存转换关系比对即可。题目中要求字母同时转换，比如`aabcc`和`ccdee`，如果首先`a`转换为`c`，则字符串是`ccbcc`，然后`c`转换为`e`，则变为`eebee`，显然不是我们需要的结果。
* 如果将转换关系看做边，则上述转换可以抽象为`a->c->e`，由依赖关系可知从`c`开始转换而不是从`a`转换。考虑其它情况，对于`a->x`，`b->x`的情况，只要`x`不在字符串1中这两个交换没有顺序问题。由题意可知字符串1中的同一个字母只能转换一次，即在字符串1中的字母出边为1，这也就意味着不能存在`a->x`，`a->y`的情况，可用字典记录判断。
* 考虑到跟复杂的情况，即`a->b->c->a`的情况，次时直接转换显然不行，可以考虑先将环中的某个字母转换为临时字母`x`，`x`在字符串1中不存在，则`a->b->c->x`，这样转换完后再将`x`转为`a`即可；现在问题是如果`x`不存在怎么办？不存在则没法打破环，从而没法转换。所以条件就是字符串字母数小于26（除非字符串相等）。

```java
class Solution {
    public boolean canConvert(String str1, String str2) {
        if (str1 == null || str2 == null || str1.length() != str2.length()) throw new IllegalArgumentException("invalid param");
        if (str1.equals(str2)) return true;
        // 记录转换关系
        Map<Character, Character> transMap = new HashMap<>();
        Set<Character> set = new HashSet<>();
        for (int i = 0; i < str1.length(); i++) {
            // 添加字符串2的字母是因为如果字符串2的字母数少于26，字符串1即使字母等于26，意味者存在多个字母转为同一个字母，合法，而如果字符串2字母数为26，则字符串1字母数必然为26（否则map判断会返回false），故根据字符串2判断环路
            set.add(str2.charAt(i));
            // 维护转换关系
            if (transMap.get(str1.charAt(i)) == null) {
                transMap.put(str1.charAt(i), str2.charAt(i));
            } else {
                if (transMap.get(str1.charAt(i)) != str2.charAt(i)) return false;
            }
        }
        return set.size() < 26;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.9MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;暂无，密切关注。