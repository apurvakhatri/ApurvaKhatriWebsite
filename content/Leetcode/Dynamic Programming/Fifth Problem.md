
---
title: "45. Jump Game II"
description: "Day V"
---

This is an interesting problem because it introduced me to a different approach to solving it. Let’s first discuss the initial approach I implemented.

### Approach I:

The first approach is a simple Dynamic Programming solution. The idea is to store the minimum number of jumps required to reach each position. We use the precomputed results for higher stairs, and finally, the value at `n-1` will store the minimum jumps required to reach that point. This value is then returned.

We need a `computeMinimum` function that will take the `minJumps` vector, the jump length at the current position, the current position index `i`, and the size of the original problem `n`.

```cpp
void computeMinimum(vector<int> &minJumps, int &jumpLength, int &n, int &i) {
    int k = 1;
    while(i + k < n && k <= jumpLength) {
        minJumps[i + k] = min(minJumps[i + k], minJumps[i] + 1);
        k++;
    }
}
```

Now, we’ll apply this function to every position, starting from the first position.

```cpp
for(int i = 0; i < n; i++) {
    int jumpLength = nums[i];
    computeMinimum(minJumps, jumpLength, n, i);
}
```

The initial declaration would be:

```cpp
int n = nums.size();
vector<int> minJumps(n, INT_MAX);
minJumps[0] = 0;
```

The final code becomes:

```cpp
void computeMinimum(vector<int> &minJumps, int &jumpLength, int &n, int &i) {
    int k = 1;
    while(i + k < n && k <= jumpLength) {
        minJumps[i + k] = min(minJumps[i + k], minJumps[i] + 1);
        k++;
    }
}

int jump(vector<int>& nums) {
    int n = nums.size();
    vector<int> minJumps(n, INT_MAX);
    minJumps[0] = 0;
    for(int i = 0; i < n; i++) {
        int jumpLength = nums[i];
        computeMinimum(minJumps, jumpLength, n, i);
    }
    return minJumps[n - 1];
}
```

- **Time Complexity:** O(n^2)
- **Space Complexity:** O(n) (for storing the `minJumps` array)

### Approach II:

In the second approach, we use a different method that doesn’t require Dynamic Programming. Instead, it’s based on simple mathematical reasoning.

We start by defining `startPoint` and `endPoint`:

```cpp
int startPoint = 0;
int endPoint = 0;
```

For each case, i.e., between the `startPoint` and `endPoint`, we need to compute the farthest index we can reach within this range.

**Base case:** Initially, the farthest index we can reach is 0. If all the values between `startPoint` and `endPoint` are 0, it means we cannot move farther (i.e., we're stuck at that position, not at index 0).

```cpp
int startPoint = 0;
int endPoint = 0;
int minJumps = 0;
int farthestIndex;
```

Now, we need to compute `farthestIndex`:

```cpp
while(endPoint < n - 1) {
    farthestIndex = 0;
    for(int pos = startPoint; pos <= endPoint; pos++) {
        farthestIndex = max(nums[pos] + pos, farthestIndex);
    }
}
```

The `startPoint` would then be the next of `endPoint`, and the new `endPoint` would be the farthest point.

```cpp
while(endPoint < n - 1) {
    farthestIndex = 0;
    for(int pos = startPoint; pos <= endPoint; pos++) {
        farthestIndex = max(nums[pos] + pos, farthestIndex);
    }
    startPoint = endPoint + 1;
    endPoint = farthestIndex;
}
```

If the farthest index is greater than or equal to `n`, it means we can reach `n-1` directly, so no more jumps are needed. If the farthest point is still less than `n-1`, we know we need to make another jump. Let’s incorporate that:

```cpp
while(endPoint < n - 1) {
    farthestIndex = 0;
    for(int pos = startPoint; pos <= endPoint; pos++) {
        farthestIndex = max(nums[pos] + pos, farthestIndex);
    }
    startPoint = endPoint + 1;
    endPoint = farthestIndex;
    if(farthestIndex >= n) {
        return ++minJumps;
    }
    minJumps++;
}
```

Therefore, the full code becomes:

```cpp
int jump(vector<int>& nums) {
    int n = nums.size();
    int startPoint = 0;
    int endPoint = 0;
    int minJumps = 0;
    int farthestIndex;
    while(endPoint < n - 1) {
        farthestIndex = 0;
        for(int pos = startPoint; pos <= endPoint; pos++) {
            farthestIndex = max(nums[pos] + pos, farthestIndex);
        }
        startPoint = endPoint + 1;
        endPoint = farthestIndex;
        if(farthestIndex >= n) {
            return ++minJumps;
        }
        minJumps++;
    }
    return minJumps;
}
```

- **Time Complexity:** O(n^2)
- **Space Complexity:** O(1) (as only variables are used to store the farthest index)

This second approach doesn’t require knowledge of Dynamic Programming as it’s based on simple math.
