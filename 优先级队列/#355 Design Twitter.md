[toc]

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

* `postTweet(userId, tweetId)`: Compose a new tweet.
* `getNewsFeed(userId)`: Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
* `follow(followerId, followeeId)`: Follower follows a followee.
* `unfollow(followerId, followeeId)`: Follower unfollows a followee.



## 题目解读

&emsp;设计一个简化版的推特(Twitter)，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近十条推文。

```java
class Twitter {

    /** Initialize your data structure here. */
    public Twitter() {
        
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```

## 程序设计

* 首先关注关系、发布推文可以用字典保存，对于关注、取关、发推就是对字典值的操作；对于获取最新推文，就是top-k的问题，将关注者和本人的推文入队判断，最后堆中的推文就是最新的10条。
* 注意推文id虽然不重复，但是不代表发推时间，还需要维护推文的先后顺序，可用全局变量记录。

```java
class Twitter {
    // 维护推特编号按照时间顺序
   private int count;
    // 记录关注关系
    private Map<Integer, Set<Integer>> followRecord;
    // 记录发布者及其推文
    private Map<Integer, List<Pair>> postRecord;

    /** Initialize your data structure here. */
    public Twitter() {
        count = 0;
        this.followRecord = new HashMap<>();
        this.postRecord = new HashMap<>();
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        List<Pair> tweets = postRecord.get(userId);
        if(tweets == null) {
            tweets = new LinkedList<>();
        }
        // 加入链表，最新的在最后面
        tweets.add(new Pair(tweetId, ++count));
        // 维护字典
        postRecord.put(userId, tweets);
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        Set<Integer> followees = followRecord.get(userId);
        if(followees == null) {
            followees = new HashSet<>();
        }
        // 加入自己
        followees.add(userId);
        // 最新的10条也就是tweetId最大的十条，使用最小堆
        PriorityQueue<Pair> queue = new PriorityQueue<>(10, (a, b) -> a.getValue() - b.getValue());
        for(Integer user : followees) {
            List<Pair> tweets = postRecord.get(user);
            if(tweets == null) {
                continue;
            }
            for(int i = tweets.size() - 1; i >= 0; i--) {
                if(queue.size() == 10) {
                    // 链表中推特id小于堆顶，不符合要求，终止循环
                    if(tweets.get(i).getValue() < queue.peek().getValue()) {
                        break;
                    } 
                    // 推特id不重复
                    else {
                        queue.poll();
                        queue.add(tweets.get(i));
                    }
                } else {
                    queue.add(tweets.get(i));
                }
            }
        }
        List<Integer> res = new LinkedList<>();
        while(!queue.isEmpty()) {
            res.add(queue.poll().getKey());
        }
        Collections.reverse(res);
        return res;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        Set<Integer> followees = followRecord.get(followerId);
        if(followees == null) {
            followees = new HashSet<>();
        }
        // 添加
        followees.add(followeeId);
        followRecord.put(followerId, followees);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        Set<Integer> followees = followRecord.get(followerId);
        // 移除
        if(followees != null && followees.contains(followeeId)) {
            followees.remove(followeeId);
        }
        followRecord.put(followerId, followees);
    }
}
// 辅助类
class Pair{
    int key;
    int value;

    Pair(int key, int value) {
        this.key = key;
        this.value = value;
    }

    public int getKey() {
        return this.key;
    }

    public int getValue() {
        return this.value;
    }
}
```

## 性能分析

&emsp;发推、关注、取关都是$O(1)$时间复杂度，获取推文假设关注者为$N$，则需要遍历$N$条链表最新的$k$条推特，每次遍历可能入队，时间复杂度为$O(\log_2k)$，总的时间复杂度为$O(kN\log_2k)$。

执行用时：43ms，在所有java提交中击败了45.07%的用户。

内存消耗：54.5MB，在所有java提交中击败了5.30%的用户。

## 官方解题

&emsp;暂无，一切关注。社区中运行时间较短的方法，不使用链表记录推文，而是定义推文类，实质上就是链表，里面增加了记录时间、发推人等信息。