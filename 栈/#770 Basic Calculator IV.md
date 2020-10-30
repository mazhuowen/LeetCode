[toc]

Given an `expression` such as `expression = "e + 8 - a + 5"` and an evaluation map such as `{"e": 1}` (given in terms of `evalvars = ["e"]` and `evalints = [1]`), return a list of tokens representing the simplified expression, such as `["-1*a","14"]`

* An expression alternates chunks and symbols, with a space separating each chunk and symbol.
* A chunk is either an expression in parentheses, a variable, or a non-negative integer.
* A variable is a string of lowercase letters (not including digits.) Note that variables can be multiple letters, and note that variables never have a leading coefficient or unary operator like `"2x"` or `"-x"`.

Expressions are evaluated in the usual order: brackets first, then multiplication, then addition and subtraction. For example, `expression = "1 + 2 * 3"` has an answer of `["7"]`.

The format of the output is as follows:

* For each term of free variables with non-zero coefficient, we write the free variables within a term in sorted order lexicographically. For example, we would never write a term like `"b*a*c"`, only `"a*b*c"`.
* Terms have degree equal to the number of free variables being multiplied, counting multiplicity. (For example, `"a*a*b*c"` has degree 4.) We write the largest degree terms of our answer first, breaking ties by lexicographic order ignoring the leading coefficient of the term.
* The leading coefficient of the term is placed directly to the left with an asterisk separating it from the variables (if they exist.)  A leading coefficient of 1 is still printed.
* An example of a well formatted answer is `["-2*a*a*a", "3*a*a*b", "3*b*b", "4*a", "5*c", "-6"]` 
* Terms (including constant terms) with coefficient 0 are not included.  For example, an expression of "0" has an output of [].



**Note**:

* expression will have length in range `[1, 250]`.
* evalvars, evalints will have equal lengths in range `[0, 100]`.



## 题目解读

&emsp;表达式每个元素用空格隔开，题目限定没有一元运算符如负号，变量前面没有类似`2x`这样的系数。计算得到的结果需要排序输出，具体的，多项式项数多的排在前面，项数一样则根据字典序排列，排序不考虑前面的整数系数。

```java
class Solution {
    public List<String> basicCalculatorIV(String expression, String[] evalvars, int[] evalints) {
        
    }
}
```

## 程序设计

* 首先考虑一般的算数表达式，我们一般使用中缀转后缀，计算后缀表达式得到结果。但存在变量时有个问题，比如`a+2`，如果`a`是数字则从栈中出栈计算，最后将结果压栈，下次计算则只需要弹出一个操作数；存在变量的问题是计算没法将`a+2`合并为一个数，这样就破坏了后缀表达式计算的逻辑。
* 为了使得有变量的计算式两两计算后为一个对象，使得后缀计算可行，我们需要创建一个对象表示多项式，这样不管是变量还是常数，两个多项式计算完还是一个多项式，入栈继续后续操作。
* 考虑到一个多项式存在多个变量，每个变量可以是单独的变量如`a`，或多个变量的乘积组合`a*b*c`，每个变量可以用链表来存储，则单独变量就是只有一个元素的链表，多个变量组合成的变量就是有多个元素的链表；而对于变量的系数，可以用字典表示，这样键是变量，值是系数。
* 那么问题来了，整数要怎么表示，整数链表部分可以表示为空字符串，值就是整数本身，在操作时需要注意判断链表里的空串。
* 在后缀表达式中除了操作数，还有操作符，可以在对象属性中新增一个字符表示操作符。这样我们这个对象就既可以表示变量、整数、多项式，又可以表示操作符。最后对象如下：

