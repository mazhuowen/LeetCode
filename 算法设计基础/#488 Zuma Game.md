[toc]

Think about Zuma Game. You have a row of balls on the table, colored red(R), yellow(Y), blue(B), green(G), and white(W). You also have several balls in your hand.

Each time, you may choose a ball in your hand, and insert it into the row (including the leftmost place and rightmost place). Then, if there is a group of 3 or more balls in the same color touching, remove these balls. Keep doing this until no more balls can be removed.

Find the minimal balls you have to insert to remove all the balls on the table. If you cannot remove all the balls, output -1.



**Constraints**:

* You may assume that the initial row of balls on the table won’t have any 3 or more consecutive balls with the same color.
* $1 \le \text{board.length} \le 16$
* $1 \le \text{hand.length} \le 5$
* Both input strings will be non-empty and only contain characters `'R'`,`'Y'`,`'B'`,`'G'`,`'W'`.



## 题目解读

&emsp;祖玛游戏。

```java
class Solution {
    public int findMinStep(String board, String hand) {

    }
}
```

## 程序设计

* 最初思路采用贪婪法，即统计连续的字符，如果可以和手中的字符构成多于3个，则消去。
* 该思路在复杂情况不成立，如`RRWWRRBBRR`和`WB`，如果根据连续字符拼接不管怎么处理，最后都剩余`RR`；而正确顺序是`RRWWRRBBRR->RRWWRRBBR(W)R->RRWWRRBB(B)R(W)R->RRWWRRR(W)R->RRWW(W)R->RRR->""`，这打破了上述贪婪思路。

```java
class Solution {
    int count = Integer.MAX_VALUE;

    public int findMinStep(String board, String hand) {
        if (board == null || board.length() == 0) return 0;

        // 统计字符数
        int[] counter = new int[26];
        for (char c : hand.toCharArray()) {
            counter[c - 'A']++;
        }
        findStep(board.toCharArray(), counter, 0);
        return count == Integer.MAX_VALUE ? -1 : count;
    }

    private void findStep(char[] board, int[] hand, int c) {
        // 剪枝
        if (c >= count) return;
        if (board.length == 0) {
            count = Math.min(count, c);
            return;
        }

        // 遍历数组中相邻且两个以上的字符
        int num = 1, left = 0, right = 0;
        // 统计相同字符数
        while (right < board.length - 1) {
            if (board[right] == board[++right]) num++;
            else {
                // 如果可以凑够三个及以上
                if (num + hand[board[left] - 'A'] >= 3) {
                    int step = 3 - num < 0 ? 1 : 3 - num;
                    hand[board[left] - 'A'] -= step;
                    char[] newBoard = copyArray(board, left, right);
                    findStep(newBoard, hand, c + step);
                    hand[board[left] - 'A'] += step;
                }
                num = 1;
                left = right;
            }
        }
        if (num + hand[board[left] - 'A'] >= 3) {
            int step = 3 - num < 0 ? 1 : 3 - num;
            hand[board[left] - 'A'] -= step;
            char[] newBoard = copyArray(board, left, right + 1);
            findStep(newBoard, hand, c + step);
            hand[board[left] - 'A'] += step;
        }
    }

    // 消去多于三个的连续字符
    private char[] copyArray(char[] array, int left, int right) {
        Stack<int[]> stack = new Stack<>();
        int size = array.length - right + left;
        for (int i = 0; i < array.length; i++) {
            if (i >= left && i < right) continue;
            // 相等则计数
            if (!stack.isEmpty() && stack.peek()[0] == array[i]) stack.peek()[1]++;
            else {
                if (!stack.isEmpty() && stack.peek()[1] >= 3) size -= stack.pop()[1];
                if (!stack.isEmpty() && stack.peek()[0] == array[i]) stack.peek()[1]++;
                else stack.push(new int[]{array[i], 1});
            }
        }
        while (!stack.isEmpty() && stack.peek()[1] >= 3) {
            size -= stack.pop()[1];
        }

        char[] res = new char[size];
        while (!stack.isEmpty()) {
            int[] cur = stack.pop();
            while (cur[1]-- > 0) {
                res[--size] = (char)(cur[0]);
            }
        }
        return res;
    }
}
```

