[toc]

We are given some website visits: the user with name `username[i]` visited the website `website[i]` at time `timestamp[i]`.

A 3-sequence is a list of websites of length 3 sorted in ascending order by the time of their visits.  (The websites in a 3-sequence are not necessarily distinct.)

Find the 3-sequence visited by the largest number of users. If there is more than one solution, return the lexicographically smallest such 3-sequence.



Note:

* $3 \le N = \text{username.length} = \text{timestamp.length} = \text{website.length} \le 50$
* $1 \le \text{username[i].length} \le 10$
* $0 \le \text{timestamp[i]} \le 10^9$
* $1 \le \text{website[i].length} \le 10$
* Both `username[i]` and `website[i]` contain only lowercase characters.
* It is guaranteed that there is at least one user who visited at least 3 websites.
* No user visits two websites at the same time.



## 题目解读

&emsp;找到用户的共性行为路径，也就是有最多的用户都**至少按某种次序访问过一次**的三个页面路径。需要注意的是，用户**可能不是连续访问**这三个路径的。

```java
class Solution {
    public List<String> mostVisitedPattern(String[] username, int[] timestamp, String[] website) {

    }
}
```

## 程序设计

* 根据用户id和时间戳排序，然后遍历生成可能的路径三元组，加入字典。最后统计字典计数。

```java
class Solution {
    public List<String> mostVisitedPattern(String[] username, int[] timestamp, String[] website) {
        List<String> res = new LinkedList<>();
        if(username == null || username.length < 3) {
            return res;
        }
        // 索引数组
        Integer[] idx = new Integer[username.length];
        for (int i = 0; i < idx.length; i++) {
            idx[i] = i;
        }
        // 根据用户id和时间戳排序
        Arrays.sort(idx, (a, b) -> username[a].compareTo(username[b]) == 0 ?
                timestamp[a] - timestamp[b] : username[a].compareTo(username[b]));
        // 创建路径map，注意值为集合，由于一个用户可能存在多条访问路径相同，使用用户集合还不是计数
        Map<String, Set<String>> counter = new HashMap<>();
        // 记录最大计数
        int maxCount = 0;
        // 记录计数相同，字典序列较小的路径
        String minOrder = website[idx[0]] + "#" + website[idx[1]] + "#" + website[idx[2]];;
        int cur = 0, next, last;
        while (cur < idx.length - 2) {
            next = cur + 1;
            // 同一个用户
            while (next < idx.length - 1 && username[idx[cur]].equals(username[idx[next]])) {
                last = next + 1;
                while (last < idx.length && username[idx[cur]].equals(username[idx[last]])) {
                    // 当前路径
                    String path = website[idx[cur]] + "#" + website[idx[next]] + "#" + website[idx[last]];
                    Set<String> temp = counter.getOrDefault(path, new HashSet<>());
                    // 加入用户
                    temp.add(username[idx[cur]]);
                    counter.put(path, temp);
                    // 统计用户，数目大则赋予新值
                    if (counter.get(path).size() > maxCount) {
                        maxCount = counter.get(path).size();
                        minOrder = path;
                    }
                    // 数目一致则判断字典序
                    else if (counter.get(path).size() == maxCount) {
                        minOrder = path.compareTo(minOrder) < 0 ? path : minOrder;
                    }
                    last++;
                }
                next++;
            }
            cur++;
        }
        res = Arrays.asList(minOrder.split("#"));
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N)$。

执行用时：43ms，在所有java提交中击败了21.74%的用户。

内存消耗：41.9MB，在所有java提交中击败了6.25%的用户。

## 官方解题

&emsp;暂无，密切关注。

#### 执行用时为 22 ms 的范例

```java
class Solution {
    /**
        inner class
    */
    class Log {
        String username;
        int timestamp;
        String website;
        
        public Log(String _username, int _timestamp, String _website) {
            username = _username;
            timestamp = _timestamp;
            website = _website;
        } 
    }
    
    /**
        inner class
        Comparator for Log
    */
    class LogComparator implements Comparator<Log> {        
        @Override
        public int compare(Log log1, Log log2) {
            if( log1.timestamp < log2.timestamp ) {
                return -1;
            }
            else if( log1.timestamp > log2.timestamp ) {
                return 1;
            }
            return 0;
        }        
    }
    
