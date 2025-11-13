# 📚 PART B - COMPLETE DETAILED ANSWERS

## ALL 16-MARK & 10-MARK QUESTIONS WITH FULL SOLUTIONS

---

## Q8. (16 MARKS) 0/1 KNAPSACK PROBLEM

**COMPLETE ANSWER:**

### 1. PROBLEM DEFINITION (2 marks)

**0/1 Knapsack Problem:**
Given n items with weights w[i] and values v[i], and knapsack capacity W, select items to maximize total value without exceeding capacity. Each item: take completely (1) or leave (0) - **no fractions**.

**Input:** n items, weights w[], values v[], capacity W
**Output:** Maximum value achievable
**Constraint:** Cannot break items

### 2. WHY DYNAMIC PROGRAMMING? (2 marks)

**Overlapping Subproblems:**
Deciding on item i with capacity w needs solutions for:
- Same items 1..i-1 with capacity w (exclude item)
- Same items 1..i-1 with capacity w-weight[i] (include item)
These repeat → store results!

**Optimal Substructure:**
Optimal solution contains optimal solutions to subproblems.

**Why NOT Greedy:** Greedy by value/weight ratio fails for 0/1 version

### 3. RECURRENCE RELATION (3 marks)

**State:**
```
dp[i][w] = Maximum value using first i items with capacity w
```

**Base Cases:**
```
dp[0][w] = 0  (no items)
dp[i][0] = 0  (no capacity)
```

**Recurrence:**
```
If weight[i] > w:
    dp[i][w] = dp[i-1][w]  // Too heavy, skip
Else:
    dp[i][w] = max(
        dp[i-1][w],                      // Exclude item i
        value[i] + dp[i-1][w-weight[i]]  // Include item i
    )
```

### 4. TABULATION (4 marks)

**Algorithm:**
```
KNAPSACK_DP(w[], v[], n, W):
1. Create table dp[0..n][0..W], initialize to 0
2. For i = 1 to n:
      For w = 1 to W:
          if weight[i] <= w:
              dp[i][w] = max(dp[i-1][w], 
                            value[i] + dp[i-1][w-weight[i]])
          else:
              dp[i][w] = dp[i-1][w]
3. Return dp[n][W]
```

### 5. NUMERICAL EXAMPLE (3 marks)

**Given:**
```
Item    Weight    Value
1       2kg       ₹12
2       1kg       ₹10
3       3kg       ₹20
4       2kg       ₹15

Capacity W = 5kg
```

**DP Table:**
```
Cap→    0   1   2   3   4   5
Items↓
0       0   0   0   0   0   0
1(2,12) 0   0  12  12  12  12
2(1,10) 0  10  12  22  22  22
3(3,20) 0  10  12  22  30  32
4(2,15) 0  10  15  25  30  37
```

**Filling Logic:**
- Row 1: Item 1 (w=2, v=12) fits from capacity 2 onwards
- Row 2: Item 2 (w=1, v=10) at cap 3: max(12, 10+12)=22 (both items!)
- Row 3: At cap 5: max(22, 20+12)=32
- Row 4: At cap 5: max(32, 15+22)=37 ✓

**Maximum Value = ₹37**

**Items selected:** 1, 2, 4 (backtrack from dp[4][5])
Total: 2+1+2=5kg, Value: 12+10+15=37 ✓

### 6. COMPLEXITY ANALYSIS (2 marks)

**Time:** O(n × W) - two nested loops
**Space:** O(n × W) - DP table
**Space Optimized:** O(W) - use only previous row

**Comparison:**
- Brute Force: O(2ⁿ)
- DP: O(n×W) - much faster!
- Greedy: Wrong answer for 0/1 version

---

## Q13. (16 MARKS) KRUSKAL'S ALGORITHM WITH UNION-FIND

**COMPLETE ANSWER:**

### 1. MST CONCEPT (2 marks)

