# Range Query Algorithms - Sparse Table

## 📌 KEY DEFINITIONS

**Sparse Table**: Data structure for answering **static** range queries in O(1) time

**Static**: Array doesn't change after preprocessing (no updates)

**Idempotent Function**: Function where f(x, x) = x (e.g., min, max, gcd)

**Precomputation**: Build table once in O(n log n) time

**Range Query**: Answer queries in O(1) or O(log n) time

---

## 🎯 SPARSE TABLE vs SEGMENT TREE

| Feature | Sparse Table | Segment Tree |
|---------|--------------|--------------|
| **Updates** | ❌ No | ✅ Yes |
| **Query Time** | O(1) | O(log n) |
| **Build Time** | O(n log n) | O(n) |
| **Space** | O(n log n) | O(n) |
| **Best For** | Static queries | Dynamic queries |
| **Works With** | Idempotent functions | Any merge function |

---

## 🏗️ SPARSE TABLE STRUCTURE

### Concept
- `sparse[i][j]` = answer for range [i, i + 2^j - 1]
- Stores answers for all ranges of length 2^j starting at each position
- Uses dynamic programming to build

### Size Calculation
```
Rows: n (number of elements)
Columns: ⌈log₂(n)⌉ + 1
Total space: O(n log n)
```

**Visual Example**:
```
Array: [2, 4, 3, 1, 6, 7, 8, 9, 1, 7]
Index:  0  1  2  3  4  5  6  7  8  9

sparse[i][j] represents min of range [i, i+2^j-1]

j=0 (length 1): sparse[i][0] = arr[i]
j=1 (length 2): sparse[i][1] = min(arr[i], arr[i+1])
j=2 (length 4): sparse[i][2] = min of 4 elements starting at i
j=3 (length 8): sparse[i][3] = min of 8 elements starting at i
```

---

## 📝 SPARSE TABLE OPERATIONS

### 1. BUILD SPARSE TABLE

```
BUILD_SPARSE_TABLE(arr, n):
    // Calculate maximum power of 2
    maxLog = floor(log₂(n)) + 1
    
    sparse[n][maxLog]
    
    // Initialize for intervals of length 1
    for i = 0 to n-1:
        sparse[i][0] = arr[i]
    
    // Build table for increasing powers of 2
    for j = 1 to maxLog-1:
        for i = 0 to n - (1 << j):
            // Combine two smaller ranges
            sparse[i][j] = min(
                sparse[i][j-1],
                sparse[i + (1 << (j-1))][j-1]
            )

Time: O(n log n)
Space: O(n log n)
```

**Explanation**:
```
sparse[i][j] = min(sparse[i][j-1], sparse[i + 2^(j-1)][j-1])

Range [i, i+2^j-1] is split into:
- Left half: [i, i+2^(j-1)-1]
- Right half: [i+2^(j-1), i+2^j-1]

Both halves have length 2^(j-1)
```

**Example**:
```
arr = [2, 4, 3, 1, 6, 7, 8, 9, 1, 7]

sparse table (for min):
i\j  0  1  2  3
0    2  2  1  1
1    4  3  1  1
2    3  1  1  1
3    1  1  1  1
4    6  6  6  1
5    7  7  7  1
6    8  8  1  1
7    9  1  1  -
8    1  1  -  -
9    7  -  -  -

sparse[0][2] = min of [2,4,3,1] = 1
sparse[0][3] = min of [2,4,3,1,6,7,8,9] = 1
```

---

### 2. RANGE QUERY (Min/Max/GCD)

#### For Idempotent Functions (O(1) Query)

```
QUERY_MIN(sparse, L, R):
    // Calculate length of range
    length = R - L + 1
    
    // Find largest power of 2 that fits in range
    k = floor(log₂(length))
    
    // Query two overlapping ranges
    return min(
        sparse[L][k],
        sparse[R - (1 << k) + 1][k]
    )

Time: O(1)
```

**Why overlapping works for min/max/gcd**:
```
min(x, x) = x (idempotent property)

Range [L, R] is covered by:
- [L, L + 2^k - 1]
- [R - 2^k + 1, R]

These ranges overlap, but min/max/gcd gives correct answer
```

**Example**:
```
arr = [2, 4, 3, 1, 6, 7, 8, 9, 1, 7]
Query: min(2, 7)

length = 7 - 2 + 1 = 6
k = floor(log₂(6)) = 2  (2^2 = 4)

Query two ranges of length 4:
- sparse[2][2] = min(3, 1, 6, 7) = 1
- sparse[7-4+1][2] = sparse[4][2] = min(6, 7, 8, 9) = 6

Result: min(1, 6) = 1 ✓
```

#### For Non-Idempotent Functions (O(log n) Query)

```
QUERY_SUM(sparse, L, R):
    sum = 0
    k = floor(log₂(R - L + 1))
    
    // Process ranges without overlap
    while L <= R:
        k = floor(log₂(R - L + 1))
        sum += sparse[L][k]
        L += (1 << k)
    
    return sum

Time: O(log n)
```

**Why overlapping doesn't work for sum**:
```
sum(x, x) = 2x ≠ x (not idempotent)

Must use non-overlapping ranges
```

---

## 💡 COMMON USE CASES

