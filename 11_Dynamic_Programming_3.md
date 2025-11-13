# Dynamic Programming 3

## 📌 KEY DEFINITIONS

**Longest Palindromic Subsequence (LPS)**: Longest subsequence that reads same forward and backward

**Egg Drop Problem**: Minimum trials to find critical floor

**Catalan Numbers**: Sequence appearing in combinatorial problems

**Bitmask DP**: Use bits to represent state (subset of elements)

**DP on Trees**: Solve DP on tree structure

---

## 📝 ADVANCED DP PROBLEMS

### 1. LONGEST PALINDROMIC SUBSEQUENCE (LPS)

**Problem**: Find length of longest palindromic subsequence

#### Approach 1: Using LCS
```
LPS(s):
    reverse_s = reverse of s
    return LCS(s, reverse_s)

Time: O(n²)
Space: O(n²)
```

#### Approach 2: Direct DP
```
LPS(s, i, j):
    // Base cases
    if i == j:
        return 1  // Single character
    
    if i > j:
        return 0  // Empty string
    
    // If characters match
    if s[i] == s[j]:
        return 2 + LPS(s, i+1, j-1)
    
    // If don't match, try both
    return max(LPS(s, i+1, j), LPS(s, i, j-1))
```

#### Tabulation
```
LPS_TAB(s, n):
    dp[n][n]
    
    // Base case: single character
    for i = 0 to n-1:
        dp[i][i] = 1
    
    // Fill table for increasing lengths
    for len = 2 to n:
        for i = 0 to n-len:
            j = i + len - 1
            
            if s[i] == s[j]:
                if len == 2:
                    dp[i][j] = 2
                else:
                    dp[i][j] = 2 + dp[i+1][j-1]
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])
    
    return dp[0][n-1]

Time: O(n²)
Space: O(n²)
```

**Example**:
```
s = "BBABCBCAB"

DP Table:
    0 1 2 3 4 5 6 7 8
    B B A B C B C A B
0 B 1 2 2 3 3 4 4 4 5
1 B   1 1 2 2 3 3 3 4
2 A     1 1 1 2 2 3 3
3 B       1 1 2 2 2 3
4 C         1 1 2 2 2
5 B           1 1 1 2
6 C             1 1 1
7 A               1 1
8 B                 1

LPS = 5 (BABAB or BCBCB)
```

---

### 2. LONGEST PALINDROMIC SUBSTRING

**Problem**: Find longest contiguous palindromic substring

#### Method 1: DP O(n²)
```
LONGEST_PALINDROME_SUBSTRING(s, n):
    dp[n][n]  // dp[i][j] = true if s[i...j] is palindrome
    start = 0
    maxLen = 1
    
    // Single characters are palindromes
    for i = 0 to n-1:
        dp[i][i] = true
    
    // Check for 2-character palindromes
    for i = 0 to n-2:
        if s[i] == s[i+1]:
            dp[i][i+1] = true
            start = i
            maxLen = 2
    
    // Check for lengths >= 3
    for len = 3 to n:
        for i = 0 to n-len:
            j = i + len - 1
            
            if s[i] == s[j] AND dp[i+1][j-1]:
                dp[i][j] = true
                start = i
                maxLen = len
    
    return s[start...start+maxLen-1]

Time: O(n²)
Space: O(n²)
```

#### Method 2: Expand Around Center O(n²)
```
LONGEST_PALINDROME_EXPAND(s):
    if s.length < 2:
        return s
    
    start = 0
    maxLen = 1
    
    for i = 0 to s.length-1:
        # Odd length palindrome (single center)
        len1 = EXPAND_AROUND_CENTER(s, i, i)
        
        # Even length palindrome (two centers)
        len2 = EXPAND_AROUND_CENTER(s, i, i+1)
        
        len = max(len1, len2)
        
        if len > maxLen:
            maxLen = len
            start = i - (len-1)/2
    
    return s[start...start+maxLen-1]

EXPAND_AROUND_CENTER(s, left, right):
    while left >= 0 AND right < s.length AND s[left] == s[right]:
        left--
        right++
    
    return right - left - 1

Time: O(n²)
Space: O(1)
```

**Note**: For O(n) solution, use Manacher's Algorithm (covered in String Algorithms 2)

---

### 3. EGG DROP PROBLEM

**Problem**: Find minimum trials to find critical floor with k eggs and n floors

**Critical Floor**: Highest floor from which egg doesn't break

