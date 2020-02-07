[toc]

Design a `Snake game` that is played on a device with screen `size = width x height`. `Play the game online` if you are not familiar with the game.

The snake is initially positioned at the top left corner $(0,0)$ with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.



## 题目解读

&emsp;设计贪吃蛇游戏，初始时蛇在左上角，也就是$(0,0)$的位置，长度为1；每当蛇吃到食物，身子长度增加1，得分也会增加1；如果蛇与自己身体或墙相撞，得分为-1，意味着游戏结束。

```java
class SnakeGame {

    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    public SnakeGame(int width, int height, int[][] food) {
        
    }
    
    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. */
    public int move(String direction) {
        
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
```

## 程序设计

* 蛇可看做是一个队列，队列长度不超过屏幕尺寸，即面积。每次蛇的移动相当于入队新坐标，出队旧坐标；每次吃到食物相当于只入队，不出队。
* 判断撞到墙，相当于队尾已经在边界，而向墙外移动；对于撞到自身，需要维护一个数组，表示哪些点是蛇的身体。

```java
class SnakeGame {
    private int width;
    private int height;
    private int score = 0;
    // 记录蛇头，也就是上次入队的坐标
    private int[] head;
    // 记录蛇身子所占坐标
    private boolean[][] snake;
    // 队列，模拟记录蛇的移动、进食
    private Queue<int[]> queue;
    // 食物
    int[][] food;
    int num;

    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    public SnakeGame(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        // 初始化蛇头
        this.head = new int[]{0,0};
        // 初始化蛇的身子
        this.snake = new boolean[height][width];
        this.snake[0][0] = true;
        // 维护队列
        this.queue = new LinkedList<>();
        this.queue.add(head);
        // 食物
        this.food = food;
        this.num = 0;
    }
    
    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. */
    public int move(String direction) {
        // 根据指令计算新的头结点
        int[] newHead = new int[2];
        switch(direction) {
            case "U":
                newHead[0] = head[0] - 1;
                newHead[1] = head[1];
                // 撞上面的墙，游戏结束
                if(newHead[0] < 0) {
                    return -1;
                }
                break;
            case "L":
                newHead[0] = head[0];
                newHead[1] = head[1] - 1;
                // 撞左边的墙，游戏结束
                if(newHead[1] < 0) {
                    return -1;
                }
                break;
            case "R":
                newHead[0] = head[0];
                newHead[1] = head[1] + 1;
                // 撞右边的墙，游戏结束
                if(newHead[1] >= width) {
                    return -1;
                }
                break;
            case "D":
                newHead[0] = head[0] + 1;
                newHead[1] = head[1];
                // 撞下面的墙，游戏结束
                if(newHead[0] >= height) {
                    return -1;
                }
                break;
            default:
                break;
        }
        // 当前移动的结点是食物
        if(num <  food.length && newHead[0] == food[num][0] && newHead[1] == food[num][1]) {
            // 判断移动是否碰撞身子（即新的头结点与身子冲突）
            if(snake[newHead[0]][newHead[1]]) {
                return -1;
            }
            // 执行入队、加分、维护蛇身、维护蛇头
            score += 1;
            queue.add(newHead);
            snake[newHead[0]][newHead[1]] = true;
            head = newHead;
            // 食物已吃完，计数下一个
            num++;
        } 
        // 不是食物即移动，入队，出队
        else {
            // 先出队，再判断是否冲突
            int[] temp = queue.poll();
            snake[temp[0]][temp[1]] = false;
            // 判断移动是否碰撞身子
            if(snake[newHead[0]][newHead[1]]) {
                return -1;
            }
            // 入队，维护蛇身、维护蛇头
            queue.add(newHead);
            snake[newHead[0]][newHead[1]] = true;
            head = newHead;
        }
        // 返回分数
        return score;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(1)$，空间复杂度$O(NM)$。

执行用时：90ms，在所有java提交中击败了58.73%的用户。

内存消耗：207.1MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;暂无，密切关注。

> 社区思路大致都一样，利用队列来模拟蛇的移动、进食，实现细节不一样，如将记录蛇身的二维数组snake变为链表等，运行时间会更少些。