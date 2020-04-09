[toc]

Given a string that contains only digits `0-9` and a target value, return all possibilities to add **binary** operators (not unary) `+`, `-`, or `*` between the digits so they evaluate to the target value.



## 题目解读

&emsp;给定字符串，在其中添加操作符，最后输出所有等于目标值的表达式。

```java
class Solution {
    public List<String> addOperators(String num, int target) {

    }
}
```

## 程序设计

* 分析题目示例，字符串`105`，目标值`5`，可以是`10-5`，也可以是`1*0-5`，可见在每个数字的位置可以尝试三个操作符，也可以没有操作符。对于一个当前的操作数，其是否可计算由后一操作符决定，如果后一操作符是乘法，则不能与之前的结果相加，如果是加减，则可从目标值中减去。
* 鉴于上述分析，回溯方法首先需要传入当前操作数的起始位置，从该位置遍历生成操作数；其次需要知道前一个操作符是什么，如果是加减，且当前操作数尝试的下一个操作符不是乘法，则可以直接从`target`里减去（或加上）当前操作数；如果前一操作符是乘法，则前一个值乘上当前操作数，同理下一个操作数不是乘法则从`target`里减去。如果下一个是尝试操作符是乘法，则`target`不变，将当前操作数作为前一操作数传入。
* 细节需要注意数字0的处理，因为不存在01这样的数，0不能作为数字的开头；其次需要注意数字溢出的情况。由于字符串中的数字长度可能大于10，这样遍历得到的数字会发生溢出，应使用长整型；最后需要注意结果的重复情况，假设满足条件的表达式为`express`，则最后仍然会试探三次，即`express+`、`express-`、`express*`，导致截取最后一位并入队，造成三个重复，可以使方法返回标识，如果是`true`则不必试探`-`和`*`。

```java
class Solution {
    char[] chars;
    List<String> res;

    public List<String> addOperators(String num, int target) {
        res = new LinkedList<>();
        if (num == null || num.length() == 0) return res;
        chars = num.toCharArray();

        addOperators(0, 0, 0, target, new ArrayList<>());
        return res;
    }

    /**
     * @param start 当前操作数起始位置
     * @param preOp 当前操作数前一个操作符 0,1,2表示+,-,*
     * @param preNum 前一个操作数（对于乘法，其他为0）
     * @param target 目标值
     * @param temp 路径链表
     */
    private boolean addOperators(int start, int preOp, long preNum, long target, List<String> temp) {
        // 找到一组可行解，对于加减法，只要target为0即可，对于乘法由于preNum还未计算进去，需要判断preNum为0
        if (start >= chars.length && target == 0 && (preOp != 2 || (preOp == 2 && preNum == 0))) {
            StringBuffer sb = new StringBuffer();
            for (String s : temp) sb.append(s);
            sb.deleteCharAt(sb.length() - 1);

            res.add(sb.toString());
            return true;
        }
        // 不可行
        if (start >= chars.length) return false;

        // 当前操作数（注意数字溢出，需要用长整型）
        long num = 0;
        for (int i = start; i < chars.length; i++) {
            num = num * 10 + (chars[i] - '0');
            // 不能出现01的情况
            if (i > start && num == (chars[i] - '0')) break;

            // 结合前一符号计算当前数：对于加减，前一数为0，只用于判断正负符号
            // 对于乘法，前一操作数是带符号的数，相乘得到新的操作数
            long curNum = calcu(preNum, num, preOp);

            // 试探加法
            temp.add(num + "+");
            boolean flag = addOperators(i + 1, 0, 0, target - curNum, temp);
            temp.remove(temp.size() - 1);
			
            // flag防止重复
            if (!flag) {
                // 试探减法
                temp.add(num + "-");
                addOperators(i + 1, 1, 0, target - curNum, temp);
                temp.remove(temp.size() - 1);

                // 试探乘法
                temp.add(num + "*");
                addOperators(i + 1, 2, curNum, target, temp);
                temp.remove(temp.size() - 1);
            }
        }
        return false;
    }

    private long calcu(long num1, long num2, int op) {
        if (op == 0) return num1 + num2;
        else if (op == 1) return num1 - num2;
        else return num1 * num2;
    }
}
```



## 性能分析

&emsp;由于$T(N) = 3*T(N - 1) + 3*T(N - 2) + \dots + 3 * T(1) \implies T(N) = 4*T(N - 1)$，即时间复杂度为$O(4^N)$，空间复杂度为$O(4^N)$。

执行用时：233ms，在所有java提交中击败了19.50%的用户。

内存消耗：40.3MB，在所有java提交中击败了55.56%的用户。

## 官方解题

&emsp;思路类似，但官方时间性能更好。