* 参考社区思路，只需在相同的两个数字中间插入不同数字即可，修改如下，同时修改消去相同连续区间的方法。

```java
class Solution {
    int count = Integer.MAX_VALUE;

    public int findMinStep(String board, String hand) {
        if (board == null || board.length() == 0) return 0;

        // 计数
        int[] counter = new int[26];
        for (char c : hand.toCharArray()) counter[c - 'A']++;

        findStep(board.toCharArray(), counter, 0);
        return count == Integer.MAX_VALUE ? -1 : count;
    }

    private void findStep(char[] board, int[] hand, int c) {
        // 剪枝
        if (c >= count) return;
        // 空字符串
        if (board.length == 0) {
            count = c;
            return;
        }

        // 遍历数组中相邻且两个以上的字符
        int num = 1, left = 0, right = 0;;
        while (right < board.length - 1) {
            // 相邻字符相等
            if (board[right] == board[++right]) {
                // 尝试插入不同字符
                for (int i = 0; i < 26; i++) {
                    if (hand[i] == 0 || i == board[right] - 'A') continue;
                    // 生成新字符串
                    char[] newBoard = insertArray(board, right, (char)(i + 'A'));
                    hand[i]--;
                    findStep(newBoard, hand, c + 1);
                    hand[i]++;
                }
                num++;
            }
            // 相邻字符不相等，检查前面字符是否可拼接为3个以上
            else {
                if (num + hand[board[left] - 'A'] >= 3) {
                    // 需要拼接的数目
                    int insert = 3 - num <= 0 ? 1 : (3 - num);
                    hand[board[left] - 'A'] -= insert;
                    // 消去拼接后的字符串
                    char[] newBoard = copyArray(board, left, right);
                    findStep(newBoard, hand, c + insert);
                    hand[board[left] - 'A'] += insert;
                }
                // 重置计数和区间
                num = 1;
                left = right;
            }
        }
        // 最后一段相同字符处理
        if (num + hand[board[left] - 'A'] >= 3) {
            // 需要拼接的数目
            int insert = 3 - num <= 0 ? 1 : (3 - num);
            hand[board[left] - 'A'] -= insert;
            char[] newBoard = copyArray(board, left, right + 1);
            findStep(newBoard, hand, c + insert);
            hand[board[left] - 'A'] += insert;
        }
    }

    // 消去大于3的连续区间
    private char[] copyArray(char[] array, int left, int right) {
        // 闭区间转换为开区间
        left -= 1;
        int l = left, r = right;
        while (true) {
            // 检查右侧区间
            int count = 1;
            while (r < array.length - 1 && array[r] == array[r + 1]) {
                r++;
                count++;
            }
            while (l >= 0 && r < array.length && array[l] == array[r]) {
                l--;
                count++;
            }
            // 连续区间则更新
            if (count >= 3) {
                left = l;
                // r为闭区间，转化为开区间
                right = ++r;
            } 
            // 非连续区间则归位
            else {
                l = left;
                r = right;
            }
            
            count = 1;
            // 检查左侧区间
            while (l >= 1 && array[l] == array[l - 1]) {
                l--;
                count++;
            }
            while (l >= 0 && r < array.length && array[l] == array[r]) {
                r++;
                count++;
            }
            if (count >= 3) {
                // l为闭区间，转化为开区间
                left = --l;
                right = r;
            } else break;
        }
		// 复制[0,left]及[right, len]的值到新数组
        char[] res = new char[array.length - right + left + 1];
        if (left >= 0) System.arraycopy(array, 0, res, 0, left + 1);
        if (array.length - right > 0) System.arraycopy(array, right, res, left + 1, array.length - right);
        return res;
    }

    private char[] insertArray(char[] array, int idx, char c) {
        // 插入字符
        char[] res = new char[array.length + 1];
        if (idx >= 0) System.arraycopy(array, 0, res, 0, idx);
        res[idx] = c;
        if (array.length - idx >= 0) System.arraycopy(array, idx, res, idx + 1, array.length - idx);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度最坏为$O(N!)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了97.71%的用户。

内存消耗：37.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。