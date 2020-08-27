[toc]

Design an algorithm to encode **a list of strings** to **a string**. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:

```c
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
```

Machine 2 (receiver) has the function:

```c
vector<string> decode(string s) {
  //... your code
  return strs;
}
```

So Machine 1 does:

```c
string encoded_string = encode(strs);
```

and Machine 2 does:

```c
vector<string> strs2 = decode(encoded_string);
```

`strs2` in Machine 2 should be the same as `strs` in Machine 1.

Implement the `encode` and `decode` methods.

 

**Note**:

* The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.
* Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.
* Do not rely on any library method such as `eval` or serialize methods. You should implement your own encode/decode algorithm.



## 题目解读

&emsp;编解码字符串。

```java
public class Codec {

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(strs));
```

## 程序设计

* 由于字符串包含任意字符，使用特殊字符作为分隔符的方法不会有效。参考官方解题，采用分块编码，前面四个字节表示后续字符串的长度。

```java
public class Codec {
  // Encodes string length to bytes string
  public String intToString(String s) {
    int x = s.length();
    char[] bytes = new char[4];
    // 32位数字分割为4个8位字符
    for(int i = 3; i > -1; --i) {
      bytes[3 - i] = (char) (x >> (i * 8) & 0xff);
    }
    return new String(bytes);
  }

  // Encodes a list of strings to a single string.
  public String encode(List<String> strs) {
    StringBuilder sb = new StringBuilder();
    for(String s: strs) {
      sb.append(intToString(s));
      sb.append(s);
    }
    return sb.toString();
  }

  // Decodes bytes string to integer
  public int stringToInt(String bytesStr) {
    int result = 0;
    // 将四个字符重新拼接为32位数字
    for(char b : bytesStr.toCharArray())
      result = (result << 8) + (int)b;
    return result;
  }

  // Decodes a single string to a list of strings.
  public List<String> decode(String s) {
    int i = 0, n = s.length();
    List<String> output = new ArrayList();
    while (i < n) {
      int length = stringToInt(s.substring(i, i + 4));
      i += 4;
      output.add(s.substring(i, i + length));
      i += length;
    }
    return output;
  }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(MN)$。

执行用时：5ms，在所有java提交中击败了95.36%的用户。

内存消耗：40.6MB，在所有java提交中击败了50.79%的用户。

## 官方解题

&emsp;见上。