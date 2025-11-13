# Dynamic Programming 1

## 📌 KEY DEFINITIONS

**Dynamic Programming (DP)**: Optimization technique that solves complex problem by breaking it down into simpler overlapping subproblems and storing results

**Overlapping Subproblems**: Same subproblems are solved multiple times

**Optimal Substructure**: Optimal solution contains optimal solutions to subproblems

**Memoization**: Top-down approach, store results in cache (recursive)

**Tabulation**: Bottom-up approach, fill table iteratively

**State**: Set of variables that uniquely identify a subproblem

---

## 🎯 DP vs OTHER TECHNIQUES

| Feature | DP | Greedy | Backtracking |
|---------|----|----|-------|
| **Approach** | Optimal substructure | Local optimum | Try all solutions |
| **Subproblems** | Overlapping | Independent | May overlap |
| **Solution** | Always optimal | May not be optimal | Enumerate all |
| **Time** | Polynomial | Usually O(n log n) | Exponential |
| **Example** | 0/1 Knapsack | Fractional Knapsack | N-Queens |

---

## 📝 DP APPROACHES

### 1. Memoization (Top-Down)
```
FUNCTION(n, memo):
    // Base case
    if n meets base condition:
        return base_value
    
    // Check if already computed
    if n in memo:
        return memo[n]
    
    // Compute and store
    memo[n] = RECURSIVE_COMPUTATION(n)
    return memo[n]

Pros: Intuitive, only computes needed subproblems
Cons: Recursion overhead, stack space
```

### 2. Tabulation (Bottom-Up)
```
FUNCTION(n):
    // Create table
    dp = array of size n+1
    
    // Initialize base cases
    dp[0] = base_value
    
    // Fill table iteratively
    for i = 1 to n:
        dp[i] = COMPUTE_FROM_PREVIOUS(dp, i)
    
    return dp[n]

Pros: No recursion, better performance
Cons: Must compute all subproblems
```

---

## 📊 CLASSIC DP PROBLEMS

### 1. FIBONACCI SEQUENCE

**Problem**: Find nth Fibonacci number

#### Naive Recursion (Exponential)
```
FIBONACCI(n):
    if n <= 1:
        return n
    return FIBONACCI(n-1) + FIBONACCI(n-2)

Time: O(2^n)
Space: O(n) - recursion stack
```

#### Memoization (Top-Down)
```
FIBONACCI_MEMO(n, memo):
    if n <= 1:
        return n
    
    if n in memo:
        return memo[n]
    
    memo[n] = FIBONACCI_MEMO(n-1, memo) + FIBONACCI_MEMO(n-2, memo)
    return memo[n]

Time: O(n)
Space: O(n)
```

#### Tabulation (Bottom-Up)
```
FIBONACCI_TAB(n):
    if n <= 1:
        return n
    
    dp[0] = 0
    dp[1] = 1
    
    for i = 2 to n:
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

Time: O(n)
Space: O(n)
```

#### Space Optimized
```
FIBONACCI_OPTIMIZED(n):
    if n <= 1:
        return n
    
    prev2 = 0
    prev1 = 1
    
    for i = 2 to n:
        curr = prev1 + prev2
        prev2 = prev1
        prev1 = curr
    
    return prev1

Time: O(n)
Space: O(1)
```

---

### 2. LONGEST COMMON SUBSEQUENCE (LCS)

**Problem**: Find length of longest subsequence common to two strings

**Subsequence**: Characters in same order but not necessarily consecutive

#### Recurrence Relation
```
LCS(i, j):
    if i == 0 OR j == 0:
        return 0
    
    if X[i-1] == Y[j-1]:
        return 1 + LCS(i-1, j-1)
    else:
        return max(LCS(i-1, j), LCS(i, j-1))
```

#### Tabulation
```
LCS(X, Y, m, n):
    dp[m+1][n+1]
    
    // Initialize base cases
    for i = 0 to m:
        dp[i][0] = 0
    for j = 0 to n:
        dp[0][j] = 0
    
    // Fill table
    for i = 1 to m:
        for j = 1 to n:
            if X[i-1] == Y[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

Time: O(m × n)
Space: O(m × n)
```

**Example**:
```
X = "ABCDGH"
Y = "AEDFHR"

DP Table:
    ""  A  E  D  F  H  R
""   0  0  0  0  0  0  0
A    0  1  1  1  1  1  1
B    0  1  1  1  1  1  1
C    0  1  1  1  1  1  1
D    0  1  1  2  2  2  2
G    0  1  1  2  2  2  2
H    0  1  1  2  2  3  3

LCS = 3 (ADH)
```

#### Print LCS
```
PRINT_LCS(dp, X, Y, i, j):
    if i == 0 OR j == 0:
        return ""
    
    if X[i-1] == Y[j-1]:
        return PRINT_LCS(dp, X, Y, i-1, j-1) + X[i-1]
    
    if dp[i-1][j] > dp[i][j-1]:
        return PRINT_LCS(dp, X, Y, i-1, j)
    else:
        return PRINT_LCS(dp, X, Y, i, j-1)
```

---

### 3. 0/1 KNAPSACK

