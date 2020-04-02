[toc]

Design a Leaderboard class, which has 3 functions:

* `addScore(playerId, score)`: Update the leaderboard by adding score to the given player's `score`. If there is no player with such id in the leaderboard, add him to the leaderboard with the given `score`.
* `top(K)`: Return the score sum of the top `K` players.
* `reset(playerId)`: Reset the score of the player with the given id to 0. It is guaranteed that the player was added to the leaderboard before calling this function.

Initially, the leaderboard is empty.



Constraints:

* $1 \le playerId, K \le 10000$
* It's guaranteed that `K` is less than or equal to the current number of players.
* $1 \le score \le 100$
* There will be at most `1000` function calls.



## 题目解读

&emsp;设计排名榜，支持分数变更和移除，可以返回top k的分数和。

```java
class Leaderboard {

    public Leaderboard() {

    }
    
    public void addScore(int playerId, int score) {

    }
    
    public int top(int K) {

    }
    
    public void reset(int playerId) {

    }
}

/**
 * Your Leaderboard object will be instantiated and called as such:
 * Leaderboard obj = new Leaderboard();
 * obj.addScore(playerId,score);
 * int param_2 = obj.top(K);
 * obj.reset(playerId);
 */
```

## 程序设计

* 初步的想法是使用堆来动态排序，由于需要变更分数，而从堆中取出某个分数代价很大，可采用字典记录每个用户的分数，堆中只需每次维护新的分数。同理，移除可以使用懒移除，只在字典中移除。
* 由于堆中存在移除的分数及一个用户的历史分数，只是简单的取前`k`个不可行，通过与字典判断，判断分数值是否一致来确定是否是最新分数，在取出`k`个分数的同时，将移除的、旧的分数删除。

```java
class Leaderboard {
    // 分数最大堆
    private PriorityQueue<int[]> queue;
    // 分数记录
    private Map<Integer, Integer> record;
    // 临时存储topK
    private List<int[]> temp;

    public Leaderboard() {
        this.queue = new PriorityQueue<>((a, b) -> b[1] - a[1]);
        this.record = new HashMap<>();
        this.temp = new LinkedList<>();
    }
    
    public void addScore(int playerId, int score) {
        if (score <= 0) return;
        // 加入记录
        record.put(playerId, record.getOrDefault(playerId, 0) + score);
        // 加入堆
        queue.add(new int[]{playerId, record.get(playerId)});
    }
    
    public int top(int K) {
        int sum = 0;

        while (!queue.isEmpty() && K > 0) {
            int[] cur = queue.poll();
            // 最新分数，更新；如果不是最新分数则出队删除
            if (record.getOrDefault(cur[0], 0) == cur[1]) {
                sum += cur[1];
                temp.add(cur);
                K--;
            }
        }
		
        // 将出队的top k元素重新入队
        queue.addAll(temp);
        temp.clear();

        return sum;
    }
    
    public void reset(int playerId) {
        // 懒删除，从记录
        record.remove(playerId);
    }
}
```

## 性能分析

&emsp;添加及变更分数时间复杂度为$O(\log_2N)$，移除分数时间复杂度为$O(1)$，返回top k时间复杂度为$O(K\log_2N)$；空间复杂度为$O(N)$，$N$为操作次数。

执行用时：24ms，在所有java提交中击败了64.47%的用户。

内存消耗：40.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区没有较统一的方法，大部分思路是双哈希表，一个哈希表记录用户和分数，一个哈希表记录分数和该分数的用户数，每次查找top k都对第二个哈希表排序求和，由于分数在100以内，大大少于用户数，所以时间性能较好。

