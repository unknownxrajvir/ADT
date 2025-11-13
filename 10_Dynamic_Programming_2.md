# Dynamic Programming 2

## 📌 KEY DEFINITIONS

**Edit Distance**: Minimum operations to convert string A to B

**Longest Increasing Subsequence (LIS)**: Longest subsequence where elements are in increasing order

**Matrix Chain Multiplication**: Find optimal order to multiply chain of matrices

**Partition Problem**: Divide set into two subsets with equal sum

**Rod Cutting**: Maximize profit by cutting rod into pieces

---

## 📝 INTERMEDIATE DP PROBLEMS

### 1. EDIT DISTANCE (LEVENSHTEIN DISTANCE)

**Problem**: Convert string A to B using minimum operations
- Insert a character
- Delete a character
- Replace a character

#### Recurrence Relation
```
EDIT_DISTANCE(i, j):
    // Base cases
    if i == 0:
        return j  // Insert j characters
    
    if j == 0:
        return i  // Delete i characters
    
    // If characters match, no operation needed
    if A[i-1] == B[j-1]:
        return EDIT_DISTANCE(i-1, j-1)
    
    // Try all three operations, take minimum
    insert = EDIT_DISTANCE(i, j-1)
    delete = EDIT_DISTANCE(i-1, j)
    replace = EDIT_DISTANCE(i-1, j-1)
    
    return 1 + min(insert, delete, replace)
```

#### Tabulation
```
EDIT_DISTANCE(A, B, m, n):
    dp[m+1][n+1]
    
    // Base cases
    for i = 0 to m:
        dp[i][0] = i  // Delete all
    
    for j = 0 to n:
        dp[0][j] = j  // Insert all
    
    // Fill table
    for i = 1 to m:
        for j = 1 to n:
            if A[i-1] == B[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(
                    dp[i][j-1],    // Insert
                    dp[i-1][j],    // Delete
                    dp[i-1][j-1]   // Replace
                )
    
    return dp[m][n]

Time: O(m × n)
Space: O(m × n)
```

**Example**:
```
A = "sunday"
B = "saturday"

DP Table:
      ""  s  a  t  u  r  d  a  y
""     0  1  2  3  4  5  6  7  8
s      1  0  1  2  3  4  5  6  7
u      2  1  1  2  2  3  4  5  6
n      3  2  2  2  3  3  4  5  6
d      4  3  3  3  3  4  3  4  5
a      5  4  3  4  4  4  4  3  4
y      6  5  4  4  5  5  5  4  3

Edit Distance = 3
Operations: Insert 'a', Insert 't', Replace 'n' with 'r'
```

#### Space Optimized
```
EDIT_DISTANCE_OPT(A, B, m, n):
    prev[n+1]
    curr[n+1]
    
    // Initialize first row
    for j = 0 to n:
        prev[j] = j
    
    for i = 1 to m:
        curr[0] = i
        for j = 1 to n:
            if A[i-1] == B[j-1]:
                curr[j] = prev[j-1]
            else:
                curr[j] = 1 + min(curr[j-1], prev[j], prev[j-1])
        
        prev = curr
    
    return prev[n]

Space: O(n)
```

---

### 2. LONGEST INCREASING SUBSEQUENCE (LIS)

**Problem**: Find length of longest subsequence where elements are in increasing order

#### Method 1: DP O(n²)
```
LIS(arr, n):
    dp[n]  // dp[i] = length of LIS ending at index i
    
    // Initialize all to 1 (each element is LIS of length 1)
    for i = 0 to n-1:
        dp[i] = 1
    
    // Compute LIS for each position
    for i = 1 to n-1:
        for j = 0 to i-1:
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    // Return maximum of all LIS lengths
    return max(dp)

Time: O(n²)
Space: O(n)
```

**Example**:
```
arr = [10, 9, 2, 5, 3, 7, 101, 18]

dp = [1, 1, 1, 2, 2, 3, 4, 4]

LIS = 4 (e.g., [2, 3, 7, 18] or [2, 5, 7, 18])
```

#### Method 2: Binary Search O(n log n)
```
LIS_BINARY_SEARCH(arr, n):
    tails = []  // tails[i] = smallest tail of LIS of length i+1
    
    for num in arr:
        pos = BINARY_SEARCH_LEFT(tails, num)
        
        if pos == tails.size:
            tails.append(num)
        else:
            tails[pos] = num
    
    return tails.size

BINARY_SEARCH_LEFT(tails, target):
    left = 0
    right = tails.size
    
    while left < right:
        mid = (left + right) / 2
        if tails[mid] < target:
            left = mid + 1
        else:
            right = mid
    
    return left

Time: O(n log n)
Space: O(n)
```

