[toc]

In LeetCode Store, there are some kinds of items to sell. Each item has a price.

However, there are some special offers, and a special offer consists of one or more different kinds of items with a sale price.

You are given the each item's price, a set of special offers, and the number we need to buy for each item. The job is to output the lowest price you have to pay for **exactly** certain items as given, where you could make optimal use of the special offers.

Each special offer is represented in the form of an array, the last number represents the price you need to pay for this special offer, other numbers represents how many specific items you could get if you buy this offer.

You could use any of special offers as many times as you want.



**Note**:

* There are at most 6 kinds of items, 100 special offers.
* For each item, you need to buy at most 6 of them.
* You are **not** allowed to buy more items than you want, even if that would lower the overall price.



## 题目解读

&emsp;在LeetCode商店中， 有许多在售的物品。然而，也有一些大礼包，每个大礼包以优惠的价格捆绑销售一组物品。现给定每个物品的价格，每个大礼包包含物品的清单，以及待购物品清单。请输出确切完成待购清单的最低花费。每个大礼包的由一个数组中的一组数据描述，最后一个数字代表大礼包的价格，其他数字分别表示内含的其他种类物品的数量。任意大礼包可无限次购买。

```java
class Solution {

    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        
    }
}
```

## 程序设计

* 首先想到的是完全背包问题，但是由于商品数目不定，无法较为简洁的使用动态规划数组计算；改用回溯暴力尝试，分别尝试单独购买和礼包。
* 上述思路会超时，首先可对礼包名单进行处理，删除数目超出所需的礼包；其次可删除礼包价格大于等于单独购买的礼包，这样可利用贪婪法，尝试所有礼包组合，当剩余数目无法用礼包来构成时，才使用单独购买。因为处理后礼包价格必然小于单独购买，故贪婪尝试礼包必然是最优的。

```java
class Solution {
    int minPrice = Integer.MAX_VALUE;
    List<Integer> price;
    List<List<Integer>> special;

    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        // 删除不满足要求的礼包
        List<List<Integer>> remove = new LinkedList<>();
        for (List<Integer> offer : special) {
            int sumPrice = 0;
            for (int i = 0; i < needs.size(); i++) {
                // 礼包数目大于需要数目
                if (offer.get(i) > needs.get(i)) remove.add(offer);
                sumPrice += offer.get(i) * price.get(i);
            }
            // 礼包总价格大于单独购买价格，删除
            if (sumPrice <= offer.get(offer.size() - 1)) remove.add(offer);
        }
        special.removeAll(remove);

        this.price = price;
        this.special = special;

        shoppingOffers(needs, 0);
        return minPrice;
    }

    private void shoppingOffers(List<Integer> needs, int curPrice) {
        if (curPrice >= minPrice) return;

        int size = needs.size();
        // 是否可填充礼包
        boolean flag = false;
        // 试探礼包
        breakpoint: for (List<Integer> offer : special) {
            for (int i = 0; i < size; i++) {
                if (offer.get(i) > needs.get(i)) continue breakpoint;
            }

            flag = true;
            List<Integer> temp = new ArrayList<>(needs);
            for (int i = 0; i < size; i++) {
                temp.set(i, needs.get(i) - offer.get(i));
            }
            shoppingOffers(temp, curPrice + offer.get(size));
        }

        // 无法由礼包构成，则单独买
        if (!flag) {
            for (int i = 0; i < size; i++) {
                curPrice += needs.get(i) * price.get(i);
            }
            minPrice = Math.min(minPrice, curPrice);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM^K)$，空间复杂度为$O(K)$，其中$N$为商品数，$M$为礼包数，$K$为礼包组合深度。

执行用时：8ms，在所有java提交中击败了76.90%的用户。

内存消耗：38.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;参考官方思路，采用记忆化回溯：

```java
class Solution {
    List<Integer> price;
    List<List<Integer>> special;
    Map<List<Integer>, Integer> record;

    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        // 删除不满足要求的礼包
        List<List<Integer>> remove = new LinkedList<>();
        for (List<Integer> offer : special) {
            int sumPrice = 0;
            for (int i = 0; i < needs.size(); i++) {
                // 礼包数目大于需要数目
                if (offer.get(i) > needs.get(i)) remove.add(offer);
                sumPrice += offer.get(i) * price.get(i);
            }
            // 礼包总价格大于单独购买价格，删除
            if (sumPrice <= offer.get(offer.size() - 1)) remove.add(offer);
        }
        special.removeAll(remove);

        this.price = price;
        this.special = special;
        this.record = new HashMap<>();
        return shoppingOffers(needs);
    }

    private int shoppingOffers(List<Integer> needs) {
        if (record.get(needs) != null) return record.get(needs);

        int size = needs.size();
        // 是否可填充礼包
        boolean flag = false;
        int minPrice = Integer.MAX_VALUE;
        // 试探礼包
        breakpoint: for (List<Integer> offer : special) {
            for (int i = 0; i < size; i++) {
                if (offer.get(i) > needs.get(i)) continue breakpoint;
            }

            flag = true;
            List<Integer> temp = new ArrayList<>(needs);
            for (int i = 0; i < size; i++) {
                temp.set(i, needs.get(i) - offer.get(i));
            }
            minPrice = Math.min(minPrice, shoppingOffers(temp) + offer.get(size));
        }

        // 无法由礼包构成，则单独买
        if (!flag) {
            minPrice = 0;
            for (int i = 0; i < size; i++) {
                minPrice += needs.get(i) * price.get(i);
            }
        }
        record.put(needs, minPrice);
        return minPrice;
    }
}
```

执行用时：11ms，在所有java提交中击败了38.58%的用户。

内存消耗：38.9MB，在所有java提交中击败了100.00%的用户。