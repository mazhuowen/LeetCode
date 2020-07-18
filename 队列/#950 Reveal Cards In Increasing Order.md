[toc]

In a deck of cards, every card has a unique integer.  You can order the deck in any order you want.

Initially, all the cards start face down (unrevealed) in one deck.

Now, you do the following steps repeatedly, until all cards are revealed:

* Take the top card of the deck, reveal it, and take it out of the deck.
* If there are still cards in the deck, put the next top card of the deck at the bottom of the deck.
* If there are still unrevealed cards, go back to step 1.  Otherwise, stop.

Return an ordering of the deck that would reveal the cards in **increasing order**.

The first entry in the answer is considered to be the top of the deck.



**note:**

* $1 \le \text{A.length} \le 1000$
* $1 \le \text{A[i]} \le 10^6$
* $A[i] \ne A[j]$ for all $i \ne j$



## 题目解读

&emsp;牌组中的每张卡牌都对应有一个唯一的整数。重复执行以下步骤，直到显示所有卡牌为止：

* 从牌组顶部抽一张牌，显示它，然后将其从牌组中移出。
* 如果牌组中仍有牌，则将下一张处于牌组顶部的牌放在牌组的底部。
* 如果仍有未显示的牌，那么返回步骤1。否则停止行动。

返回能以递增顺序显示卡牌的原始牌组顺序。

```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {

    }
}
```

## 程序设计

* 逆向工程模拟，对于最后有序的牌，从后往前模拟，发现每次只需将尾部的牌插入到当前牌的后面，正向模拟结果是有序的。

```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        if (deck == null || deck.length <= 2) return deck;

        // 排序
        Arrays.sort(deck);
        List<Integer> temp = new LinkedList<>();
        for (int num : deck) {
            temp.add(num);
        }

        // 倒序遍历，将当前数据尾的数拆插入当前位置之后
        for (int i = deck.length - 3; i >= 0; i--) {
            int remove = temp.remove(deck.length - 1);
            temp.add(i + 1, remove);
        }

        // 返回
        int[] res = new int[deck.length];
        for (int i = 0; i < deck.length; i++) {
            res[i] = temp.get(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了10.40%的用户。

内存消耗：40.1MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方采用正向模拟的方式，只需将最小的牌放到下标为0的位置，1放到队尾；次小的放在2的位置，3放到队尾；依次类推。

```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        int N = deck.length;
        // 加入所有索引
        Queue<Integer> index = new LinkedList();
        for (int i = 0; i < N; ++i)
            index.add(i);

        // 排序
        int[] ans = new int[N];
        Arrays.sort(deck);
        // 从低到高遍历
        for (int card: deck) {
            // 加入当前牌
            ans[index.poll()] = card;
            // 将之后的索引出队加入队尾，较大值会占用
            if (!index.isEmpty())
                index.add(index.poll());
        }

        return ans;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了72.15%的用户。

内存消耗：39.8MB，在所有java提交中击败了50.00%的用户。