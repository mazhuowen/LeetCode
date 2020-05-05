[toc]



```java
class Solution {

    public List<String> removeInvalidParentheses(String s) {
        List<String> res = new LinkedList<>();
        if (s == null) return res;

        // 查找分隔点
        char[] str = s.toCharArray();
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < str.length; i++) {
            char c = str[i];
            if (c == '(') {
                stack.push(i);
            } else if (c == ')') {
                if (!stack.isEmpty() && str[stack.peek()] == '(') stack.pop();
                else stack.push(i);
            }
        }
        // 本身为合法括号对
        if (stack.isEmpty()) {
            res.add(s);
            return res;
        }
        // 统计分割区块和各区块需要删除括号数目
        int idx = -1;
        int num = stack.size();
        int openNum = num, closeNum = 0;
        while (!stack.isEmpty()) {
            if (str[stack.peek()] == ')') {
                openNum = num - stack.size();
                closeNum = stack.size();
                idx = stack.peek();
                break;
            }
            stack.pop();
        }

        // 分别删除括号
        Set<String> preSet = new HashSet<>();
        Set<String> nextSet = new HashSet<>();
        remove(s.substring(0, idx + 1).toCharArray(), 0, closeNum, ')', preSet);
        remove(s.substring(idx + 1).toCharArray(), 0, openNum, '(', nextSet);

        // 前后拼接
        if (preSet.isEmpty()) preSet.add("");
        if (nextSet.isEmpty()) nextSet.add("");
        Set<String> temp = new HashSet<>();
        for (String p : preSet) {
            for (String q : nextSet) {
                temp.add(p + q);
            }
        }
        return new LinkedList<>(temp);
    }

    private void remove(char[] source, int start, int num, char target, Set<String> set) {
        // 终止条件
        if (num == 0) {
            // 需要检验删除后时候符合要求
            if (isValid(source)) set.add(new String(source).replace(" ", ""));
            return;
        }
        if (start >= source.length) return;

        for (int i = start; i <= source.length - num; i++) {
            if (source[i] != target) continue;
            // 试探
            source[i] = ' ';
            remove(source, i + 1, num - 1, target, set);
            // 回溯
            source[i] = target;
        }
    }

    private boolean isValid(char[] str) {
        int count = 0;
        for (char c : str) {
            if (c == ')') count--;
            else if (c == '(') count++;
            // 序列中闭括号不能多于开括号
            if (count < 0) return false;
        }
        return count == 0;
    }
}
```



```java
class Solution {

    public List<String> removeInvalidParentheses(String s) {
        List<String> res = new LinkedList<>();

        // 查找分隔点
        char[] str = s.toCharArray();

        // 统计分割区块和各区块需要删除括号数目
        int idx = -1;
        int count = 0, openNum = 0, closeNum = 0;
        for (int i = 0; i < str.length; i++) {
            char c = str[i];
            if (c == '(') count++;
            else if (c == ')') count--;

            // 说明当前闭括号多于开括号，需要删除
            if (count < 0) {
                idx = i;
                closeNum -= count;
                // 重置（关键）
                count = 0;
            }
        }
        // 最后多余的开括号数
        if (count > 0) openNum = count;

        // 本身为合法括号对
        if (closeNum == 0 && openNum == 0) {
            res.add(s);
            return res;
        }

        // 分别删除前面的闭括号和后面的开括号
        Set<String> preSet = new HashSet<>(), nextSet = new HashSet<>();
        remove(s.substring(0, idx + 1).toCharArray(), 0, closeNum, ')', preSet);
        remove(s.substring(idx + 1).toCharArray(), 0, openNum, '(', nextSet);

        // 前后拼接，为空则添加空，避免不遍历的情况
        if (preSet.isEmpty()) preSet.add("");
        if (nextSet.isEmpty()) nextSet.add("");
        // 排除重复
        Set<String> temp = new HashSet<>();
        for (String p : preSet) {
            for (String q : nextSet) {
                temp.add(p + q);
            }
        }
        return new LinkedList<>(temp);
    }

    private void remove(char[] source, int start, int num, char target, Set<String> set) {
        // 终止条件
        if (num == 0) {
            if (isValid(source, 0, source.length - 1)) set.add(new String(source).replace(" ", ""));
            return;
        }
        if (start >= source.length) return;

        for (int i = start; i <= source.length - num; i++) {
            if (source[i] != target) continue;
            // 试探
            source[i] = ' ';
            if (isValid(source, 0, i)) remove(source, i + 1, num - 1, target, set);
            // 回溯
            source[i] = target;
        }
    }

    private boolean isValid(char[] str, int i, int j) {
        int count = 0;
        for (; i <= j; i++) {
            char c = str[i];
            if (c == ')') count--;
            else if (c == '(') count++;
            // 序列中闭括号不能多于开括号
            if (count < 0) return false;
        }

        return true;
    }
}
```

