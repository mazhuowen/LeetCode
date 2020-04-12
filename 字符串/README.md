[toc]

&emsp;首先对于字符串匹配问题，定义主串和模式串；根据主串匹配模式串的数目分为单模式匹配和多模式匹配。

## 单模式串匹配

### BF（Brute Force）暴力匹配算法

&emsp;假设主串长度为$n$，模式串长度为$m$，本质思想是在主串起始位置$0,1,\dots,n -m$的$m$个子串是否和模式串匹配。BF算法的时间复杂度是$O(NM)$，但在实际的开发中，却是比较常用的字符串匹配算法。因为大部分情况下模式串和主串的长度都不会太长。

<img src="../images/字符串1.jpg" style="zoom:50%;" />

* 简单[#14 Longest Common Prefix](./#14 Longest Common Prefix.md)    字符串遍历匹配
* 简单[#392 Is Subsequence](./#392 Is Subsequence.md)    字符串遍历匹配
* 繁杂[#408 Valid Word Abbreviation](./#408 Valid Word Abbreviation.md)    变形的字符串遍历匹配，[#320 Generalized Abbreviation](../算法设计基础/#320 Generalized Abbreviation.md)与[#411 Minimum Unique Word Abbreviation](../算法设计基础/#411 Minimum Unique Word Abbreviation.md)的相关主题

### RK（Rabin-Karp）算法

&emsp;通过哈希算法对主串中的$n - m + 1$个子串分别求哈希值，然后逐个与模式串的哈希值比较大小。如果某个子串的哈希值与模式串相等，在不考虑哈希冲突的情况下，说明对应的子串和模式串匹配。RK算法重点在于哈希算法的设计，以全部小写字母组成的字符串为例，把字母映射为数字，小写字母有26个，可以认为是26进制的数字，这样哈希算法就是这段字母表示的数字大小。这样设计的好处还有以$m$长度的窗口滑动时不需要重新遍历计算当前窗口的字母，当前窗口的哈希值与上一窗口有关联。这样总的时间复杂度就是$O(N)$。

<img src="../images/字符串2.jpg" style="zoom:50%;" />



### BM（Boyer-Moore）算法

&emsp;在某些极端情况下，BF算法性能会退化，而RK算法需要用到哈希算法，而设计一个可以应对各种类型字符的哈希算法并不简单。BM算法包含两部分，分别是坏字符规则（bad character rule）和好后缀规则（good suffix shift）。

* 坏字符规则

&emsp;BM算法的匹配顺序按照模式串下标从大到小的顺序倒着匹配，当发现某个字符没法匹配的时候，把这个没有匹配的字符叫作坏字符。然后使用坏字符在模式串中查找。

<img src="../images/BM.jpg" style="zoom:50%;" />

<img src="../images/BM1.jpg" style="zoom:50%;" />

如果坏字符在模式串中存在，设坏字符对应的模式串中的字符下标记作$s_i$，坏字符在模式串中的下标记作$x_i$；如果坏字符在模式串中不存在，则$x_i$ 记作-1。这两种情况模式串往后移动的位数就等于$s_i - x_i$。需注意如果坏字符在模式串里多处出现，$x_i$选择最后面那个。

> 由于只需记录最后出现的位置，可以使用散列表记录模式串中每个字符出现的最后的位置，避免遍历搜索。

&emsp;利用坏字符规则，BM算法在最好情况下的时间复杂度是$O(n/m)$。比如主串是`aaabaaabaaabaaab`，模式串是`aaaa`。每次比对模式串都可以直接后移四位。单纯使用坏字符规则还不够，因为根据$s_i - x_i$计算出来的移动位数有可能是负数，BM算法还需要用到好后缀规则。

* 好后缀规则

<img src="../images/BM2.jpg" style="zoom:50%;" />

<img src="../images/BM3.jpg" style="zoom:50%;" />

&emsp;思路类似坏字符，将好后缀在模式串中查找，如果找到了另一个跟好后缀相匹配的子串，就将模式串滑动到这个子串与主串中好后缀对齐的位置。如果在模式串中找不到另一个等于好后缀的子串，需要根据情况判断：好后缀中的后缀子串与模式串开头的前缀子串没有重合，则可以直接移动到好后缀后面；如果有，则需要将前缀子串与后缀子串对齐。

<img src="../images/BM4.jpg" style="zoom:50%;" />

当模式串和主串中的某个字符不匹配的时，可分别计算好后缀和坏字符往后滑动的位数，然后取两个数中最大的作为模式串往后滑动的位数。这种处理方法可以避免根据坏字符规则计算后移位数是负数的情况。

> 为了加快匹配后缀子串，可以引入数组`Suffix`，索引对应后缀子串的长度，数组值表示与好后缀的匹配的子串的起始索引。存在多个后缀子串匹配则选择索引较大（后缀长度较长的）的。其次还需引入`Prefix`数组来记录后缀子串是否匹配好后缀的后缀子串。
>
> <img src="../images/BM5.jpg" style="zoom:50%;" />

```java
    public int bm(String source, String pattern) {
        char[] s = source.toCharArray();
        char[] p = pattern.toCharArray();
        int n = s.length, m = p.length;

        // 构建坏字符索引表（假设全部由小写字母构成），初始化为-1，只记录每个字母最后出现的位置
        int[] bc = new int[26];
        Arrays.fill(bc, -1);
        for (int i = 0; i < m; i++) {
            bc[p[i] - 'a'] = i;
        }

        // 好后缀
        int[] suffix = new int[m];
        boolean[] prefix = new boolean[m];
        Arrays.fill(suffix, -1);
        // 初始化，求模式串子串0~i与模式串0~m-1的公共后缀子串，如果存在，若长度为k，则suffix[k] = 0~i中的子串起始索引
        for (int i = 0; i < m - 1; ++i) {
            // j从后往前遍历，k为公共后缀子串长度
            int j = i, k = 0;
            // 后缀相等，往前遍历
            while (j >= 0 && p[j] == p[m - 1 - k]) {
                suffix[k++] = j-- + 1;
            }
            // 如果公共后缀子串也是模式串的前缀子串
            if (j == -1) prefix[k] = true;
        }

        // 比较的主串起始索引
        int i = 0;
        while (i <= n - m) {
            int j;
            // 倒序比较
            for (j = m - 1; j >= 0; j--) {
                // 坏字符
                if (s[i + j] != p[j]) break;
            }
            // 匹配成功
            if (j < 0) return i;
            // 计算后移距离
            int offset1 = j - bc[s[i + j] - 'a'];

            // 如果有好后缀，则计算
            int offset2 = 0;
            if (j < m - 1) {
                offset2 = moveByGS(j, m, suffix, prefix);
            }

            i += Math.max(offset1, offset2);
        }
        // 未匹配到
        return -1;
    }

    private int moveByGS(int j, int m, int[] suffix, boolean[] prefix) {
        // 好后缀长度
        int k = m - 1 - j;
        // 返回对齐所需偏移
        if (suffix[k] != -1) return j - suffix[k] +1;
        // 如果后缀与前缀重叠
        for (int r = j + 2; r <= m-1; ++r) {
            if (prefix[m - r]) return r;
        }
        // 好后缀不匹配
        return m;
    }
```



### KMP算法



## 多模式串匹配

### Trie树



### AC自动机



* $\clubs$技巧[#466 Count The Repetitions](./#466 Count The Repetitions.md)    循环节规律，周期化字符串



## 其它

* 简单[#38 Count and Say](./#38 Count and Say.md)    字符串递归计数