**Problem**: Maximize value with weight constraint (can't take fractions)

#### Recurrence
```
KNAPSACK(i, w):
    if i == 0 OR w == 0:
        return 0
    
    if weight[i-1] > w:
        // Can't include this item
        return KNAPSACK(i-1, w)
    else:
        // Max of including or excluding
        include = value[i-1] + KNAPSACK(i-1, w - weight[i-1])
        exclude = KNAPSACK(i-1, w)
        return max(include, exclude)
```

#### Tabulation
```
KNAPSACK(weights, values, n, W):
    dp[n+1][W+1]
    
    // Initialize base cases
    for i = 0 to n:
        dp[i][0] = 0
    for w = 0 to W:
        dp[0][w] = 0
    
    // Fill table
    for i = 1 to n:
        for w = 1 to W:
            if weights[i-1] <= w:
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],  // Include
                    dp[i-1][w]                                 // Exclude
                )
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][W]

Time: O(n × W)
Space: O(n × W)
```

**Example**:
```
weights = [1, 3, 4, 5]
values = [1, 4, 5, 7]
W = 7

DP Table:
     0  1  2  3  4  5  6  7
0    0  0  0  0  0  0  0  0
1    0  1  1  1  1  1  1  1
3    0  1  1  4  5  5  5  5
4    0  1  1  4  5  6  6  9
5    0  1  1  4  5  7  8  9

Max value = 9 (items 2 and 3: 4+5)
```

#### Space Optimized (1D Array)
```
KNAPSACK_OPTIMIZED(weights, values, n, W):
    dp[W+1]
    
    for i = 0 to n-1:
        // Traverse from right to left
        for w = W down to weights[i]:
            dp[w] = max(dp[w], values[i] + dp[w - weights[i]])
    
    return dp[W]

Time: O(n × W)
Space: O(W)
```

---

### 4. COIN CHANGE

#### Problem 1: Minimum Coins for Amount

**Problem**: Find minimum coins needed to make amount

```
COIN_CHANGE_MIN(coins, amount):
    dp[amount+1]
    dp[0] = 0
    
    // Initialize with infinity
    for i = 1 to amount:
        dp[i] = INFINITY
    
    for i = 1 to amount:
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], 1 + dp[i - coin])
    
    return dp[amount] if dp[amount] != INFINITY else -1

Time: O(amount × coins)
Space: O(amount)
```

**Example**:
```
coins = [1, 2, 5]
amount = 11

DP: [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]

Answer: 3 (5 + 5 + 1)
```

#### Problem 2: Number of Ways to Make Amount

**Problem**: Count ways to make amount using coins

```
COIN_CHANGE_WAYS(coins, amount):
    dp[amount+1]
    dp[0] = 1  // One way to make 0: use no coins
    
    for coin in coins:
        for i = coin to amount:
            dp[i] += dp[i - coin]
    
    return dp[amount]

Time: O(amount × coins)
Space: O(amount)
```

**Example**:
```
coins = [1, 2, 5]
amount = 5

DP progression:
After coin 1: [1, 1, 1, 1, 1, 1]
After coin 2: [1, 1, 2, 2, 3, 3]
After coin 5: [1, 1, 2, 2, 3, 4]

Answer: 4 ways (5), (2+2+1), (2+1+1+1), (1+1+1+1+1)
```

---

### 5. CLIMBING STAIRS

**Problem**: Count ways to reach nth stair (can climb 1 or 2 steps)

```
CLIMBING_STAIRS(n):
    if n <= 2:
        return n
    
    dp[n+1]
    dp[1] = 1  // 1 way to reach stair 1
    dp[2] = 2  // 2 ways to reach stair 2
    
    for i = 3 to n:
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

Time: O(n)
Space: O(n)

Note: This is essentially Fibonacci!
```

**Space Optimized**:
```
CLIMBING_STAIRS_OPT(n):
    if n <= 2:
        return n
    
    prev2 = 1
    prev1 = 2
    
    for i = 3 to n:
        curr = prev1 + prev2
        prev2 = prev1
        prev1 = curr
    
    return prev1

Space: O(1)
```

---

## 🎓 DP PROBLEM SOLVING STEPS

### Step 1: Identify DP Problem
✅ Overlapping subproblems
✅ Optimal substructure
✅ "Count ways", "maximum/minimum", "longest/shortest"

### Step 2: Define State
What variables uniquely identify subproblem?
- Fibonacci: dp[i] = ith number
- LCS: dp[i][j] = LCS of X[0...i], Y[0...j]
- Knapsack: dp[i][w] = max value with i items, weight w

### Step 3: Write Recurrence
Express current state in terms of previous states

### Step 4: Identify Base Cases
Smallest subproblems with known answers

### Step 5: Decide Approach
- Memoization if not all subproblems needed
- Tabulation for better performance

### Step 6: Optimize Space
Can you reduce dimensions?

---

## 📖 MUST REMEMBER

1. **DP = Recursion + Memoization** OR **Iteration + Table**
2. **Two properties**: Overlapping subproblems + Optimal substructure
3. **Fibonacci**: Classic DP, O(n) with memoization
4. **LCS**: 2D DP, dp[i][j] based on match or max of neighbors
5. **0/1 Knapsack**: Include/exclude pattern, O(n×W)
6. **Coin Change (min)**: dp[i] = min(dp[i], 1 + dp[i-coin])
7. **Coin Change (ways)**: dp[i] += dp[i-coin], iterate coins first!
8. **Space optimization**: Often reduce from 2D to 1D
9. **Base cases**: Critical for correctness
10. **Drawing table** helps understand state transitions

---

## 🚨 COMMON MISTAKES

❌ Not identifying overlapping subproblems

❌ Wrong recurrence relation

❌ Incorrect base cases

❌ Wrong iteration order in tabulation

❌ Coin change ways: iterating amount before coins (creates duplicates)

---

## 🎯 EXAM TIPS

✅ Draw small example and manually compute table

✅ Verify recurrence with example

✅ Check base cases carefully

✅ Consider space optimization after correct solution

✅ Practice both memoization and tabulation
