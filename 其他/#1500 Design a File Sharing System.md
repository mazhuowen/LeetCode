[toc]

We will use a file-sharing system to share a very large file which consists of $m$ small **chunks** with IDs from $1$ to $m$.

When users join the system, the system should assign **a unique** ID to them. The unique ID should be used **once** for each user, but when a user leaves the system, the ID can be **reused** again.

Users can request a certain chunk of the file, the system should return a list of IDs of all the users who own this chunk. If the user receive a non-empty list of IDs, they receive the requested chunk successfully.


Implement the `FileSharing` class:

* `FileSharing(int m)` Initializes the object with a file of $m$ chunks.
* `int join(int[] ownedChunks)`: A new user joined the system owning some chunks of the file, the system should assign an id to the user which is the **smallest positive integer** not taken by any other user. Return the assigned id.
* `void leave(int userID)`: The user with `userID` will leave the system, you cannot take file chunks from them anymore.
* `int[] request(int userID, int chunkID)`: The user `userID` requested the file chunk with `chunkID`. Return a list of the IDs of all users that own this chunk sorted in ascending order.



**Follow-ups**:

* What happens if the system identifies the user by their IP address instead of their unique ID and users disconnect and connect from the system with the same IP?
* If the users in the system join and leave the system frequently without requesting any chunks, will your solution still be efficient?
* If all each user join the system one time, request all files and then leave, will your solution still be efficient?
* If the system will be used to share n files where the `i`th file consists of `m[i]`, what are the changes you have to do?



**Constraints**:

* $1 \le m \le 10^5$
* $0 \le \text{ownedChunks.length} \le \min(100, m)$
* $1 \le \text{ownedChunks[i]} \le m$
* Values of `ownedChunks` are unique.
* $1 \le \text{chunkID} \le m$
* `userID` is guaranteed to be a user in the system if you **assign** the IDs **correctly**. 
* At most $10^4$ calls will be made to `join`, `leave` and `request`.
* Each call to `leave` will have a matching call for `join`.



## 题目解读

&emsp;设计文件系统，要求用户可分派复用最小的`id`，每个用户可查自己所属的文件块，并返回文件块的所有用户。题目示例隐含当用户查询不属于自己的文件块后，该文件块会对该用户可见。

```java
class FileSharing {

    public FileSharing(int m) {

    }
    
    public int join(List<Integer> ownedChunks) {

    }
    
    public void leave(int userID) {

    }
    
    public List<Integer> request(int userID, int chunkID) {

    }
}

/**
 * Your FileSharing object will be instantiated and called as such:
 * FileSharing obj = new FileSharing(m);
 * int param_1 = obj.join(ownedChunks);
 * obj.leave(userID);
 * List<Integer> param_3 = obj.request(userID,chunkID);
 */
```

## 程序设计

* 需注意查询时，如果当前用户没有该文件块的权限，而文件块有用户列表，则需要将当前用户加入到文件块。

```java
class FileSharing {
    // 递增id
    int id;
    // 复用id
    PriorityQueue<Integer> queue;
    // 文件及用户块
    Map<Integer, Set<Integer>> filesToUser;
    // 用户及文件
    Map<Integer, Set<Integer>> userToFiles;

    public FileSharing(int m) {
        this.id = 1;
        this.queue = new PriorityQueue<>();
        this.filesToUser = new HashMap<>();
        for (int i = 1; i <= m; i++) filesToUser.put(i, new HashSet<>());
        this.userToFiles = new HashMap<>();
    }
    
    public int join(List<Integer> ownedChunks) {
        // 分派id
        int userID = queue.isEmpty() ? this.id++ : queue.poll();
        for (Integer chunkID : ownedChunks) filesToUser.get(chunkID).add(userID);
        userToFiles.put(userID, new HashSet<>(ownedChunks));
        return userID;
    }
    
    public void leave(int userID) {
        queue.add(userID);
        for (int chunkID : userToFiles.get(userID)) filesToUser.get(chunkID).remove(userID);
        userToFiles.remove(userID);
    }
    
    public List<Integer> request(int userID, int chunkID) {
        List<Integer> res = new LinkedList<>(filesToUser.get(chunkID));
        Collections.sort(res);
        // 即使用户没有当前文件块，查询后加入该文件块
        if (!res.isEmpty()) {
            filesToUser.get(chunkID).add(userID);
            userToFiles.get(userID).add(chunkID);
        }
        return res;
    }
}
```

## 性能分析

执行用时：223ms，在所有java提交中击败了15.00%的用户。

内存消耗：60.9MB，在所有java提交中击败了45.45%的用户。

## 官方解题

&emsp;暂无，密切关注。社区较快的思路只是用一个哈希表记录用户及文件列表，在现有测试用例较快。