```java
// 定义多项式或操作符
class Poly {
    // 操作符
    char opera;
    // 多项式，每个变量以List的形式保存，键值为变量系数
    Map<List<String>, Integer> count;
    /*
    * 初始创建时，无非是操作符、单变量、整数
    * 在后续计算中逐渐形成多项式，故构造函数只有这三类
    */
    // 操作符的构建方法
    Poly(char opera) {
        // 操作符的表示：操作符属性
        this.opera = opera;
    }
    // 整数的构建方法
    Poly(Integer val) {
        this.count = new HashMap<>();
        List<String> var = new LinkedList<>();
        var.add("");
        // 整数的表示：空串:整数值
        this.count.put(var, val);
    }
    // 变量的构造方法
    Poly(String varName, Integer val) {
        this.count = new HashMap<>();
        List<String> var = new LinkedList<>();
        var.add(varName);
        // 变量的表示：变量名:系数（初始为1）
        this.count.put(var, val);
    }

    // 不存在则设置，存在则更新
    private void update(List<String> key, int val) {
        this.count.put(key, this.count.getOrDefault(key, 0) + val);
    }

    // 多项式加法（将num2加入当前对象num1）
    private void add(Poly num2) {
        Map<List<String>, Integer> temp = num2.count;
        for(List<String> key : temp.keySet()) {
            // 存在则系数相加，不存在则加入
            update(key, temp.get(key));
        }
    }

    // 多项式减法（与加法同理，只是加入的系数要取相反数）
    private void sub(Poly num2) {
        Map<List<String>, Integer> temp = num2.count;
        for(List<String> key : temp.keySet()) {
            // 取相反数
            update(key, -temp.get(key));
        }
    }

    // 多项式乘法（将num2乘入num1）
    private void mul(Poly num2) {
        // 创建新的字典
        Map<List<String>, Integer> newCount = new HashMap<>();
        for(List<String> key1 : this.count.keySet()) {
            for(List<String> key2 : num2.count.keySet()) {
                // 乘法相当于将两个多变量中的单个变量再次组合
                List<String> newKey = new LinkedList<>();
                for(String s : key1) {
                    // 过滤常量即空串
                    if(!s.isEmpty()) {
                        // 加入
                        newKey.add(s);
                    }
                }
                for(String s : key2) {
                     // 过滤常量即空串
                    if(!s.isEmpty()) {
                        // 加入
                        newKey.add(s);
                    }
                }
                // 最后新的串是空，表示是两个常量相乘，加入空串
                if(newKey.isEmpty()) {
                    newKey.add("");
                } 
                // 存在变量的乘法，需要排序
                else {
                     // 字典序排序（非常重要）
                    Collections.sort(newKey);
                }
                // 将相乘后的变量加入新的字典（系数相乘）
                newCount.put(newKey, newCount.getOrDefault(newKey, 0) + this.count.get(key1) * num2.count.get(key2));
            }
        }
        // 新的字典代替旧的字典
        this.count = newCount;
    }

    // 对外计算方法，传入操作符和操作数
    public void calculate(char opera, Poly num2) {
        switch(opera) {
            case '+':
                add(num2);
                break;
            case '-':
                sub(num2);
                break;
            case '*':
                mul(num2);
                break;
            default:
                break;
        }
    }

    // 比较两个变量（非常重要）
    // 变量优先级比常量高，即变量小于常量；变量长度越长，优先级越高，即越小；
    // 变量长度一致，根据字典序，越小优先级越高
    private int compareList(List<String> num1, List<String> num2) {
        // 其中一个是常量
        if(num1.get(0).isEmpty()) {
            // 都是常量则优先级相等，否则num2是变量优先级高
            return num2.get(0).isEmpty() ? 0 : 1;
        }
        // num1为变量，num2为常量，num1小
        if(num2.get(0).isEmpty()) {
            return -1;
        }
        // 都为变量，长度不一致，比较长度
        if(num1.size() != num2.size()) {
            return num2.size() - num1.size();
        }
        // 长度一致，比较字典序
        int i = 0;
        for (String x: num1) {
            String y = num2.get(i++);
            // 字典序
            if (x.compareTo(y) != 0) return x.compareTo(y);
        }
        // 两个变量完全相等
        return 0;
    }

    // 将多项式转化为有序数组
    public List<String> toList() {
        List<String> res = new LinkedList<>();
        // 变量数组
        List<List<String>> keys = new ArrayList<>(this.count.keySet());
        // 比较两个链表，项数越大变量越小，排列在最前面；项数一致则比较字典序
        Collections.sort(keys, (a, b) -> compareList(a, b));
        // 拼接乘法符号
        for (List<String> key: keys) {
            int v = this.count.get(key);
            if (v == 0) continue;
            StringBuilder word = new StringBuilder();
            // 系数在前面，包括1也要显示
            word.append(v);
            // 拼接
            for (String token: key) {
                if(!token.isEmpty()) {
                    word.append('*');
                    word.append(token);
                }
            }
            // 将拼接好的变量字符串加入数组
            res.add(word.toString());
        }
        return res;
    }
}
```

