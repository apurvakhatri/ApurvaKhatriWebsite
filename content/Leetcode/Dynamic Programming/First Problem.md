---
title: "746. Min Cost Climbing Stairs"
description: "Day I"
---


### Dynamic Programming Approach

In Dynamic Programming (DP), we first identify the target variable, which we'll denote as `F(n)`. The next step is to determine what `F(n)` depends on. For instance, `F(n)` might depend on `F(n-1)` or `F(n-3)`.

In this specific problem, `F(n)` depends on `F(n-1)` and `F(n-2)`. This means that to reach the `n`th point, an individual can arrive either from one step before (`F(n-1)`) or from two steps before (`F(n-2)`).

#### Base Cases:
- `F(0) = cost[0]`
- `F(1) = min(cost[0] + cost[1], cost[1])`

For every element starting from the 2nd position, we can compute the results in a similar fashion:

```cpp
cost[i] = cost[i] + min(cost[i-1], cost[i-2])
```

At the end, we return:

```cpp
min(cost[cost.size()-1], cost[cost.size()-2])
```