**How it works**:
```
arr = [10, 9, 2, 5, 3, 7, 101, 18]

tails progression:
[10]
[9]
[2]
[2, 5]
[2, 3]
[2, 3, 7]
[2, 3, 7, 101]
[2, 3, 7, 18]

Length = 4
```

#### Print LIS
```
PRINT_LIS(arr, n):
    dp[n]
    parent[n] initialized to -1
    
    for i = 0 to n-1:
        dp[i] = 1
    
    maxLen = 1
    maxIndex = 0
    
    for i = 1 to n-1:
        for j = 0 to i-1:
            if arr[j] < arr[i] AND dp[j] + 1 > dp[i]:
                dp[i] = dp[j] + 1
                parent[i] = j
                
                if dp[i] > maxLen:
                    maxLen = dp[i]
                    maxIndex = i
    
    // Reconstruct LIS
    lis = []
    while maxIndex != -1:
        lis.prepend(arr[maxIndex])
        maxIndex = parent[maxIndex]
    
    return lis
```

---

### 3. MATRIX CHAIN MULTIPLICATION

**Problem**: Find minimum scalar multiplications to multiply chain of matrices

**Key Insight**: Order matters!
- A(10×30) × B(30×5) × C(5×60)
- (AB)C = 10×30×5 + 10×5×60 = 1500 + 3000 = 4500
- A(BC) = 30×5×60 + 10×30×60 = 9000 + 18000 = 27000

#### Recurrence
```
MCM(i, j):
    if i == j:
        return 0  // Single matrix, no multiplication
    
    min_cost = INFINITY
    
    // Try splitting at each position k
    for k = i to j-1:
        cost = MCM(i, k) + MCM(k+1, j) + dimensions[i-1] × dimensions[k] × dimensions[j]
        min_cost = min(min_cost, cost)
    
    return min_cost
```

#### Tabulation
```
MCM(dimensions, n):
    // n matrices, n+1 dimensions
    dp[n][n]  // dp[i][j] = min cost to multiply matrices i to j
    
    // Base case: single matrix
    for i = 0 to n-1:
        dp[i][i] = 0
    
    // l is chain length
    for l = 2 to n:
        for i = 0 to n-l:
            j = i + l - 1
            dp[i][j] = INFINITY
            
            // Try all split points
            for k = i to j-1:
                cost = dp[i][k] + dp[k+1][j] + 
                       dimensions[i] × dimensions[k+1] × dimensions[j+1]
                
                dp[i][j] = min(dp[i][j], cost)
    
    return dp[0][n-1]

Time: O(n³)
Space: O(n²)
```

**Example**:
```
Matrices: A(10×30), B(30×5), C(5×60)
dimensions = [10, 30, 5, 60]

DP Table:
     0     1     2
0    0  1500  4500
1    -     0  9000
2    -     -     0

Minimum cost = 4500 (multiply AB first, then multiply with C)
```

---

### 4. PARTITION PROBLEM (EQUAL SUM)

**Problem**: Can array be partitioned into two subsets with equal sum?

#### Approach: Subset Sum Problem
```
CAN_PARTITION(arr, n):
    totalSum = sum(arr)
    
    // If odd, can't partition equally
    if totalSum % 2 != 0:
        return false
    
    target = totalSum / 2
    
    // Check if subset with sum = target exists
    return SUBSET_SUM(arr, n, target)

SUBSET_SUM(arr, n, target):
    dp[n+1][target+1]
    
    // Base cases
    for i = 0 to n:
        dp[i][0] = true  // Sum 0 is always possible (empty subset)
    
    for j = 1 to target:
        dp[0][j] = false  // No elements, can't make positive sum
    
    // Fill table
    for i = 1 to n:
        for j = 1 to target:
            if arr[i-1] <= j:
                dp[i][j] = dp[i-1][j] OR dp[i-1][j - arr[i-1]]
            else:
                dp[i][j] = dp[i-1][j]
    
    return dp[n][target]

Time: O(n × sum)
Space: O(n × sum)
```

**Example**:
```
arr = [1, 5, 11, 5]
sum = 22, target = 11

DP Table (partial):
       0  1  2  3  4  5  6  7  8  9 10 11
0      T  F  F  F  F  F  F  F  F  F  F  F
1(1)   T  T  F  F  F  F  F  F  F  F  F  F
5(5)   T  T  F  F  F  T  T  F  F  F  F  F
11(11) T  T  F  F  F  T  T  F  F  F  F  T
5(5)   T  T  F  F  F  T  T  F  F  F  T  T

Can partition: true ({1,5,5} and {11})
```