#### Recurrence
```
EGG_DROP(eggs, floors):
    // Base cases
    if floors == 0 OR floors == 1:
        return floors
    
    if eggs == 1:
        return floors  // Must try all floors
    
    minTrials = INFINITY
    
    // Try dropping from each floor
    for floor = 1 to floors:
        // Egg breaks: check lower floors with one less egg
        breaks = EGG_DROP(eggs-1, floor-1)
        
        // Egg doesn't break: check higher floors with same eggs
        doesntBreak = EGG_DROP(eggs, floors-floor)
        
        // Worst case (max) + current trial (1)
        trials = 1 + max(breaks, doesntBreak)
        
        minTrials = min(minTrials, trials)
    
    return minTrials
```

#### Tabulation
```
EGG_DROP_TAB(eggs, floors):
    dp[eggs+1][floors+1]
    
    // Base cases
    for i = 1 to eggs:
        dp[i][0] = 0  // 0 floors
        dp[i][1] = 1  // 1 floor
    
    for j = 1 to floors:
        dp[1][j] = j  // 1 egg, need j trials
    
    // Fill table
    for i = 2 to eggs:
        for j = 2 to floors:
            dp[i][j] = INFINITY
            
            for k = 1 to j:
                trials = 1 + max(dp[i-1][k-1], dp[i][j-k])
                dp[i][j] = min(dp[i][j], trials)
    
    return dp[eggs][floors]

Time: O(eggs × floors²)
Space: O(eggs × floors)
```

**Example**:
```
eggs = 2, floors = 10

DP Table:
      0  1  2  3  4  5  6  7  8  9 10
1 egg 0  1  2  3  4  5  6  7  8  9 10
2 egg 0  1  2  2  3  3  3  4  4  4  4

Minimum trials = 4
```

#### Optimized with Binary Search
```
EGG_DROP_BINARY(eggs, floors):
    dp[eggs+1][floors+1]
    
    // Base cases (same as above)
    
    for i = 2 to eggs:
        for j = 2 to floors:
            low = 1
            high = j
            
            while low <= high:
                mid = (low + high) / 2
                
                breaks = dp[i-1][mid-1]
                doesntBreak = dp[i][j-mid]
                
                if breaks > doesntBreak:
                    high = mid - 1
                else:
                    low = mid + 1
                
                dp[i][j] = min(dp[i][j], 1 + max(breaks, doesntBreak))
    
    return dp[eggs][floors]

Time: O(eggs × floors × log floors)
```

---

### 4. CATALAN NUMBERS

**Definition**: C(n) = (2n)! / ((n+1)! × n!)

**Recurrence**: C(n) = Σ C(i) × C(n-1-i) for i = 0 to n-1

**Applications**:
- Number of binary search trees with n nodes
- Number of ways to parenthesize expression
- Number of valid parentheses sequences
- Number of paths in grid not crossing diagonal

#### DP Solution
```
CATALAN(n):
    dp[n+1]
    dp[0] = 1
    dp[1] = 1
    
    for i = 2 to n:
        dp[i] = 0
        for j = 0 to i-1:
            dp[i] += dp[j] × dp[i-1-j]
    
    return dp[n]

Time: O(n²)
Space: O(n)
```

**First few Catalan numbers**:
```
C(0) = 1
C(1) = 1
C(2) = 2
C(3) = 5
C(4) = 14
C(5) = 42
C(6) = 132
```

#### Application: Count BSTs
```
COUNT_BST(n):
    return CATALAN(n)

Example: n=3
Trees: 5
  1      1      2      3    3
   \      \    / \    /    /
    2      3  1   3  1    2
     \    /            \  /
      3  2              2 1
```

---

### 5. TRAVELLING SALESMAN PROBLEM (TSP) - BITMASK DP

**Problem**: Visit all cities exactly once and return to start with minimum cost

#### Using Bitmask DP
```
TSP(dist, n):
    // dp[mask][i] = min cost to visit cities in mask, ending at i
    dp[2^n][n]
    
    // Initialize with infinity
    for mask = 0 to 2^n-1:
        for i = 0 to n-1:
            dp[mask][i] = INFINITY
    
    // Base case: start at city 0
    dp[1][0] = 0
    
    // Fill DP table
    for mask = 0 to 2^n-1:
        for i = 0 to n-1:
            // If city i is in mask
            if mask & (1 << i):
                for j = 0 to n-1:
                    // If city j is also in mask and i != j
                    if mask & (1 << j) AND i != j:
                        prevMask = mask XOR (1 << i)
                        dp[mask][i] = min(dp[mask][i], 
                                         dp[prevMask][j] + dist[j][i])
    
    // Find minimum cost returning to start
    fullMask = (1 << n) - 1
    minCost = INFINITY
    
    for i = 1 to n-1:
        minCost = min(minCost, dp[fullMask][i] + dist[i][0])
    
    return minCost

Time: O(n² × 2^n)
Space: O(n × 2^n)
```

