---
title: "279. Perfect Squares"
description: "Day II"
---

A challenging problem indeed.

Initially, I thought of using a Dynamic Programming (DP) approach and did not consider the need to take the minimum at any point. My initial thought was that the answer to reach `n` would simply be:

 Dp[n] = 1 + Dp[n - pow(floor(sqrt(n)), 2)]


### Base Cases:
- `Dp[1] = 1`
- `Dp[m] = 1` if `m == pow(floor(sqrt(m)), 2)` (i.e., `m` is a perfect square).

However, this approach is greedy and fails in some cases. For example, when `n = 12`:


#### DP Table:
| i  | Dp[i] |
|----|-------|
| 0  | 0     |
| 1  | 1     |
| 2  | 2     |
| 3  | 3     |
| 4  | 1     |
| 5  | 2     |
| 6  | 3     |
| 7  | 4     |
| 8  | 2     |
| 9  | 1     |
| 10 | 2     |
| 11 | 3     |
| 12 | **4** |



The above approach fails because `Dp[12]` should be `4 + 4 + 4`, which is `3` elements, not `4`.

### The Correct Approach

Here comes the minimum part. To reach 12, we need to consider all possible squares less than or equal to 12. It can be reached by `1^2`, `2^2`, or `3^2`, but not by `4^2`. We should calculate the minimum number of steps required to reach `12` from each of these squares and use the minimum value.

Thus, the range for `n` would be from `1` to `floor(sqrt(n))`.

### Algorithm:
1. For each `i` from 1 to `n`:
   - For each `j` from 1 to the range of `i`:
     - `Dp[i] = min(Dp[i], numbersRequired(i, j))`

2. The `numbersRequired` function calculates the steps required to reach `i` from `j`.

### Implementation in C++:

```cpp
class Solution {
public:
    int perfectSquare(int n, vector<int> &vec, int &j) {
        if(n == 1 || n == pow(floor(sqrt(n)), 2)) return 1;
        int diff = n - pow(j, 2);
        if(vec[diff] != INT_MAX) return 1 + vec[diff];
        vec[n] = 1 + perfectSquare(diff, vec, j);
        return vec[n];
    }

    int numSquares(int n) {
        if(n == pow(floor(sqrt(n)), 2)) return 1;
        vector<int> vec(n + 1, INT_MAX);
        vec[0] = 0;
        vec[1] = 1;
        int range;
        int j;
        for(int i = 2; i < n + 1; i++) {
            range = floor(sqrt(i));
            for(j = 1; j <= range; j++) {
                vec[i] = min(vec[i], perfectSquare(i, vec, j));
            }
        }
        return vec[vec.size() - 1];
    }
};