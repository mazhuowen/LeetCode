[toc]

Given a url `startUrl` and an interface `HtmlParser`, implement **a Multi-threaded web crawler** to crawl all links that are under the **same hostname** as `startUrl`. 

Return all urls obtained by your web crawler in **any** order.

Your crawler should:

* Start from the page: `startUrl`
* Call `HtmlParser.getUrls(url)` to get all urls from a webpage of given url.
* Do not crawl the same link twice.
* Explore only the links that are under the **same hostname** as `startUrl`.



As shown in the example url above, the hostname is `example.org`. For simplicity sake, you may assume all urls use **http protocol** without any **port** specified. For example, the urls `http://leetcode.com/problems` and `http://leetcode.com/contest` are under the same hostname, while urls `http://example.org/test` and `http://example.com/abc` are not under the same hostname.

The HtmlParser interface is defined as such: 

```java
interface HtmlParser {
  // Return a list of all urls from a webpage of given url.
  // This is a blocking call, that means it will do HTTP request and return when this request is finished.
  public List<String> getUrls(String url);
}
```

Note that `getUrls(String url)` simulates performing a HTTP request. You can treat it as a blocking function call which waits for a HTTP request to finish. It is guaranteed that `getUrls(String url)` will return the urls within **15ms**.  Single-threaded solutions will exceed the time limit so, can your multi-threaded web crawler do better?

Below are two examples explaining the functionality of the problem, for custom testing purposes you'll have three variables `urls`, `edges` and `startUrl`. Notice that you will only have access to `startUrl` in your code, while `urls` and `edges` are not directly accessible to you in code.



**Follow up**:

* Assume we have 10,000 nodes and 1 billion URLs to crawl. We will deploy the same software onto each node. The software can know about all the nodes. We have to minimize communication between machines and make sure each node does equal amount of work. How would your web crawler design change?
* What if one node fails or does not work?
* How do you know when the crawler is done?



**Constraints**:

* $1 \le \text{urls.length} \le 1000$
* $1 \le \text{urls[i].length} \le 300$
* `startUrl` is one of the `urls`.
* Hostname label must be from 1 to 63 characters long, including the dots, may contain only the ASCII letters from 'a' to 'z', digits from '0' to '9' and the hyphen-minus character ('-').
* The hostname may not start or end with the hyphen-minus character ('-'). 
* See:  https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames
* You may assume there're no duplicates in url library.



## 题目解读

&emsp;多线程爬取网页链接。

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

* 最基本的，考虑使用并发集合类记录是否已爬取，但是查询是否爬取及加入集合两步操作并不是原子的，必须加入锁，如上分析，锁的粒度只需覆盖这两步即可。

```java
class Solution {

    public List<String> crawl(String startUrl, HtmlParser htmlParser) {
        // 记录链接是否爬取过
        Set<String> record = new HashSet<>();
        // 独占锁
        ReentrantLock lock = new ReentrantLock();
        
        Thread thread = new Worker(startUrl, htmlParser, record, lock);
        thread.start();

        try {
            thread.join();
        } catch (InterruptedException e) {
        
        }

        List<String> res = new LinkedList<>(record);
        return res;
    }
}

class Worker extends Thread {
    String url;
    HtmlParser htmlParser;
    Set<String> record;
    ReentrantLock lock;

    Worker(String url, HtmlParser htmlParser, Set<String> record, ReentrantLock lock) {
        this.url = url;
        this.htmlParser = htmlParser;
        this.record = record;
        this.lock = lock;
    }

    @Override
    public void run() {
        // 加锁维护是否爬取关系
        lock.lock();
        try {
            // 已经被其他线程爬取
            if (record.contains(url)) return;
            // 当前线程爬取当前链接
            record.add(url);
        } finally {
            lock.unlock();
        }
        
        String curHost = getHost(url);
        List<Thread> threads = new LinkedList<>();
        for (String nextUrl : htmlParser.getUrls(url)) {
            // 已爬取或域名不相等
            if (record.contains(nextUrl) || !curHost.equals(getHost(nextUrl))) continue;
            Thread thread = new Worker(nextUrl, htmlParser, record, lock);
            thread.start();
            threads.add(thread);
        }

        // 等待子线程结束
        for (Thread thread : threads) {
            try {
                thread.join();
            } catch (InterruptedException e) {

            }
        }
    }

    private String getHost(String url) {
        if (url.startsWith("http://")) {
            url = url.substring(7);
        } else if (url.startsWith("https://")) {
            url = url.substring(8);
        } else throw new IllegalArgumentException("invalid param");
        
        int end = url.indexOf("/");
        if (end != -1) url = url.substring(0, end);
        return url;
    }
}
```

## 性能分析

执行用时：5ms，在所有java提交中击败了98.75%的用户。

内存消耗：55.6MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。