**Minimum Spanning Tree (MST):**
- Connects all vertices
- Uses V-1 edges (V = vertices)
- Minimum total edge weight
- No cycles (it's a tree!)

**Applications:** Network design, road/cable planning, cluster analysis

### 2. KRUSKAL'S ALGORITHM (4 marks)

**Greedy Strategy:** Select minimum weight edge that doesn't create cycle

**Algorithm Steps:**
```
KRUSKAL(G):
1. Sort all edges by weight (ascending)
2. Initialize Union-Find structure
3. MST = empty set
4. For each edge (u,v) in sorted order:
      If Find(u) ≠ Find(v):  // Different components
          Add edge to MST
          Union(u, v)
5. Return MST
```

**Why Sorting?** To always select minimum weight edge that's safe

### 3. UNION-FIND DATA STRUCTURE (4 marks)

**Purpose:** Detect cycles efficiently

**Operations:**

**Find(x):** Find root/representative of set containing x
```
Find(x):
    if parent[x] ≠ x:
        parent[x] = Find(parent[x])  // Path compression
    return parent[x]
```

**Union(x, y):** Merge sets containing x and y
```
Union(x, y):
    rootX = Find(x)
    rootY = Find(y)
    if rank[rootX] < rank[rootY]:
        parent[rootX] = rootY
    else if rank[rootX] > rank[rootY]:
        parent[rootY] = rootX
    else:
        parent[rootY] = rootX
        rank[rootX]++
```

**Optimizations:**
- **Path Compression:** Make all nodes point directly to root
- **Union by Rank:** Attach smaller tree under larger

**How it Prevents Cycles:**
If Find(u) = Find(v), they're already in same component → adding edge creates cycle!

### 4. DETAILED EXAMPLE (4 marks)

**Graph:**
```
      2
  0-------1
  |\     /|
 6| \3  /8| 5
  |  \ /  |
  3   2   4
  |  / \  |
 9| /7  \4|
  |/     \|
  5-------6
      10
```

**Edges sorted by weight:**
```
(0-1): 2
(1-2): 3
(2-4): 4
(1-4): 5
(0-3): 6
(2-5): 7
(1-2): 8  [duplicate path]
(3-5): 9
(5-6): 10
```

**Execution:**

**Initial:** Each vertex is its own set
```
Sets: {0} {1} {2} {3} {4} {5} {6}
```

**Step 1:** Edge (0-1, w=2)
- Find(0)=0, Find(1)=1 → Different sets
- Add to MST, Union(0,1)
- Sets: {0,1} {2} {3} {4} {5} {6}

**Step 2:** Edge (1-2, w=3)
- Find(1)=0, Find(2)=2 → Different
- Add to MST, Union(1,2)
- Sets: {0,1,2} {3} {4} {5} {6}

**Step 3:** Edge (2-4, w=4)
- Find(2)=0, Find(4)=4 → Different
- Add to MST, Union(2,4)
- Sets: {0,1,2,4} {3} {5} {6}

**Step 4:** Edge (1-4, w=5)
- Find(1)=0, Find(4)=0 → **Same set!**
- SKIP (would create cycle)

**Step 5:** Edge (0-3, w=6)
- Find(0)=0, Find(3)=3 → Different
- Add to MST, Union(0,3)
- Sets: {0,1,2,3,4} {5} {6}

**Step 6:** Edge (2-5, w=7)
- Find(2)=0, Find(5)=5 → Different
- Add to MST, Union(2,5)
- Sets: {0,1,2,3,4,5} {6}

**Step 7:** Edge (5-6, w=10)
- Find(5)=0, Find(6)=6 → Different
- Add to MST, Union(5,6)
- Sets: {0,1,2,3,4,5,6}

**MST Complete!** (V-1 = 6 edges)

**MST Edges:**
```
(0-1): 2
(1-2): 3
(2-4): 4
(0-3): 6
(2-5): 7
(5-6): 10
Total Weight: 32
```

### 5. COMPLEXITY ANALYSIS (2 marks)

**Time Complexity:**
1. Sort edges: O(E log E)
2. Process E edges:
   - Each Find: O(α(V)) ≈ O(1) amortized
   - Each Union: O(α(V)) ≈ O(1)
   - Total: O(E)

**Overall: O(E log E)** (sorting dominates)

Since E ≤ V², O(E log E) = O(E log V)

**Space Complexity:** O(V) for Union-Find arrays

**Comparison with Prim's:**
- Kruskal: O(E log E), best for sparse graphs
- Prim: O(E log V), best for dense graphs

---

## Q14. (16 MARKS) KMP ALGORITHM

**COMPLETE ANSWER:**

### 1. PROBLEM & MOTIVATION (2 marks)

**Pattern Matching Problem:** Find pattern P in text T

**Naive Approach Issues:**
- Time: O(n×m) where n=text length, m=pattern length
- Backtracks in text after mismatch
- Redundant comparisons

**KMP Improvement:** Uses **Longest Prefix Suffix (LPS)** array to avoid backtracking in text → O(n+m)

### 2. LPS ARRAY CONSTRUCTION (5 marks)

**LPS Definition:**
LPS[i] = length of longest proper prefix of pattern[0..i] that is also a suffix

**Proper prefix:** Prefix that is not the entire string

**Algorithm:**
```
COMPUTE_LPS(pattern):
1. LPS[0] = 0  // Single char has no proper prefix
2. len = 0     // Length of previous longest prefix
3. i = 1
4. While i < m:
      if pattern[i] == pattern[len]:
          len++
          LPS[i] = len
          i++
      else:
          if len != 0:
              len = LPS[len-1]  // Try shorter prefix
          else:
              LPS[i] = 0
              i++
```

**Example: Pattern = "AABAACAABAA"**

```
Index:  0  1  2  3  4  5  6  7  8  9  10
Char:   A  A  B  A  A  C  A  A  B  A  A
LPS:    0  1  0  1  2  0  1  2  3  4  5
```

**Step-by-Step:**
- i=1: A=A → LPS[1]=1
- i=2: B≠A → LPS[2]=0
- i=3: A=A → LPS[3]=1
- i=4: A=A → LPS[4]=2
- i=5: C≠B → LPS[5]=0
- i=6: A=A → LPS[6]=1
- i=7: A=A → LPS[7]=2
- i=8: B=B → LPS[8]=3
- i=9: A=A → LPS[9]=4
- i=10: A=A → LPS[10]=5

**Why LPS helps:** When mismatch at position j, we know pattern[0..j-1] matched. LPS[j-1] tells us how many characters we can skip!

### 3. KMP SEARCH ALGORITHM (5 marks)

**Algorithm:**
```
KMP_SEARCH(text, pattern):
1. Compute LPS array for pattern
2. i = 0  // Index for text
3. j = 0  // Index for pattern
4. While i < n:
      if pattern[j] == text[i]:
          i++
          j++
      if j == m:
          print "Pattern found at", i-j
          j = LPS[j-1]  // Continue searching
      else if i < n and pattern[j] != text[i]:
          if j != 0:
              j = LPS[j-1]  // Don't match LPS chars
          else:
              i++
```

**Example:**
```
Text:    "AABAABAAA"
Pattern: "AABAACAABAA"... wait, pattern too long

Better example:
Text:    "AABAACAADAABAABA"
Pattern: "AABAABA"
LPS:     [0,1,0,1,2,3,4]
```

**Search Process:**
```
i=0,j=0: A=A ✓ → i=1,j=1
i=1,j=1: A=A ✓ → i=2,j=2
i=2,j=2: B=B ✓ → i=3,j=3
i=3,j=3: A=A ✓ → i=4,j=4
i=4,j=4: A=A ✓ → i=5,j=5
i=5,j=5: C≠B ✗ → j=LPS[4]=2, i stays 5
i=5,j=2: C≠B ✗ → j=LPS[1]=1, i stays 5
i=5,j=1: C≠A ✗ → j=LPS[0]=0, i stays 5
i=5,j=0: C≠A ✗ → j=0, i=6
i=6,j=0: A=A ✓ → i=7,j=1
i=7,j=1: A=A ✓ → i=8,j=2
i=8,j=2: D≠B ✗ → j=LPS[1]=1
i=8,j=1: D≠A ✗ → j=0, i=9
i=9,j=0: A=A ✓ → continue...
i=15,j=7: MATCH! Pattern found at position 9
```

**Key Point:** Text index i never decreases → No backtracking!

### 4. COMPLEXITY ANALYSIS (2 marks)

**LPS Construction:**
- Time: O(m) - each character processed once
- Space: O(m) - LPS array

**Search:**
- Time: O(n) - i moves forward n times
- While loop may run more than n times, but i increases each iteration
- j can decrease but overall bounded by n

**Total: O(n + m)**

**Comparison:**
- Naive: O(n×m)
- KMP: O(n+m)
- For n=1000, m=10: Naive≈10,000 vs KMP≈1,010 ops!

### 5. CODE OUTLINE (2 marks)

```cpp
void computeLPS(string pattern, int lps[]) {
    int len = 0, i = 1;
    lps[0] = 0;
    while (i < pattern.length()) {
        if (pattern[i] == pattern[len]) {
            lps[i++] = ++len;
        } else {
            if (len) len = lps[len-1];
            else lps[i++] = 0;
        }
    }
}

void KMP(string text, string pattern) {
    int m = pattern.length(), n = text.length();
    int lps[m];
    computeLPS(pattern, lps);
    
    int i = 0, j = 0;
    while (i < n) {
        if (pattern[j] == text[i]) { i++; j++; }
        if (j == m) {
            cout << "Found at " << i-j << endl;
            j = lps[j-1];
        } else if (i < n && pattern[j] != text[i]) {
            if (j) j = lps[j-1];
            else i++;
        }
    }
}
```

---

## Q10. (16 MARKS) FLOYD-WARSHALL ALGORITHM

**COMPLETE ANSWER:**

### 1. INTRODUCTION (2 marks)

**Floyd-Warshall Algorithm:** Finds shortest paths between **all pairs** of vertices in weighted graph.

**Features:**
- Works with negative weights (but no negative cycles)
- Dynamic programming approach
- Simple implementation
- Time: O(V³)

**vs Other Algorithms:**
- Dijkstra: Single source, O((V+E) log V), no negative weights
- Bellman-Ford: Single source, O(VE), handles negative
- Floyd-Warshall: All pairs, O(V³), handles negative

### 2. ALGORITHM PRINCIPLE (4 marks)

**Core Idea:** Try all vertices as intermediate points

**State:**
```
dist[i][j][k] = shortest distance from i to j 
                using only vertices {0,1,...,k} as intermediate
```

**Recurrence:**
```
dist[i][j][k] = min(
    dist[i][j][k-1],              // Don't use vertex k
    dist[i][k][k-1] + dist[k][j][k-1]  // Use vertex k
)
```

**Optimization:** Use 2D array (overwrite in-place)
```
For k = 0 to V-1:
    For i = 0 to V-1:
        For j = 0 to V-1:
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```

**Initialization:**
```
dist[i][j] = weight of edge (i,j) if exists
           = ∞ if no edge
           = 0 if i == j
```

### 3. DETAILED EXAMPLE (6 marks)

**Graph with 4 vertices:**
```
    1     -3      2
  (0)---------->(1)
   |    ↗     ↙   |
  7|   /     /  5 |2
   | 4/    1/     ↓
  (3)<---------(2)
```

**Edges:**
- 0→1: 1
- 0→3: 7
- 1→2: 2
- 2→3: 1
- 3→1: 4

**Initial Distance Matrix (k=-1):**
```
     0   1   2   3
0 [  0   1   ∞   7 ]
1 [  ∞   0   2   ∞ ]
2 [  ∞   ∞   0   1 ]
3 [  ∞   4   ∞   0 ]
```

**Iteration k=0 (use vertex 0 as intermediate):**

Check all pairs: Can path i→0→j be shorter?

Changes:
- dist[1][3] = min(∞, dist[1][0]+dist[0][3]) = min(∞, ∞+7) = ∞
- dist[3][1] = min(4, dist[3][0]+dist[0][1]) = min(4, ∞+1) = 4
- All others stay same (no path through 0 improves)

```
     0   1   2   3
0 [  0   1   ∞   7 ]
1 [  ∞   0   2   ∞ ]
2 [  ∞   ∞   0   1 ]
3 [  ∞   4   ∞   0 ]
```

**Iteration k=1 (use vertex 1):**

Changes:
- dist[0][2] = min(∞, 1+2) = 3 ✓
- dist[3][2] = min(∞, 4+2) = 6 ✓

```
     0   1   2   3
0 [  0   1   3   7 ]
1 [  ∞   0   2   ∞ ]
2 [  ∞   ∞   0   1 ]
3 [  ∞   4   6   0 ]
```

**Iteration k=2 (use vertex 2):**

Changes:
- dist[0][3] = min(7, 3+1) = 4 ✓
- dist[1][3] = min(∞, 2+1) = 3 ✓
- dist[3][3] = min(0, 6+1) = 0 (no change)

```
     0   1   2   3
0 [  0   1   3   4 ]
1 [  ∞   0   2   3 ]
2 [  ∞   ∞   0   1 ]
3 [  ∞   4   6   0 ]
```

**Iteration k=3 (use vertex 3):**

Changes:
- dist[0][1] = min(1, 4+4) = 1 (no change)
- dist[2][1] = min(∞, 1+4) = 5 ✓

```
     0   1   2   3
0 [  0   1   3   4 ]
1 [  ∞   0   2   3 ]
2 [  ∞   5   0   1 ]
3 [  ∞   4   6   0 ]
```

**FINAL: All-pairs shortest distances!**

**Example paths:**
- 0→2: 0→1→2 = 1+2 = 3
- 0→3: 0→1→2→3 = 1+2+1 = 4
- 2→1: 2→3→1 = 1+4 = 5

### 4. COMPLEXITY (2 marks)

**Time Complexity:**
- Three nested loops (k, i, j): O(V³)
- Each operation: O(1)
- **Total: O(V³)**

**Space Complexity:**
- Distance matrix: V×V = O(V²)
- Can store paths with additional matrix

**When to use:**
- Need all-pairs distances
- Dense graphs (many edges)
- Negative weights but no negative cycles

### 5. APPLICATIONS (2 marks)

1. **Network routing:** Find optimal paths between all routers
2. **Game theory:** Analyze all possible moves
3. **Social networks:** Find degrees of separation
4. **Transitive closure:** Reachability between all pairs

**Detecting negative cycles:**
```
After Floyd-Warshall:
If dist[i][i] < 0 for any i:
    Negative cycle exists!
```

---

## MORE ANSWERS CONTINUED...

*Due to length, I've provided detailed complete answers for the most important questions. Would you like me to continue with more Part B questions like Q4 (Rabin-Karp), Q5 (Rat in Maze), Q6 (Subset Sum), Q7 (LCS), Q11 (Algorithm Comparisons), Q12 (Time Complexity), etc.?*

**For your exam tomorrow, focus on:**
1. Understanding these detailed examples
2. Being able to draw diagrams
3. Writing recurrence relations clearly
4. Showing step-by-step execution
5. Analyzing complexity

Good luck! 🎯📚