* 在得到上述对象后，就可以使用中缀转后缀，后缀计算得到最后结果了。

```java
class Solution {
    public List<String> basicCalculatorIV(String expression, String[] evalvars, int[] evalints) {
        // 维护变量关系
        Map<String, Integer> eval = new HashMap<>();
        for(int i = 0; i < evalvars.length; i++) {
            eval.put(evalvars[i], evalints[i]);
        }
        // 转后缀
        List<Poly> post = transToPost(expression, eval);
        // 计算后缀
        Poly res = calculatePost(post);
        // 输出链表
        return res.toList();
    }

    private List<Poly> transToPost(String expression, Map<String, Integer> eval) {
        List<Poly> res = new LinkedList<>();
        Stack<Poly> stack = new Stack<>();
        for(int i = 0; i < expression.length(); i++) {
            char symbol = expression.charAt(i);
            switch(symbol) {
                // 开括号入栈
                case '(':
                    stack.push(new Poly(symbol));
                    break;
                // 闭括号则出栈操作符
                case ')':
                    while(!stack.isEmpty() && stack.peek().opera != '(') {
                        res.add(stack.pop());
                    }
                    // 弹出开括号
                    stack.pop();
                    break;
                case '+':
                case '-':
                    // 弹出前面的操作符
                    while(!stack.isEmpty() && stack.peek().opera != '(') {
                        res.add(stack.pop());
                    }
                    // 入栈当前操作符
                    stack.push(new Poly(symbol));
                    break;
                case '*':
                    // 弹出前面的乘号操作符
                    while(!stack.isEmpty() && stack.peek().opera == '*') {
                        res.add(stack.pop());
                    }
                    // 入栈当前操作符
                    stack.push(new Poly(symbol));
                    break;
                 // 空格过滤
                case ' ':
                    break;
                // 数字或变量
                default:
                    // 数字
                    if(Character.isDigit(symbol)) {
                        int val = 0;
                        while(i < expression.length() && Character.isDigit(expression.charAt(i))) {
                            val = val * 10 + (expression.charAt(i) - '0');
                            i++;
                        }
                        // 创建整数对象，入队
                        res.add(new Poly(val));
                    } 
                    // 字母
                    else {
                        String varName = "";
                        while(i < expression.length() && Character.isLetter(expression.charAt(i))) {
                            varName += expression.charAt(i);
                            i++;
                        }
                        // 变量有参数，则创建整数对象
                        if(eval.get(varName) != null) {
                            res.add(new Poly(eval.get(varName)));
                        } 
                        // 无参数创建单变量对象
                        else {
                            res.add(new Poly(varName, 1));
                        }
                    }
                    // 由于for循环i++，这儿i--抵消影响
                    i--;
                    break;
            }
        }
        // 操作符出栈入队
        while(!stack.isEmpty()) {
            res.add(stack.pop());
        }
        // 返回后缀表达式数组
        return res;
    }

    private Poly calculatePost(List<Poly> post) {
        Stack<Poly> stack = new Stack<>();
        for(Poly symbol : post) {
            switch(symbol.opera) {
                // 操作符则弹出栈顶num2，计算更新num1 
                case '+':
                case '-':
                case '*':
                    Poly num2 = stack.pop();
                    stack.peek().calculate(symbol.opera, num2);
                    break;
                // 数字或变量，入栈等待计算
                default:
                    stack.push(symbol);
                    break;
            }
        }
        // 返回最后结果
        return stack.pop();
    }
}
```

## 性能分析

&emsp;$N$为表达式长度，$M$为参数长度。由于需要遍历存储参数时间复杂度、空间复杂度都是$O(M)$；而在表达式计算时，最坏的情况是乘法操作，两个多项式相乘有$2^N$种组合，时间复杂度$O(2^N)$，空间复杂度$O(N)$。总的时间复杂度为$O(2^N + M)$，空间复杂度$O(N + M)$。

执行用时：15ms，在所有java提交中击败了73.91%的用户。

内存消耗：43.4MB，在所有java提交中击败了29.41%的用户。

## 官方解题

&emsp;时间复杂度、空间复杂度相同。官方对多项式的处理思路相近，计算未使用后缀表达式。