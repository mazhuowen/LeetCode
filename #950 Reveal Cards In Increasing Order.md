[toc]

In a deck of cards, every card has a unique integer.  You can order the deck in any order you want.

Initially, all the cards start face down (unrevealed) in one deck.

Now, you do the following steps repeatedly, until all cards are revealed:

Take the top card of the deck, reveal it, and take it out of the deck.
If there are still cards in the deck, put the next top card of the deck at the bottom of the deck.
If there are still unrevealed cards, go back to step 1.  Otherwise, stop.
Return an ordering of the deck that would reveal the cards in increasing order.

The first entry in the answer is considered to be the top of the deck.



**note:**

* $1 \le \text{A.length} \le 1000$
* $1 \le \text{A[i]} \le 10^6$
* $A[i] \ne A[j]$ for all $i \ne j$



##



```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {

    }
}
```

##

* 

```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        if (deck == null || deck.length <= 2) return deck;

        Arrays.sort(deck);
        List<Integer> temp = new LinkedList<>();
        for (int num : deck) {
            temp.add(num);
        }

        for (int i = deck.length - 3; i >= 0; i--) {
            int remove = temp.remove(deck.length - 1);
            temp.add(i + 1, remove);
        }

        int[] res = new int[deck.length];
        for (int i = 0; i < deck.length; i++) {
            res[i] = temp.get(i);
        }
        return res;
    }
}
```

##



执行用时 :9 ms, 在所有 Java 提交中击败了10.40%的用户

内存消耗 :40.1 MB, 在所有 Java 提交中击败了50.00%的用户

## 