### 1. Range Minimum Query (RMQ)
```
BUILD_RMQ(arr, n):
    sparse[n][log n]
    
    // Base case
    for i = 0 to n-1:
        sparse[i][0] = arr[i]
    
    // Fill table
    for j = 1; (1 << j) <= n; j++:
        for i = 0; i + (1 << j) <= n; i++:
            sparse[i][j] = min(sparse[i][j-1], 
                              sparse[i + (1 << (j-1))][j-1])

QUERY_RMQ(L, R):
    k = floor(log₂(R - L + 1))
    return min(sparse[L][k], sparse[R - (1 << k) + 1][k])
```

### 2. Range Maximum Query
```
Same as RMQ, but use max() instead of min()
```

### 3. Range GCD Query
```
BUILD_GCD(arr, n):
    // Same structure as RMQ
    sparse[i][j] = gcd(sparse[i][j-1], 
                       sparse[i + (1 << (j-1))][j-1])

QUERY_GCD(L, R):
    k = floor(log₂(R - L + 1))
    return gcd(sparse[L][k], sparse[R - (1 << k) + 1][k])
```

### 4. Range Bitwise OR/AND
```
OR and AND are idempotent:
or(x, x) = x
and(x, x) = x

Use same O(1) query approach
```

---

## 🎯 IMPLEMENTATION

### Complete Sparse Table Implementation
```python
import math

class SparseTable:
    def __init__(self, arr):
        self.n = len(arr)
        self.maxLog = int(math.log2(self.n)) + 1
        self.sparse = [[0] * self.maxLog for _ in range(self.n)]
        self.build(arr)
    
    def build(self, arr):
        # Initialize for length 1
        for i in range(self.n):
            self.sparse[i][0] = arr[i]
        
        # Build for increasing lengths
        j = 1
        while (1 << j) <= self.n:
            i = 0
            while i + (1 << j) <= self.n:
                self.sparse[i][j] = min(
                    self.sparse[i][j-1],
                    self.sparse[i + (1 << (j-1))][j-1]
                )
                i += 1
            j += 1
    
    def query(self, L, R):
        # O(1) query for min
        length = R - L + 1
        k = int(math.log2(length))
        
        return min(
            self.sparse[L][k],
            self.sparse[R - (1 << k) + 1][k]
        )

# Usage
arr = [2, 4, 3, 1, 6, 7, 8, 9, 1, 7]
st = SparseTable(arr)
print(st.query(2, 7))  # Output: 1
```

---

## 📊 IDEMPOTENT FUNCTIONS

### What is Idempotent?
```
Function f is idempotent if: f(x, x) = x

Examples:
- min(x, x) = x ✓
- max(x, x) = x ✓
- gcd(x, x) = x ✓
- or(x, x) = x ✓
- and(x, x) = x ✓

Counter-examples:
- sum(x, x) = 2x ✗
- product(x, x) = x² ✗
- xor(x, x) = 0 ✗
```

### Why Idempotent Matters?
```
Allows overlapping ranges in query:

For min:
Range [2, 7] covered by [2, 5] and [4, 7]
Elements 4, 5 counted twice
min(a, a, b, c) = min(a, b, c) ✓

For sum:
sum(a, a, b, c) = 2a + b + c ≠ a + b + c ✗
```

---

## 💡 OPTIMIZATION TECHNIQUES

### 1. Precompute Logarithms
```
// Instead of calculating log in each query
logValues[n]
for i = 2 to n:
    logValues[i] = logValues[i/2] + 1

QUERY_OPTIMIZED(L, R):
    k = logValues[R - L + 1]
    return min(sparse[L][k], sparse[R - (1 << k) + 1][k])
```

### 2. Space Optimization
```
// Only store necessary columns
maxLog = floor(log₂(n)) + 1
Not all n×log(n) cells are needed
```

---

## 📊 COMPLEXITY COMPARISON

| Operation | Sparse Table | Segment Tree | Array |
|-----------|--------------|--------------|-------|
| **Build** | O(n log n) | O(n) | O(1) |
| **Query (min)** | O(1) | O(log n) | O(n) |
| **Update** | ❌ O(n log n) rebuild | ✅ O(log n) | ✅ O(1) |
| **Space** | O(n log n) | O(n) | O(n) |

---

## 🚨 COMMON MISTAKES

❌ Using sparse table when updates are needed → Use segment tree

❌ Overlapping ranges for sum queries → Use O(log n) approach

❌ Wrong calculation of k = floor(log₂(R - L + 1))

❌ Not handling 1-indexed vs 0-indexed properly

❌ Forgetting that 1 << j means 2^j

---

## 📖 MUST REMEMBER

1. **Static only**: No updates after building
2. **Build time**: O(n log n) - precompute all ranges of length 2^j
3. **Query time**: O(1) for min/max/gcd, O(log n) for sum
4. **Space**: O(n log n) - store ranges for each power of 2
5. **Idempotent**: f(x,x) = x allows overlapping ranges
6. **Formula**: sparse[i][j] = min(sparse[i][j-1], sparse[i+2^(j-1)][j-1])
7. **Query k**: k = floor(log₂(length)) gives largest power of 2 ≤ length
8. **Overlap OK**: Only for idempotent functions
9. **vs Segment Tree**: Faster query, but no updates
10. **Best use**: Static RMQ problems with many queries

---

## 🎯 EXAM TIPS

✅ Draw small table to visualize structure

✅ Trace build process for small array

✅ Understand why overlapping works for min/max

✅ Know when to use sparse table vs segment tree

✅ Practice log₂ calculations

✅ Remember bit shift: (1 << j) = 2^j

✅ Identify idempotent functions quickly

✅ Compare trade-offs: space vs time, static vs dynamic
