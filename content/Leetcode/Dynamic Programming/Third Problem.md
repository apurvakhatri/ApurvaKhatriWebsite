---
title: "338. Counting Bits"
description: "Day III"

---

# 338. Counting Bits
**Description:** Day III of coding challenges.

## Observations and Approach

While working through examples, I noticed a pattern in how binary numbers are constructed. Specifically, to form any number, you first take the highest power of 2 that is less than or equal to the number, and then form the remaining part with the difference.

For example, to form the number `9`, we take `8` (which is `2^3`, the highest power of 2 less than 9) and subtract it from `9`. The difference is `1`, which in binary is `1`. Therefore, the binary representation of `9` is `1001`, and it contains two `1`s.

### Examples:

- `0`: `0` (0 ones)
- `1`: `0001` (1 one)
- `2`: `0010` (1 one)
- `3`: `0011` (2 ones)
- `4`: `0100` (1 one)
- `5`: `0101` (2 ones)
- `6`: `0110` (2 ones)
- `7`: `0111` (3 ones)
- `8`: `1000` (1 one)
- `9`: `1001` (2 ones)

### Mathematical Insight

We can generalize the number of `1`s in the binary representation as follows:

- **Base Cases:**
  - `f(n) = 1` if `n` is a power of 2 (i.e., `n = 2^x`).

- **Recursive Case:**
  - `f(n) = 1 + f(n - 2^floor(log2(n)))`.

This recursive approach can be implemented efficiently using dynamic programming.

### Implementation

Here is the implementation of the above approach in C++:

```cpp
#include <vector>
#include <cmath>

bool checkSquare(int &n) {
    int no = floor(log2(n));
    return n == pow(2, no);
}

std::vector<int> countBits(int n) {
    std::vector<int> dp(n + 1, 0);
    if (n == 0) return dp;

    dp[1] = 1;
    if (n == 1) return dp;

    dp[2] = 1;
    for (int i = 3; i <= n; i++) {
        if (checkSquare(i)) {
            dp[i] = 1;
        } else {
            dp[i] = 1 + dp[i - pow(2, floor(log2(i)))];
        }
    }
    return dp;
}
