[toc]





## 题目解读



```java

```

## 程序设计

* 最基本的思路是维护白名单，但是会内存超出题目限制。

```java
class Solution {
    private List<Integer> whiteList;
    private Random r;

    public Solution(int N, int[] blacklist) {
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < N; i++) {
            set.add(i);
        }
        for(int b : blacklist) {
            set.remove(b);
        }
        this.whiteList = new ArrayList<>(set);
        this.r = new Random();
    }
    
    public int pick() {
        return whiteList.get(r.nextInt(whiteList.size()));
    }
}
```

* 



## 性能分析



## 官方解题