**Example**:
```
Cities: 0, 1, 2, 3
Distance matrix:
    0  1  2  3
0   0 10 15 20
1  10  0 35 25
2  15 35  0 30
3  20 25 30  0

Minimum cost = 80
Path: 0 → 1 → 3 → 2 → 0
```

---

### 6. MAXIMUM SUM INCREASING SUBSEQUENCE

**Problem**: Find maximum sum of increasing subsequence

```
MAX_SUM_INCREASING(arr, n):
    dp[n]  // dp[i] = max sum of increasing subsequence ending at i
    
    // Initialize with array elements
    for i = 0 to n-1:
        dp[i] = arr[i]
    
    // Compute max sum for each position
    for i = 1 to n-1:
        for j = 0 to i-1:
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + arr[i])
    
    return max(dp)

Time: O(n²)
Space: O(n)
```

**Example**:
```
arr = [1, 101, 2, 3, 100, 4, 5]

dp = [1, 102, 3, 6, 106, 10, 15]

Maximum sum = 106 (1 + 2 + 3 + 100)
```

---

### 7. DP ON TREES

**Problem**: Maximum independent set in tree (no adjacent vertices)

```
MAX_INDEPENDENT_SET(node, include_parent):
    if node == NULL:
        return 0
    
    if include_parent:
        // Can't include current node
        return MAX_INDEPENDENT_SET(node.left, false) + 
               MAX_INDEPENDENT_SET(node.right, false)
    else:
        // Can choose to include or exclude current node
        include = node.value + 
                 MAX_INDEPENDENT_SET(node.left, true) + 
                 MAX_INDEPENDENT_SET(node.right, true)
        
        exclude = MAX_INDEPENDENT_SET(node.left, false) + 
                 MAX_INDEPENDENT_SET(node.right, false)
        
        return max(include, exclude)

Time: O(n) with memoization
Space: O(n)
```

---

## 💡 ADVANCED DP TECHNIQUES

### 1. Bitmask DP
- Use bits to represent subset
- State: dp[mask][additional_params]
- Useful when n ≤ 20

### 2. Digit DP
- Count numbers with specific properties
- State: dp[position][tight][other_params]

### 3. DP on Trees
- Two states: include/exclude node
- Process in post-order

### 4. DP with Binary Search
- Optimize inner loop with binary search
- Example: LIS in O(n log n)

### 5. State Space Reduction
- Reduce dimensions when possible
- Rolling array technique

---

## 📊 COMPLEXITY SUMMARY

| Problem | Time | Space | Key Technique |
|---------|------|-------|---------------|
| **LPS** | O(n²) | O(n²) | Interval DP |
| **Palindrome Substring** | O(n²) | O(1) | Expand center |
| **Egg Drop** | O(k×n²) | O(k×n) | 2D DP |
| **Egg Drop (opt)** | O(k×n log n) | O(k×n) | Binary search |
| **Catalan** | O(n²) | O(n) | Recurrence sum |
| **TSP** | O(n²×2^n) | O(n×2^n) | Bitmask DP |
| **Max Sum IS** | O(n²) | O(n) | Like LIS |
| **DP on Trees** | O(n) | O(n) | Tree DP |

---

## 🚨 KEY INSIGHTS

### Palindrome Problems
- LPS: Use interval DP or LCS with reverse
- Substring: Expand around center for O(1) space

### Egg Drop
- Binary search optimization important
- Think worst case at each floor

### Catalan Numbers
- Recognize pattern: nested structures
- C(n) involves sum of products

### Bitmask DP
- Set bit = element included
- Check bit: mask & (1 << i)
- Toggle bit: mask XOR (1 << i)

### DP on Trees
- Usually two choices: include/exclude node
- Use post-order traversal

---

## 📖 MUST REMEMBER

1. **LPS**: Like LCS but with string and its reverse
2. **Palindrome Substring**: Expand around center for space efficiency
3. **Egg Drop**: 1 + max(break, don't break) with min over all floors
4. **Catalan**: Sum of products of smaller Catalans
5. **TSP Bitmask**: dp[mask][city] with mask representing visited cities
6. **Bitmask operations**: & for check, | for set, XOR for toggle
7. **DP on Trees**: Include/exclude pattern, process bottom-up
8. **Optimization**: Binary search, space reduction, memoization
9. **State design**: Critical for efficiency
10. **Practice recognizing**: Which DP pattern applies to new problem

---

## 🎯 EXAM TIPS

✅ Recognize Catalan number problems (nested structures)

✅ Bitmask DP when n ≤ 20 and need subset representation

✅ Tree DP: Think include/exclude for each node

✅ Palindrome: Consider expand-around-center for O(1) space

✅ Always check if binary search can optimize DP

✅ Draw recursion tree for small examples

✅ Bit manipulation: Practice (1<<i), &, |, XOR operations
