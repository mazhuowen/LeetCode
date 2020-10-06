[toc]

Design a parking system for a parking lot. The parking lot has three kinds of parking spaces: big, medium, and small, with a fixed number of slots for each size.

Implement the `ParkingSystem` class:

* `ParkingSystem(int big, int medium, int small)` Initializes object of the `ParkingSystem` class. The number of slots for each parking space are given as part of the constructor.
* `bool addCar(int carType)` Checks whether there is a parking space of `carType` for the car that wants to get into the parking lot. `carType` can be of three kinds: big, medium, or small, which are represented by $1$, $2$, and $3$ respectively. **A car can only park in a parking space of its** `carType`. If there is no space available, return `false`, else park the car in that size space and return `true`.



**Constraints**:

* $0 \le \text{big, medium, small} \le 1000$
* `carType` is $1$, $2$, or $3$
* At most $1000$ calls will be made to addCar



## 题目解读

&emsp;设计停车系统。

```java
class ParkingSystem {

    public ParkingSystem(int big, int medium, int small) {

    }
    
    public boolean addCar(int carType) {

    }
}

/**
 * Your ParkingSystem object will be instantiated and called as such:
 * ParkingSystem obj = new ParkingSystem(big, medium, small);
 * boolean param_1 = obj.addCar(carType);
 */
```

## 程序设计

* 计数即可。

```java
class ParkingSystem {
    int big, medium, small;

    public ParkingSystem(int big, int medium, int small) {
        this.big = big;
        this.medium = medium;
        this.small = small;
    }
    
    public boolean addCar(int carType) {
        switch (carType) {
            case 1:
                return --big >= 0;
            case 2:
                return --medium >= 0;
            case 3:
                return --small >= 0;
            default:
                throw new IllegalArgumentException("invalid type");
        }
    }
}
```

* 优化代码：

```java
class ParkingSystem {
    int[] counter = new int[3];

    public ParkingSystem(int big, int medium, int small) {
        counter[0] = big;
        counter[1] = medium;
        counter[2] = small;
    }
    
    public boolean addCar(int carType) {
        if (carType < 1 || carType > 3) throw new IllegalArgumentException("invalid type");
        return --counter[carType - 1] >= 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：11ms，在所有java提交中击败了66.04%的用户。

内存消耗：39.3MB，在所有java提交中击败了98.39%的用户。

## 官方解题

&emsp;暂无，密切关注。