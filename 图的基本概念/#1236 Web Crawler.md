[toc]

Given a url `startUrl` and an interface `HtmlParser`, implement a web crawler to crawl all links that are under the **same hostname** as `startUrl`. 

Return all urls obtained by your web crawler in **any** order.

Your crawler should:

* Start from the page: startUrl
* Call `HtmlParser.getUrls(url)` to get all urls from a webpage of given url.
* Do not crawl the same link twice.
* Explore only the links that are under the **same hostname** as `startUrl`.



Constraints:

* $1 \le \text{urls.length} \le 1000$
* $1 \le \text{urls[i].length} \le 300$
* `startUrl` is one of the urls.
* Hostname label must be from 1 to 63 characters long, including the dots, may contain only the ASCII letters from 'a' to 'z', digits  from '0' to '9' and the hyphen-minus character ('-').
* The hostname may not start or end with the hyphen-minus character ('-'). 
* See:  https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames
* You may assume there're no duplicates in url library.



## 题目解读

&emsp;从一个链接开始爬虫，返回与起始链接域名相同的链接。

> 题目没有说明，但是根据结果还隐含了只爬虫相同域名的链接，不会爬取所有的链接。

```java
/**
 * // This is the HtmlParser's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface HtmlParser {
 *     public List<String> getUrls(String url) {}
 * }
 */
class Solution {
    public List<String> crawl(String startUrl, HtmlParser htmlParser) {
        
    }
}
```

## 程序设计

* 图的深度优先搜索遍历实现，需注意只爬取域名相同的链接。

```java
class Solution {
    public List<String> crawl(String startUrl, HtmlParser htmlParser) {
        List<String> res = new LinkedList<>();
        if (startUrl == null || startUrl.isEmpty()) return res;

        // 获取域名
        String domainUrl = startUrl.split("/")[2];
        Set<String> set = new HashSet<>();
        crawl(startUrl, "http://" + domainUrl, htmlParser, set, res);
        return res;
    }

    private void crawl(String startUrl, String domainUrl, HtmlParser htmlParser, Set<String> set, List<String> res) {
        // 不合法或已遍历，返回
        if (startUrl == null || startUrl.isEmpty() || set.contains(startUrl)) return;
        // 添加到已遍历
        set.add(startUrl);
        res.add(startUrl);
        // 递归遍历
        List<String> urls = htmlParser.getUrls(startUrl);
        if (urls == null) return;
        for (String url : urls) {
            // 注意爬虫域名相同的链接
            if (url.startsWith(domainUrl)) crawl(url, domainUrl, htmlParser, set, res);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，$N$为初始链接所在连通图的结点数目。

执行用时：4ms，在所有java提交中击败了98.57%的用户。

内存消耗：48MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有广度优先搜索的实现。