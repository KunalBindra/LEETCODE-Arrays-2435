# LEETCODE-Arrays-2435
---

# ✅ **1. What code does**

The function counts **how many paths from (0,0) → (m-1,n-1)** such that:

```
(sum of values along the path) % k == 0
```

Allowed moves: **only RIGHT or DOWN**

You keep track of `remain` = **sum of path so far modulo k**.

Memo table `t[row][col][remain]` stores:

```
Number of valid paths from (row, col) onward 
given current remainder = remain
```

---

# ✅ **2. Recursion structure**

At each cell `(row, col)`:

1. **Out of bounds → 0**
2. **If at bottom-right → check if condition satisfies**
3. **If memo exists → return it**
4. **Else compute:**

   * newRemain = (remain + grid[row][col]) % k
   * down paths
   * right paths
   * return (down + right)

---

# ✅ **3. Dry run on a small example**

Let’s dry run on the simplest non-trivial case:

```
grid = [
  [1,2],
  [3,4]
]
k = 3
```

All possible paths:

* Path 1: 1 → 2 → 4

  * sum = 7 → 7%3 = 1 ❌
* Path 2: 1 → 3 → 4

  * sum = 8 → 8%3 = 2 ❌

Expected answer = **0**

---

## 🔍 Start:

`t[2][2][3]` initialized to null.

### Call: `solve(0,0,0)`

* at cell (0,0) value = 1
* newRemain = (0 + 1) % 3 = **1**
* need to go DOWN and RIGHT

---

## 1️⃣ **DOWN move**

Call: `solve(1,0,1)`
Cell value = 3
newRemain = (1 + 3) % 3 = **1**

### ↓ further down:

Call: `solve(2,0,x)` → out of bounds → returns **0**

### → right move:

Call: `solve(1,1,1)`

* This is destination cell
* value = 4
* final sum remainder = (1 + 4) % 3 = 5 % 3 = **2**
* 2 != 0 → return **0**

So from (1,0,1) → total paths = **0**

Store in memo:
`t[1][0][1] = 0`

---

## 2️⃣ **RIGHT move**

Call: `solve(0,1,1)`
Cell value = 2
newRemain = (1 + 2) % 3 = **0**

### ↓ down move:

Call: `solve(1,1,0)`

* destination cell
* value = 4
* final remainder = (0 + 4) % 3 = 1 ≠ 0 → return **0**

### → right move:

Call: `solve(0,2,x)` → out of bounds → return **0**

So from (0,1,1) → total = **0**

Store:
`t[0][1][1] = 0`

---

## 🟢 Final aggregation at (0,0,0)

```
down = 0
right = 0
return 0
```

Stored:
`t[0][0][0] = 0`

---

# 🎉 Final answer = **0**

---

# ✅ **4. Important Notes / Why your code is correct**

* `remain` stores remainder **before adding the current cell**
  You correctly update `newRemain` for child calls.
* Memo dimension `t[m][n][k]` is correct.
* Base case returns 1 only when **including final cell** makes `(sum % k == 0)`

---