    public List<String> mostVisitedPattern(String[] username, int[] timestamp, String[] website) {
        
        // Create log
        int len = timestamp.length;
        List<Log> logs = new ArrayList<Log>();
        for( int i = 0; i < len; i++ ) {
            logs.add(new Log(username[i], timestamp[i], website[i]));
        }
        
        // Use comparator to sort the log
        Collections.sort(logs, new LogComparator());
        
        // printLogs(logs);
        
        // HashMap<Username, List<String>> map1
        HashMap<String, List<String>> userVisitMap = new HashMap<String, List<String>>();
        
        // HashMap<3-Sequence, Integer> map2
        HashMap<List<String>, Integer> patternTimesMap = new HashMap<List<String>, Integer>();
        
        // memory: O(N)
        
        // iterate timestamp and build map1
        for( int i = 0; i < logs.size(); i++ ) {
            List<String> visitPattern = userVisitMap.getOrDefault(logs.get(i).username, new ArrayList<String>());
            visitPattern.add(logs.get(i).website);
            userVisitMap.put(logs.get(i).username, visitPattern);
        }
        
        List<String> mostVisitPattern = new ArrayList<String>();
        int maxFrequency = Integer.MIN_VALUE;
        // iterate userVisitMap and generate all 3-sequence pattern
        // N
        for( String user : userVisitMap.keySet() ) {
            List<String> visitPattern = userVisitMap.get(user);
            // get all 3-sequence pattern of a user
            // < N
            // C(n, 3) = n(n-1)(n-2)
            int size = visitPattern.size();
            // Make sure a certain for one user can only count for once
            HashSet<List<String>> set = new HashSet<List<String>>();
            for( int i = 0; i <= size - 3; i++ ) {
                for( int j = i + 1; j <= size - 2; j++ ) {
                    for( int k = j + 1; k <= size - 1; k++ ) {
                        List<String> threeSequence = new ArrayList<String>();
                        threeSequence.add(visitPattern.get(i));
                        threeSequence.add(visitPattern.get(j));
                        threeSequence.add(visitPattern.get(k));
                        if( set.contains(threeSequence) ) {
                            continue;
                        }
                        set.add(threeSequence);
                        int times = patternTimesMap.getOrDefault(threeSequence, 0);
                        times++;
                        patternTimesMap.put(threeSequence, times);
                        if( times > maxFrequency ) {
                            maxFrequency = times;
                            mostVisitPattern = new ArrayList<String>(threeSequence);
                        }
                        else if( times == maxFrequency ) {
                            // If there is more than one solution, 
                            // return the lexicographically smallest such 3-sequence.
                            mostVisitPattern = new ArrayList<String>(getAlphabetSequence(mostVisitPattern, threeSequence));
                        }
                    }
                }
            }      
            
        }
        return mostVisitPattern;      
    }
        
    /**
        return the lexicographically smallest such 3-sequence.
    */
    private List<String> getAlphabetSequence(List<String> mostVisitPattern, List<String> threeSequence) {        
        for( int i = 0; i < 3; i++ ) {            
            // Compare every website
            int index = 0;
            String word1 = mostVisitPattern.get(i);
            String word2 = threeSequence.get(i);
            while( index < word1.length() && index < word2.length() ) {
                if( word1.charAt(index) < word2.charAt(index) ) {
                    return mostVisitPattern;
                }
                else if( word1.charAt(index) > word2.charAt(index) ) {
                    return threeSequence;
                }
                
                // word1.charAt(index) == word2.charAt(index)
                index++;         
            }
            if( index == word1.length() && index < word2.length() ) {
                return mostVisitPattern;
            }
            if( index == word2.length() && index < word1.length() ) {
                return threeSequence;
            }
        }       
        return mostVisitPattern;      
    }
    
    /**
        print all logs
    */
    private void printLogs( List<Log> logs ) {
        for( int i = 0; i < logs.size(); i++ ) {
            Log log = logs.get(i);
            System.out.println(log.username +" " + log.timestamp + " " + log.website);
        }
    }
   
}
```