#### Space Optimized
```
SUBSET_SUM_OPT(arr, n, target):
    dp[target+1] = false
    dp[0] = true
    
    for i = 0 to n-1:
        for j = target down to arr[i]:
            dp[j] = dp[j] OR dp[j - arr[i]]
    
    return dp[target]

Space: O(target)
```

---

### 5. ROD CUTTING

**Problem**: Cut rod of length n to maximize profit

**Given**: prices[i] = price of rod of length i+1

#### Recurrence
```
ROD_CUTTING(prices, n):
    if n == 0:
        return 0
    
    maxProfit = -INFINITY
    
    // Try all possible first cuts
    for i = 1 to n:
        maxProfit = max(maxProfit, 
                       prices[i-1] + ROD_CUTTING(prices, n-i))
    
    return maxProfit
```

#### Tabulation
```
ROD_CUTTING(prices, n):
    dp[n+1]
    dp[0] = 0
    
    // For each rod length
    for i = 1 to n:
        maxProfit = -INFINITY
        
        // Try all cut positions
        for j = 1 to i:
            maxProfit = max(maxProfit, prices[j-1] + dp[i-j])
        
        dp[i] = maxProfit
    
    return dp[n]

Time: O(n²)
Space: O(n)
```

**Example**:
```
length: 1  2  3  4  5  6  7  8
price:  1  5  8  9 10 17 17 20

DP:
dp[0] = 0
dp[1] = 1 (no cut)
dp[2] = 5 (no cut)
dp[3] = 8 (no cut)
dp[4] = 10 (cut 2+2: 5+5)
dp[5] = 13 (cut 2+3: 5+8)
dp[6] = 17 (no cut)
dp[7] = 18 (cut 2+2+3: 5+5+8)
dp[8] = 22 (cut 2+6: 5+17)

Maximum profit for length 8 = 22
```

#### With Cut Positions
```
ROD_CUTTING_CUTS(prices, n):
    dp[n+1]
    cuts[n+1]  // Store first cut position
    
    dp[0] = 0
    
    for i = 1 to n:
        maxProfit = -INFINITY
        
        for j = 1 to i:
            if prices[j-1] + dp[i-j] > maxProfit:
                maxProfit = prices[j-1] + dp[i-j]
                cuts[i] = j
        
        dp[i] = maxProfit
    
    // Reconstruct cuts
    print "Cuts at: "
    while n > 0:
        print cuts[n]
        n -= cuts[n]
    
    return dp[original_n]
```

---

## 📊 COMPLEXITY COMPARISON

| Problem | Time | Space | Space Optimized |
|---------|------|-------|-----------------|
| **Edit Distance** | O(m×n) | O(m×n) | O(min(m,n)) |
| **LIS (DP)** | O(n²) | O(n) | - |
| **LIS (Binary)** | O(n log n) | O(n) | - |
| **MCM** | O(n³) | O(n²) | - |
| **Partition** | O(n×sum) | O(n×sum) | O(sum) |
| **Rod Cutting** | O(n²) | O(n) | - |

---

## 💡 KEY PATTERNS

### Pattern 1: String DP (Edit Distance)
- 2D table: dp[i][j] for two strings
- Consider match/mismatch at each position

### Pattern 2: Sequence DP (LIS)
- 1D table: dp[i] = answer ending at i
- Consider all previous elements

### Pattern 3: Interval DP (MCM)
- Fill table diagonally by interval length
- Try all split points in interval

### Pattern 4: Subset/Knapsack (Partition)
- 2D table: items vs target
- Include/exclude pattern

### Pattern 5: Unbounded Choice (Rod Cutting)
- Similar to unbounded knapsack
- Can use same choice multiple times

---

## 📖 MUST REMEMBER

1. **Edit Distance**: 3 operations (insert, delete, replace), min of 3 choices
2. **LIS (O(n²))**: dp[i] = max(dp[j]+1) for all j<i where arr[j]<arr[i]
3. **LIS (O(n log n))**: Maintain tails array with binary search
4. **MCM**: Try all split points k, cost includes subproblems + merge cost
5. **Partition**: Reduce to subset sum with target = totalSum/2
6. **Rod Cutting**: Try all first cuts, similar to unbounded knapsack
7. **Space optimization**: Often 2D→1D by keeping only previous row
8. **Interval DP**: Fill by increasing interval length
9. **Print solution**: Track choices or use parent array
10. **Base cases**: Critical for correctness

---

## 🚨 EXAM TIPS

✅ Draw table for small example

✅ Identify state variables clearly

✅ Write recurrence before coding

✅ Check base cases thoroughly

✅ Consider space optimization

✅ Practice printing actual solution, not just optimal value
