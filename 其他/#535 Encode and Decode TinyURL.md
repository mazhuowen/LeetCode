[toc]

> Note: This is a companion problem to the [System Design](https://leetcode.com/discuss/interview-question/system-design/) problem: [Design TinyURL](https://leetcode.com/discuss/interview-question/124658/Design-a-URL-Shortener-(-TinyURL-)-System/).



TinyURL is a URL shortening service where you enter a URL such as `https://leetcode.com/problems/design-tinyurl` and it returns a short URL such as `http://tinyurl.com/4e9iAk`.

Design the `encode` and `decode` methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.



## 题目解读

&emsp;设计一个短链系统。

```java
public class Codec {

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        
    }
}
```

## 程序设计

* 由于题目只是输入一条链接，如果不考虑冲突问题，则可用字符串哈希编码来作为短链后缀。

```java
public class Codec {
    private final static String prefix = "http://tinyurl.com/";

    // 记录短链和原本网址
    private Map<String, String> urlRecord = new HashMap<>();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        String shortUrl = prefix + Integer.toString(longUrl.hashCode());
        urlRecord.put(shortUrl, longUrl);
        return shortUrl;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return urlRecord.get(shortUrl);
    }
}
```

* 可进一步优化，考虑冲突及存储空间等问题。

```java
public class Codec {
    private final static String prefix = "http://tinyurl.com/";

    // 记录短链后缀和原本网址
    private Map<String, String> short2long = new ConcurrentHashMap<>();
    private Map<String, String> long2short = new ConcurrentHashMap<>();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        // 已存在
        if (long2short.get(longUrl) != null) return long2short.get(longUrl);
        
        String suffix = Integer.toString(longUrl.hashCode());
        // 已存在，则继续生成
        while (short2long.get(suffix) != null) {
            suffix =  Integer.toString(suffix.hashCode());
        }
        // 未考虑线程安全
        short2long.put(suffix, longUrl);
        long2short.put(longUrl, prefix + suffix);
        return prefix + suffix;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return short2long.get(shortUrl.substring(prefix.length()));
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度主要是哈希表消耗。

执行用时：4ms，在所有java提交中击败了58.93%的用户。

内存消耗：39.6MB，在所有java提交中击败了12.50%的用户。

## 官方解题

&emsp;除了哈希码，官方还提出了随机数和固定长后缀的思路。固定长度后缀思路如下：

```java
public class Codec {
    // 后缀字符
    String alphabet = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    HashMap<String, String> map = new HashMap<>();
    Random rand = new Random();
    String key = getRand();

    public String getRand() {
        StringBuilder sb = new StringBuilder();
        // 固定长度为6
        for (int i = 0; i < 6; i++) {
            sb.append(alphabet.charAt(rand.nextInt(62)));
        }
        return sb.toString();
    }

    public String encode(String longUrl) {
        // 冲突解决
        while (map.containsKey(key)) {
            key = getRand();
        }
        map.put(key, longUrl);
        return "http://tinyurl.com/" + key;
    }

    public String decode(String shortUrl) {
        return map.get(shortUrl.replace("http://tinyurl.com/", ""));
    }
}
```

执行用时：6ms，在所有java提交中击败了51.43%的用户。

内存消耗：39.4MB，在所有java提交中击败了12.50%的用户。