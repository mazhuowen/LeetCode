[toc]

Given a chemical `formula` (given as a string), return the count of each atom.

An atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

1 or more digits representing the count of that element may follow if the count is greater than 1. If the count is 1, no digits will follow. For example, $H_2O$ and $H_2O_2$ are possible, but $H_1O_2$ is impossible.

Two formulas concatenated together produce another formula. For example, $H_2O_2He_3Mg_4$ is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula. For example, $(H_2O_2)$ and $(H_2O_2)_3$ are formulas.

Given a formula, output the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than 1), followed by the second name (in sorted order), followed by its count (if that count is more than 1), and so on.



Note:

* All atom names consist of lowercase letters, except for the first character which is uppercase.
* The length of formula will be in the range [1, 1000].
* formula will only consist of letters, digits, and round parentheses, and is a valid formula as defined in the problem.



## 题目解读

&emsp;给定化学表达式，计算原子数。最终的结果需要根据原子字母的字典序排序。

```java
class Solution {
    public String countOfAtoms(String formula) {
        
    }
}
```

## 程序设计

* 采用有序字典保存原子及其数目，采用栈匹配计算括号内的原子数，最后将有序字典输出即可。

```java
class Solution {
    // 新对象，保存原子和下标
    class AtomNum {
        public String atom;
        public int num;

        AtomNum(String atom, int num) {
            this.atom = atom;
            this.num = num;
        }
    }

    public String countOfAtoms(String formula) {
        Stack<AtomNum> stack = new Stack<>();
        Stack<AtomNum> tempStack = new Stack<>();
        int index = 0;
        while(index < formula.length()) {
            // 当前字符是原子首字母
            if(Character.isUpperCase(formula.charAt(index))) {
                // 遍历找出原子名
                String atom = formula.charAt(index) + "";
                while(++index < formula.length() && Character.isLowerCase(formula.charAt(index))) {
                    atom += formula.charAt(index);
                }
                // 已遍历完或原子名后没有数字，则是1，入栈
                if(index == formula.length() || Character.isUpperCase(formula.charAt(index)) || formula.charAt(index) == ')' || formula.charAt(index) == '(') {
                    stack.push(new AtomNum(atom, 1));
                } else {
                    int val = 0;
                    while(index < formula.length() && Character.isDigit(formula.charAt(index))) {
                        val = val * 10 + (formula.charAt(index++) - '0');
                    }
                    stack.push(new AtomNum(atom, val));
                }
            }
            // 遇到闭括号，则出栈重新计算
            else if(formula.charAt(index) == ')') {
                 int val = 0;
                while(++index < formula.length() && Character.isDigit(formula.charAt(index))) {
                        val = val * 10 + (formula.charAt(index) - '0');
                }
                while(!stack.isEmpty() && !"(".equals(stack.peek().atom)) {
                    AtomNum temp = stack.pop();
                    temp.num = temp.num * val;
                    tempStack.push(temp);
                }
                // 弹出(
                stack.pop();
                // 重新入栈
                while(!tempStack.isEmpty()) {
                    stack.push(tempStack.pop());
                }
            }
            // 开括号入栈
            else {
                stack.push(new AtomNum(formula.charAt(index++) + "", -1));
            }
        }
        Map<String, Integer> map = new TreeMap<>();
        while(!stack.isEmpty()) {
            AtomNum temp = stack.pop();
            map.put(temp.atom, map.getOrDefault(temp.atom, 0) + temp.num);
        }
        StringBuffer sb = new StringBuffer();
        for(Map.Entry entry : map.entrySet()) {
            sb.append(entry.getKey());
            if((Integer)entry.getValue() > 1) {
                sb.append(entry.getValue());
            }
        }
        return sb.toString();
    }
}
```

## 性能分析

&emsp;由于遍历过程中遇到闭括号还需要从栈顶遍历直到遇到开括号，最坏情况只有一个原子，但嵌套了$N$对括号，时间复杂度$O(N^2)$，空间复杂度$O(N)$。

执行用时：6ms，在所有java提交中击败了80.12%的用户。

内存消耗：36.1MB，在所有java提交中击败了8.41%的用户。

## 官方解题

&emsp;官方解题大体思路相近。

```java
class Solution {
    public String countOfAtoms(String formula) {
        int N = formula.length();
        Stack<Map<String, Integer>> stack = new Stack();
        stack.push(new TreeMap());

        for (int i = 0; i < N;) {
            if (formula.charAt(i) == '(') {
                stack.push(new TreeMap());
                i++;
            } else if (formula.charAt(i) == ')') {
                Map<String, Integer> top = stack.pop();
                int iStart = ++i, multiplicity = 1;
                while (i < N && Character.isDigit(formula.charAt(i))) i++;
                if (i > iStart) multiplicity = Integer.parseInt(formula.substring(iStart, i));
                for (String c: top.keySet()) {
                    int v = top.get(c);
                    stack.peek().put(c, stack.peek().getOrDefault(c, 0) + v * multiplicity);
                }
            } else {
                int iStart = i++;
                while (i < N && Character.isLowerCase(formula.charAt(i))) i++;
                String name = formula.substring(iStart, i);
                iStart = i;
                while (i < N && Character.isDigit(formula.charAt(i))) i++;
                int multiplicity = i > iStart ? Integer.parseInt(formula.substring(iStart, i)) : 1;
                stack.peek().put(name, stack.peek().getOrDefault(name, 0) + multiplicity);
            }
        }

        StringBuilder ans = new StringBuilder();
        for (String name: stack.peek().keySet()) {
            ans.append(name);
            int multiplicity = stack.peek().get(name);
            if (multiplicity > 1) ans.append("" + multiplicity);
        }
        return new String(ans);
    }
}
```

> 社区运行较快的代码大体思想相近，只是实现细节不用，比如不采用红黑树，采用优先级队列等。