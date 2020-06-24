[toc]

Table: `Teams`

```
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| team_id       | int      |
| team_name     | varchar  |
+---------------+----------+
team_id is the primary key of this table.
Each row of this table represents a single football team.
```

Table: `Matches`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| match_id      | int     |
| host_team     | int     |
| guest_team    | int     | 
| host_goals    | int     |
| guest_goals   | int     |
+---------------+---------+
match_id is the primary key of this table.
Each row is a record of a finished match between two different teams. 
Teams host_team and guest_team are represented by their IDs in the teams table (team_id) and they scored host_goals and guest_goals goals respectively.
```

You would like to compute the scores of all teams after all matches. Points are awarded as follows:

* A team receives three points if they win a match (Score strictly more goals than the opponent team).
* A team receives one point if they draw a match (Same number of goals as the opponent team).
* A team receives no points if they lose a match (Score less goals than the opponent team).

Write an SQL query that selects the **team_id**, **team_name** and **num_points** of each team in the tournament after all described matches. Result table should be ordered by **num_points** (decreasing order). In case of a tie, order the records by **team_id** (increasing order).

The query result format is in the following example:

```
Teams table:
+-----------+--------------+
| team_id   | team_name    |
+-----------+--------------+
| 10        | Leetcode FC  |
| 20        | NewYork FC   |
| 30        | Atlanta FC   |
| 40        | Chicago FC   |
| 50        | Toronto FC   |
+-----------+--------------+

Matches table:
+------------+--------------+---------------+-------------+--------------+
| match_id   | host_team    | guest_team    | host_goals  | guest_goals  |
+------------+--------------+---------------+-------------+--------------+
| 1          | 10           | 20            | 3           | 0            |
| 2          | 30           | 10            | 2           | 2            |
| 3          | 10           | 50            | 5           | 1            |
| 4          | 20           | 30            | 1           | 0            |
| 5          | 50           | 30            | 1           | 0            |
+------------+--------------+---------------+-------------+--------------+

Result table:
+------------+--------------+---------------+
| team_id    | team_name    | num_points    |
+------------+--------------+---------------+
| 10         | Leetcode FC  | 7             |
| 20         | NewYork FC   | 3             |
| 50         | Toronto FC   | 3             |
| 30         | Atlanta FC   | 1             |
| 40         | Chicago FC   | 0             |
+------------+--------------+---------------+
```



## 题目解读

&emsp;根据足球比赛的输赢计算得分。

## 程序设计

* 对主队和客队分别统计分数，然后使用`UNION ALL`结合球队表得到结果。需注意有些球队没有比赛，分数为$0$。

```mysql
SELECt s.team_id, s.team_name, sum(ifnull(t.num_points, 0)) AS num_points
FROM 
    Teams s 
LEFT JOIN 
    (
        (SELECT 
            host_team AS team_id, 
            (CASE 
                WHEN host_goals > guest_goals THEN 3 
                WHEN host_goals = guest_goals THEN 1 
                ELSE 0 
            END) AS num_points
        FROM Matches) 
        UNION ALL
        (SELECT 
            guest_team AS team_id, 
            (CASE 
                WHEN guest_goals > host_goals THEN 3 
                WHEN host_goals = guest_goals THEN 1 
                ELSE 0 END) AS num_points
        FROM Matches
        )
    ) t 
ON s.team_id = t.team_id GROUP BY s.team_id
ORDER BY num_points DESC, s.team_id ASC
;
```

## 性能分析

执行用时：526ms，在所有MySQL提交中击败了42.13%的用户。

内存消耗：0B，在所有MySQL提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用一次判断方法。

```mysql
SELECT
    team_id,
    team_name,
    IFNULL(SUM(CASE
        WHEN host_goals = guest_goals THEN 1
        WHEN host_team = team_id AND host_goals > guest_goals THEN 3
        WHEN guest_team = team_id AND host_goals < guest_goals THEN 3
        ELSE 0
    END), 0) num_points
FROM 
    Teams LEFT JOIN Matches
    ON Matches.host_team = team_id OR Matches.guest_team = team_id
GROUP BY team_id
ORDER BY num_points DESC, team_id ASC
;
```