---
title: "22. Generate Parentheses"
description: "Day IV"
---

**Input:** `n`

**Each Output String Length:** `2 * n`

## Approach

The initial approach is to recognize that at every position in the resulting string, there are two possible characters: `'('` or `')'`. However, both options come with constraints:

### Constraints:

1. **Number of Opening Brackets (`ob`):**
   - The maximum number of opening brackets in the result is `n`.
   - An opening bracket can only be inserted if its count is less than `n` and the count of opening brackets is greater than or equal to the count of closing brackets.

2. **Number of Closing Brackets (`cb`):**
   - The maximum number of closing brackets in the result is `n`.
   - A closing bracket can only be inserted if its count is less than `n` and is less than the count of opening brackets.
   - **Note:** When the closing bracket count equals the opening bracket count, a closing bracket **cannot** be inserted.

### Code Logic:

```cpp
if (op < n && op >= cb) {
    str[pos] = '(';
}
if (cb < n && cb < op) {
    str[pos] = ')';
}
```

For each case, we recursively call the Generate Parentheses (GP) function, incrementing the position pointer by 1.

```cpp
if (op < n && op >= cb) {
    str[pos] = '(';
    GP(n, str, pos+1, ob+1, cb);
}
if (cb < n && cb < op) {
    str[pos] = ')';
    GP(n, str, pos+1, ob, cb+1);
}
```

### What about base condition?
When the position (pos) is equal to 2 * n, it indicates that all positions from 0 to (2 * n - 1) have been filled, and the generated string can be added to the result.

Note: The result is a vector of strings that will store all valid permutations.

```cpp
if(pos==2*n) results.push_back(str);
if(op < n && op >= cb){
    str[pos] = '(';
    GP(n, str, pos+1, ob+1, cb);
}
if(cb < n && cb < op){
    str[pos] = ')';
    GP(n, str, pos+1, ob, cb+1);
}
```


### Final Code

```cpp
void GP(vector<string> &result, string &str, int n, int pos, int ob, int cb) {
    if (pos >= 2 * n) result.push_back(str);
    if (ob < n && ob >= cb) {
        str[pos] = '(';
        GP(result, str, n, pos+1, ob+1, cb);
    }
    if (cb < n && cb < ob) {
        str[pos] = ')';
        GP(result, str, n, pos+1, ob, cb + 1);
    }
}

// Initial call
vector<string> result;
string str(2 * n, '0');
GP(result, str, n, 0, 0, 0);
```



