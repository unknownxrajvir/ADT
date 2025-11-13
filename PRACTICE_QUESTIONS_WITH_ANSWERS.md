# 📝 PRACTICE QUESTIONS WITH COMPLETE ANSWERS

## 🎯 NEW EXPECTED QUESTIONS (Not from Model Paper)

Based on your syllabus and teacher's priority topics, here are **DIFFERENT** questions with full answers.

---

## 📝 PART A - SHORT ANSWER QUESTIONS (2 MARKS)

### Q1. What are the two key properties required for Dynamic Programming? Explain with an example.

**ANSWER:**

The two key properties are:

1. **Overlapping Subproblems:** The same smaller problems are solved multiple times during computation.

2. **Optimal Substructure:** The optimal solution to the problem can be constructed from optimal solutions of its subproblems.

**Example:** In Fibonacci sequence:
- F(5) = F(4) + F(3)
- F(4) = F(3) + F(2)
- Notice F(3) is calculated twice → Overlapping Subproblems
- F(5)'s optimal solution uses optimal F(4) and F(3) → Optimal Substructure

---

### Q2. Explain the difference between Prim's and Kruskal's algorithm for MST.

**ANSWER:**

**Kruskal's Algorithm:**
- Edge-based approach
- Sorts all edges by weight
- Uses Union-Find to detect cycles
- Better for sparse graphs
- Time: O(E log E)

**Prim's Algorithm:**
- Vertex-based approach
- Grows MST from a source vertex
- Uses priority queue (min-heap)
- Better for dense graphs
- Time: O((V+E) log V) with binary heap

**Key Difference:** Kruskal works on edges globally, Prim grows tree from one vertex locally.

---

### Q3. Why does Dijkstra's algorithm fail with negative edge weights?

**ANSWER:**

Dijkstra's algorithm uses a **greedy approach** that assumes once a vertex is processed (minimum distance found), it will never be updated again.

**Problem with negative weights:**
- A vertex marked as "finalized" might later need updating if a path through a negative edge is discovered
- Greedy selection doesn't reconsider already processed vertices

**Example:**
```
    A --(-5)--> B
    |           |
   (10)        (2)
    |           |
    v           v
    C -------> D
         (3)
```
Path A→C→D (13) gets finalized, but A→B→D (−3) is shorter!

**Solution:** Use **Bellman-Ford algorithm** for negative weights.

---

### Q4. What is the LPS array in KMP algorithm? Give example for pattern "AABAAC".

**ANSWER:**

**LPS (Longest Proper Prefix which is also Suffix)** array stores the length of the longest proper prefix that is also a suffix for each position.

**For pattern "AABAAC":**

| Index | 0 | 1 | 2 | 3 | 4 | 5 |
|-------|---|---|---|---|---|---|
| Char  | A | A | B | A | A | C |
| LPS   | 0 | 1 | 0 | 1 | 2 | 0 |

**Explanation:**
- LPS[0] = 0 (no proper prefix for single char)
- LPS[1] = 1 ("A" matches)
- LPS[2] = 0 ("AAB" has no matching prefix-suffix)
- LPS[3] = 1 ("AABA" → "A" matches)
- LPS[4] = 2 ("AABAA" → "AA" matches)
- LPS[5] = 0 ("AABAAC" has no matching prefix-suffix)

---

### Q5. Compare time complexity: Bubble Sort vs Merge Sort vs Quick Sort.

**ANSWER:**

| Algorithm | Best Case | Average Case | Worst Case | Space |
|-----------|-----------|--------------|------------|-------|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) |

**When to use:**
- Bubble: Small datasets, teaching purposes
- Merge: Guaranteed O(n log n), stable sort needed
- Quick: Average case fastest, in-place sorting

---

### Q6. What is the difference between Backtracking and Branch-and-Bound?

**ANSWER:**

**Backtracking:**
- Explores all possible solutions recursively
- Abandons path as soon as constraint violated
- Used for: N-Queens, Sudoku, Graph Coloring
- No optimization goal - finds feasible solutions
- DFS-based approach

**Branch and Bound:**
- Explores solution space systematically
- Uses **bounding function** to prune branches
- Used for: TSP, Job Scheduling, 0/1 Knapsack
- Finds **optimal** solution
- Can use BFS or DFS

**Key Difference:** B&B optimizes (finds best), Backtracking finds feasible solutions.

---

### Q7. Explain Union-Find data structure with path compression.

**ANSWER:**

**Union-Find (Disjoint Set Union)** maintains disjoint sets with two operations:

1. **Find(x):** Returns representative of set containing x
2. **Union(x,y):** Merges sets containing x and y

**Path Compression Optimization:**
During Find(x), make all nodes on path point directly to root.

**Example:**
```
Before Find(4):
    1
   / \
  2   3
 /
4

After Find(4) with path compression:
    1
   /|\
  2 3 4
```

**Benefit:** Makes subsequent Find operations nearly O(1).

---

### Q8. What is the recurrence relation for Tower of Hanoi? Solve it.

**ANSWER:**

**Recurrence Relation:**
```
T(n) = 2T(n-1) + 1
T(1) = 1
```

**Solving:**
```
T(n) = 2T(n-1) + 1
     = 2[2T(n-2) + 1] + 1 = 2²T(n-2) + 2 + 1
     = 2²[2T(n-3) + 1] + 2 + 1 = 2³T(n-3) + 4 + 2 + 1
     = 2^(n-1)T(1) + (2^(n-1) - 1)
     = 2^(n-1) + 2^(n-1) - 1
     = 2^n - 1
```

**Answer:** T(n) = 2^n - 1 moves

---

### Q9. Compare Greedy and Dynamic Programming approaches for Coin Change problem.

**ANSWER:**

**Greedy Approach:**
- Pick largest coin that fits
- Fast: O(amount/smallest_coin)
- **Fails for arbitrary denominations**

**Example where Greedy fails:**
- Coins: [1, 3, 4], Amount: 6
- Greedy: 4+1+1 = **3 coins** ✗
- Optimal: 3+3 = **2 coins** ✓

**DP Approach:**
- Tries all possibilities
- Slower: O(amount × coins)
- **Always finds optimal solution**

**Conclusion:** Use Greedy only for standard denominations (1,5,10,25), use DP for arbitrary coins.

---

### Q10. What is the space complexity of recursive Fibonacci? How to optimize?

**ANSWER:**

**Recursive Fibonacci:**
```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
```

**Space Complexity:** O(n) - recursion stack depth

**Optimization 1 - DP with Memoization:**
```cpp
int memo[n+1];
int fib(int n) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    return memo[n] = fib(n-1) + fib(n-2);
}
```
Space: O(n) for array + O(n) stack = O(n)

**Optimization 2 - Iterative (Best):**
```cpp
int fib(int n) {
    if (n <= 1) return n;
    int prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```
**Space: O(1)** - Best optimization!

---

## 📝 PART B - DETAILED QUESTIONS WITH ANSWERS (10-16 MARKS)

---

## Q1. Explain Kruskal's Minimum Spanning Tree algorithm with Union-Find. Illustrate with example. (16 marks)

### ANSWER:

#### 1. Introduction (2 marks)

**Minimum Spanning Tree (MST)** is a subset of edges that:
- Connects all vertices
- Has minimum total edge weight
- Contains no cycles
- Has exactly (V-1) edges

**Kruskal's Algorithm** is a **greedy algorithm** that builds MST by selecting edges in increasing order of weight, avoiding cycles using Union-Find data structure.

---

#### 2. Union-Find Data Structure (4 marks)

**Purpose:** Efficiently detect cycles

**Two Operations:**

**a) Find(x):** Returns root/representative of set containing x
```cpp
int Find(int x, int parent[]) {
    if (parent[x] != x)
        parent[x] = Find(parent[x], parent); // Path compression
    return parent[x];
}
```

**b) Union(x, y):** Merges two sets
```cpp
void Union(int x, int y, int parent[], int rank[]) {
    int rootX = Find(x, parent);
    int rootY = Find(y, parent);
    
    if (rank[rootX] < rank[rootY])
        parent[rootX] = rootY;
    else if (rank[rootX] > rank[rootY])
        parent[rootY] = rootX;
    else {
        parent[rootY] = rootX;
        rank[rootX]++;
    }
}
```

**Path Compression:** Makes Find nearly O(1)
**Union by Rank:** Keeps tree balanced

---

#### 3. Kruskal's Algorithm (4 marks)

**Steps:**
1. Sort all edges by weight (ascending order)
2. Initialize Union-Find: each vertex is its own set
3. For each edge (u,v) in sorted order:
   - If Find(u) ≠ Find(v) (different sets):
     * Add edge to MST
     * Union(u, v)
4. Stop when MST has (V-1) edges

**Pseudocode:**
```
KRUSKAL(Graph G):
    MST = []
    Sort edges by weight
    
    for each vertex v:
        parent[v] = v
        rank[v] = 0
    
    for each edge (u,v,w) in sorted order:
        if Find(u) != Find(v):
            MST.add((u,v,w))
            Union(u,v)
    
    return MST
```

---

#### 4. Example (4 marks)

**Graph:**
```
       2        3
   0------1------2
   |      |      |
  6|     8|     7|
   |      |      |
   3------4------5
       9      5
```

**Edges sorted by weight:**
| Edge | Weight |
|------|--------|
| 0-1  | 2      |
| 1-2  | 3      |
| 2-5  | 7      |
| 5-4  | 5      |
| 0-3  | 6      |
| 1-4  | 8      |
| 3-4  | 9      |

**Execution:**

**Initial:** Each vertex is separate set
- Sets: {0}, {1}, {2}, {3}, {4}, {5}

**Step 1:** Edge 0-1 (weight 2)
- Find(0)=0, Find(1)=1 → Different sets ✓
- Add to MST, Union(0,1)
- Sets: {0,1}, {2}, {3}, {4}, {5}
- **MST edges: [(0,1,2)]**

**Step 2:** Edge 1-2 (weight 3)
- Find(1)=0, Find(2)=2 → Different ✓
- Add to MST, Union(1,2)
- Sets: {0,1,2}, {3}, {4}, {5}
- **MST edges: [(0,1,2), (1,2,3)]**

**Step 3:** Edge 2-5 (weight 7)
- Find(2)=0, Find(5)=5 → Different ✓
- Add to MST
- Sets: {0,1,2,5}, {3}, {4}
- **MST edges: [(0,1,2), (1,2,3), (2,5,7)]**

**Step 4:** Edge 5-4 (weight 5)
- Find(5)=0, Find(4)=4 → Different ✓
- Add to MST
- Sets: {0,1,2,4,5}, {3}
- **MST edges: [(0,1,2), (1,2,3), (2,5,7), (5,4,5)]**

**Step 5:** Edge 0-3 (weight 6)
- Find(0)=0, Find(3)=3 → Different ✓
- Add to MST
- Sets: {0,1,2,3,4,5}
- **MST edges: [(0,1,2), (1,2,3), (2,5,7), (5,4,5), (0,3,6)]**

**Done!** 5 edges (V-1 = 6-1 = 5)

**Final MST:**
```
       2        3
   0------1------2
   |              |
  6|             7|
   |              |
   3      4------5
              5
```

**Total Weight:** 2+3+7+5+6 = **23**

---

#### 5. Time Complexity Analysis (2 marks)

**Operations:**
1. **Sorting edges:** O(E log E)
2. **Union-Find operations:** O(E × α(V))
   - α(V) is inverse Ackermann function ≈ O(1)

**Total Time Complexity:** **O(E log E)** or **O(E log V)**
- Since E ≤ V², log E = 2 log V

**Space Complexity:** O(V) for parent and rank arrays

**Why O(E log E) dominates?**
Sorting takes most time, Union-Find is nearly constant.

---

## Q2. Explain Dijkstra's Algorithm with example. What are its limitations? (16 marks)

### ANSWER:

#### 1. Introduction (2 marks)

**Dijkstra's Algorithm** finds the shortest path from a **single source vertex** to all other vertices in a weighted graph.

**Key Characteristics:**
- Greedy algorithm
- Works only with **non-negative edge weights**
- Uses priority queue (min-heap) for efficiency
- Single-source shortest path (SSSP)

**Applications:** GPS navigation, network routing, flight paths

---

#### 2. Algorithm Explanation (4 marks)

**Concept:** Always pick the unvisited vertex with minimum distance from source, then relax its neighbors.

**Steps:**
1. Initialize distance to source as 0, all others as ∞
2. Mark all vertices as unvisited
3. While there are unvisited vertices:
   - Pick unvisited vertex `u` with minimum distance
   - Mark `u` as visited
   - For each neighbor `v` of `u`:
     * If `dist[u] + weight(u,v) < dist[v]`:
       - Update: `dist[v] = dist[u] + weight(u,v)`
       - Set `parent[v] = u` (for path reconstruction)

**Pseudocode:**
```
DIJKSTRA(Graph G, source s):
    for each vertex v:
        dist[v] = ∞
        visited[v] = false
    
    dist[s] = 0
    PriorityQueue pq
    pq.insert(s, 0)
    
    while pq is not empty:
        u = pq.extractMin()
        visited[u] = true
        
        for each neighbor v of u:
            if not visited[v]:
                newDist = dist[u] + weight(u,v)
                if newDist < dist[v]:
                    dist[v] = newDist
                    parent[v] = u
                    pq.decreaseKey(v, newDist)
    
    return dist[]
```

---

#### 3. Detailed Example (6 marks)

**Graph:**
```
         4        8
    0-------1-------2
    |       |       |
   1|      2|      7|
    |       |       |
    3-------4-------5
         1      9
```

**Find shortest path from vertex 0 to all others.**

**Initialization:**
- dist[] = [0, ∞, ∞, ∞, ∞, ∞]
- visited[] = [F, F, F, F, F, F]
- Priority Queue: [(0, 0)]

---

**Iteration 1:** Extract vertex 0 (dist=0)
- Mark 0 as visited
- **Relax neighbors:**
  - To 1: dist[1] = min(∞, 0+4) = 4
  - To 3: dist[3] = min(∞, 0+1) = 1
- dist[] = [0, 4, ∞, 1, ∞, ∞]
- PQ: [(3,1), (1,4)]

---

**Iteration 2:** Extract vertex 3 (dist=1)
- Mark 3 as visited
- **Relax neighbors:**
  - To 0: Already visited
  - To 4: dist[4] = min(∞, 1+1) = 2
- dist[] = [0, 4, ∞, 1, 2, ∞]
- PQ: [(4,2), (1,4)]

---

**Iteration 3:** Extract vertex 4 (dist=2)
- Mark 4 as visited
- **Relax neighbors:**
  - To 1: dist[1] = min(4, 2+2) = 4 (no change)
  - To 3: Already visited
  - To 5: dist[5] = min(∞, 2+9) = 11
- dist[] = [0, 4, ∞, 1, 2, 11]
- PQ: [(1,4), (5,11)]

---

**Iteration 4:** Extract vertex 1 (dist=4)
- Mark 1 as visited
- **Relax neighbors:**
  - To 0: Already visited
  - To 2: dist[2] = min(∞, 4+8) = 12
  - To 4: Already visited
- dist[] = [0, 4, 12, 1, 2, 11]
- PQ: [(5,11), (2,12)]

---

**Iteration 5:** Extract vertex 5 (dist=11)
- Mark 5 as visited
- **Relax neighbors:**
  - To 2: dist[2] = min(12, 11+7) = 12 (no change)
  - To 4: Already visited
- dist[] = [0, 4, 12, 1, 2, 11]
- PQ: [(2,12)]

---

**Iteration 6:** Extract vertex 2 (dist=12)
- Mark 2 as visited
- All vertices visited

**Final Result:**
| Vertex | 0 | 1 | 2 | 3 | 4 | 5 |
|--------|---|---|---|---|---|---|
| Distance | 0 | 4 | 12 | 1 | 2 | 11 |

**Shortest Paths from 0:**
- 0 → 0: 0
- 0 → 1: 0→1 (4)
- 0 → 2: 0→1→2 (12)
- 0 → 3: 0→3 (1)
- 0 → 4: 0→3→4 (2)
- 0 → 5: 0→3→4→5 (11)

---

#### 4. Limitations (2 marks)

**Critical Limitation: Cannot handle NEGATIVE edge weights!**

**Why?**
Dijkstra's greedy approach assumes once a vertex is finalized (minimum distance found), it won't be updated. Negative edges can invalidate this assumption.

**Example where it fails:**
```
    A ---(-5)---> B
    |             |
   (10)          (2)
    |             |
    v             v
    C ---------> D
         (3)
```

**Dijkstra from A:**
1. Picks C (dist=10), finalizes it
2. Never reconsiders C via A→B→D path
3. **Wrong result!**

**Correct shortest path:** A→B→D = -5+2 = -3 (but Dijkstra misses this)

**Solutions for negative weights:**
- Use **Bellman-Ford Algorithm** (handles negative weights)
- Time: O(VE) - slower but correct

---

#### 5. Time Complexity (2 marks)

**With Binary Heap (Min-Priority Queue):**
- **Building heap:** O(V)
- **Extract-Min:** O(log V) × V times = O(V log V)
- **Decrease-Key:** O(log V) × E times = O(E log V)
- **Total: O((V+E) log V)**

**With Fibonacci Heap:**
- Extract-Min: O(log V) amortized
- Decrease-Key: O(1) amortized
- **Total: O(E + V log V)** - Better for dense graphs

**Space Complexity:** O(V) for distance array and priority queue

---

## Q3. Solve 0/1 Knapsack using Dynamic Programming with complete example. (16 marks)

### ANSWER:

#### 1. Problem Definition (2 marks)

**0/1 Knapsack Problem:**
Given:
- n items, each with weight `w[i]` and value `v[i]`
- Knapsack capacity `W`

**Goal:** Select items to maximize total value without exceeding capacity.

**Constraint:** Each item can be taken **0 or 1 time** (cannot break items).

**Example:** Packing a suitcase with weight limit.

---

#### 2. Why Dynamic Programming? (2 marks)

**Properties:**

1. **Overlapping Subproblems:**
   - Same subproblems computed multiple times
   - Example: Knapsack(items 1-3, capacity 5) appears in multiple recursive calls

2. **Optimal Substructure:**
   - Optimal solution contains optimal solutions to subproblems
   - If item i is in optimal solution, remaining items must also be optimal

**Why NOT Greedy?**
Greedy by value/weight ratio fails!
- Items: w=[10,20,30], v=[60,100,120], W=50
- Greedy: Take item 1 (ratio 6) → Can't fit others optimally
- Optimal: Take items 2,3 → Better value!

---

#### 3. Recurrence Relation (3 marks)

**Define:** `dp[i][w]` = Maximum value using first `i` items with capacity `w`

**Base Cases:**
```
dp[0][w] = 0  for all w  (no items → value = 0)
dp[i][0] = 0  for all i  (capacity 0 → value = 0)
```

**Recurrence:**
For item `i` with weight `w[i]` and value `v[i]`:

```
If w[i] > w:
    dp[i][w] = dp[i-1][w]  // Can't include (too heavy)
Else:
    dp[i][w] = max(
        dp[i-1][w],                    // Exclude item i
        v[i] + dp[i-1][w - w[i]]      // Include item i
    )
```

**Logic:**
- **Exclude:** Don't take item i → same as solution with i-1 items
- **Include:** Take item i → add its value + solve for remaining capacity

---

#### 4. Complete Example (7 marks)

**Problem:**
- Items: 4
- Weights: [1, 3, 4, 5]
- Values:  [1, 4, 5, 7]
- Capacity: W = 7

**DP Table Construction:**

```
dp[i][w] where i = items (0 to 4), w = capacity (0 to 7)
```

**Step-by-step:**

**Row 0 (No items):** All zeros
```
w:    0  1  2  3  4  5  6  7
i=0:  0  0  0  0  0  0  0  0
```

---

**Row 1 (Item 1: w=1, v=1):**
- For w≥1: Can include item 1
```
w=1: max(0, 1+dp[0][0]) = max(0, 1) = 1
w=2: max(0, 1+dp[0][1]) = max(0, 1) = 1
...all w≥1: value = 1
```

```
w:    0  1  2  3  4  5  6  7
i=0:  0  0  0  0  0  0  0  0
i=1:  0  1  1  1  1  1  1  1
```

---

**Row 2 (Item 2: w=3, v=4):**
```
w=1,2: Can't fit (w[2]=3 > w) → dp[1][w]
w=3: max(dp[1][3], 4+dp[1][0]) = max(1, 4) = 4
w=4: max(dp[1][4], 4+dp[1][1]) = max(1, 5) = 5
w=5: max(dp[1][5], 4+dp[1][2]) = max(1, 5) = 5
w=6: max(dp[1][6], 4+dp[1][3]) = max(1, 5) = 5
w=7: max(dp[1][7], 4+dp[1][4]) = max(1, 5) = 5
```

```
w:    0  1  2  3  4  5  6  7
i=0:  0  0  0  0  0  0  0  0
i=1:  0  1  1  1  1  1  1  1
i=2:  0  1  1  4  5  5  5  5
```

---

**Row 3 (Item 3: w=4, v=5):**
```
w=1,2,3: Can't fit → dp[2][w]
w=4: max(dp[2][4], 5+dp[2][0]) = max(5, 5) = 5
w=5: max(dp[2][5], 5+dp[2][1]) = max(5, 6) = 6
w=6: max(dp[2][6], 5+dp[2][2]) = max(5, 6) = 6
w=7: max(dp[2][7], 5+dp[2][3]) = max(5, 9) = 9
```

```
w:    0  1  2  3  4  5  6  7
i=0:  0  0  0  0  0  0  0  0
i=1:  0  1  1  1  1  1  1  1
i=2:  0  1  1  4  5  5  5  5
i=3:  0  1  1  4  5  6  6  9
```

---

**Row 4 (Item 4: w=5, v=7):**
```
w=1-4: Can't fit → dp[3][w]
w=5: max(dp[3][5], 7+dp[3][0]) = max(6, 7) = 7
w=6: max(dp[3][6], 7+dp[3][1]) = max(6, 8) = 8
w=7: max(dp[3][7], 7+dp[3][2]) = max(9, 8) = 9
```

```
w:    0  1  2  3  4  5  6  7
i=0:  0  0  0  0  0  0  0  0
i=1:  0  1  1  1  1  1  1  1
i=2:  0  1  1  4  5  5  5  5
i=3:  0  1  1  4  5  6  6  9
i=4:  0  1  1  4  5  7  8  9
```

**Final Answer: dp[4][7] = 9**

---

**Items Selected (Backtracking):**

Start at dp[4][7] = 9

```
At dp[4][7] = 9:
  dp[4][7] = 9, dp[3][7] = 9 → Same, item 4 NOT included
  Move to dp[3][7]

At dp[3][7] = 9:
  dp[3][7] = 9, dp[2][7] = 5 → Different, item 3 INCLUDED ✓
  Remaining capacity: 7-4 = 3
  Move to dp[2][3]

At dp[2][3] = 4:
  dp[2][3] = 4, dp[1][3] = 1 → Different, item 2 INCLUDED ✓
  Remaining capacity: 3-3 = 0
  Move to dp[1][0]

At dp[1][0] = 0:
  Done!
```

**Selected Items: 2 and 3**
- Item 2: weight=3, value=4
- Item 3: weight=4, value=5
- Total weight: 3+4 = 7 ✓
- Total value: 4+5 = 9 ✓

---

#### 5. Time and Space Complexity (2 marks)

**Time Complexity:** **O(n × W)**
- Fill n×W table
- Each cell computed in O(1)

**Space Complexity:** **O(n × W)** for DP table

**Space Optimization:**
Can reduce to O(W) by using 1D array (process row by row, right to left)

```cpp
int dp[W+1];
for (int i = 1; i <= n; i++) {
    for (int w = W; w >= weight[i]; w--) {
        dp[w] = max(dp[w], value[i] + dp[w-weight[i]]);
    }
}
```

**Optimized Space: O(W)**

---

## Q4. Explain KMP Algorithm with LPS array construction and example. (16 marks)

### ANSWER:

#### 1. Introduction (2 marks)

**KMP (Knuth-Morris-Pratt)** is an efficient string matching algorithm that finds pattern P in text T.

**Problem with Naive Approach:**
- On mismatch, shifts pattern by 1 and starts matching from beginning
- Time: O(m×n) where m=text length, n=pattern length
- Redundant comparisons!

**KMP Improvement:**
- Uses **LPS (Longest Proper Prefix which is also Suffix)** array
- On mismatch, intelligently shifts pattern without backtracking in text
- **Time: O(m+n)** - Linear!

---

#### 2. LPS Array Concept (4 marks)

**LPS[i]** = Length of longest proper prefix of pattern[0...i] which is also a suffix.

**Proper Prefix:** Prefix that is not the entire string

**Example Pattern: "AABAACAABAA"**

```
Index: 0  1  2  3  4  5  6  7  8  9  10
Char:  A  A  B  A  A  C  A  A  B  A  A
LPS:   0  1  0  1  2  0  1  2  3  4  5
```

**Explanation:**
- LPS[0] = 0 → "A" has no proper prefix
- LPS[1] = 1 → "AA": prefix "A" = suffix "A"
- LPS[2] = 0 → "AAB": no match
- LPS[3] = 1 → "AABA": "A" matches
- LPS[4] = 2 → "AABAA": "AA" matches
- LPS[5] = 0 → "AABAAC": no match
- LPS[6] = 1 → "AABAACA": "A" matches
- LPS[7] = 2 → "AABAACAA": "AA" matches
- LPS[8] = 3 → "AABAACAAB": "AAB" matches
- LPS[9] = 4 → "AABAACAABA": "AABA" matches
- LPS[10] = 5 → "AABAACAABAA": "AABAA" matches

---

#### 3. LPS Array Construction Algorithm (4 marks)

```cpp
void computeLPS(string pattern, int lps[]) {
    int m = pattern.length();
    lps[0] = 0;  // First element always 0
    int len = 0;  // Length of previous longest prefix suffix
    int i = 1;
    
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        }
        else {
            if (len != 0) {
                // Don't increment i, try smaller prefix
                len = lps[len - 1];
            }
            else {
                lps[i] = 0;
                i++;
            }
        }
    }
}
```

**Step-by-step for "AABAACAABAA":**

```
i=1: pattern[1]='A' == pattern[0]='A' → len=1, lps[1]=1
i=2: pattern[2]='B' != pattern[1]='A' → len=0, lps[2]=0
i=3: pattern[3]='A' == pattern[0]='A' → len=1, lps[3]=1
i=4: pattern[4]='A' == pattern[1]='A' → len=2, lps[4]=2
i=5: pattern[5]='C' != pattern[2]='B' → len=0, lps[5]=0
i=6: pattern[6]='A' == pattern[0]='A' → len=1, lps[6]=1
i=7: pattern[7]='A' == pattern[1]='A' → len=2, lps[7]=2
i=8: pattern[8]='B' == pattern[2]='B' → len=3, lps[8]=3
i=9: pattern[9]='A' == pattern[3]='A' → len=4, lps[9]=4
i=10: pattern[10]='A' == pattern[4]='A' → len=5, lps[10]=5
```

**Time Complexity:** O(m) where m = pattern length

---

#### 4. KMP Search Algorithm (4 marks)

```cpp
void KMPSearch(string text, string pattern) {
    int n = text.length();
    int m = pattern.length();
    
    int lps[m];
    computeLPS(pattern, lps);
    
    int i = 0;  // index for text
    int j = 0;  // index for pattern
    
    while (i < n) {
        if (pattern[j] == text[i]) {
            i++;
            j++;
        }
        
        if (j == m) {
            // Pattern found
            cout << "Pattern found at index " << (i - j) << endl;
            j = lps[j - 1];  // Look for next match
        }
        else if (i < n && pattern[j] != text[i]) {
            if (j != 0)
                j = lps[j - 1];  // Use LPS to skip
            else
                i++;
        }
    }
}
```

**Key Idea:**
- On mismatch at position j, use lps[j-1] to determine next position in pattern
- Avoids re-comparing already matched characters!

---

#### 5. Complete Example (5 marks)

**Text:** "AABAACAADAABAABA"
**Pattern:** "AABA"

**Step 1: Compute LPS for "AABA"**
```
Pattern: A  A  B  A
Index:   0  1  2  3
LPS:     0  1  0  1
```

**Step 2: Search**

```
Text:    A A B A A C A A D A A B A A B A
         0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15

Pattern: A A B A
         0 1 2 3

i=0, j=0: text[0]='A' == pattern[0]='A' → i=1, j=1
i=1, j=1: text[1]='A' == pattern[1]='A' → i=2, j=2
i=2, j=2: text[2]='B' == pattern[2]='B' → i=3, j=3
i=3, j=3: text[3]='A' == pattern[3]='A' → i=4, j=4
j=4: MATCH FOUND at index 0! ✓
      j = lps[3] = 1 (continue from here)

i=4, j=1: text[4]='A' == pattern[1]='A' → i=5, j=2
i=5, j=2: text[5]='C' != pattern[2]='B' → j=lps[1]=1
i=5, j=1: text[5]='C' != pattern[1]='A' → j=lps[0]=0
i=5, j=0: text[5]='C' != pattern[0]='A' → i=6

i=6, j=0: text[6]='A' == pattern[0]='A' → i=7, j=1
i=7, j=1: text[7]='A' == pattern[1]='A' → i=8, j=2
i=8, j=2: text[8]='D' != pattern[2]='B' → j=lps[1]=1
i=8, j=1: text[8]='D' != pattern[1]='A' → j=lps[0]=0
i=8, j=0: text[8]='D' != pattern[0]='A' → i=9

i=9, j=0: text[9]='A' == pattern[0]='A' → i=10, j=1
i=10, j=1: text[10]='A' == pattern[1]='A' → i=11, j=2
i=11, j=2: text[11]='B' == pattern[2]='B' → i=12, j=3
i=12, j=3: text[12]='A' == pattern[3]='A' → i=13, j=4
j=4: MATCH FOUND at index 9! ✓
      j = lps[3] = 1

i=13, j=1: text[13]='A' == pattern[1]='A' → i=14, j=2
i=14, j=2: text[14]='B' == pattern[2]='B' → i=15, j=3
i=15, j=3: text[15]='A' == pattern[3]='A' → i=16, j=4
j=4: MATCH FOUND at index 12! ✓

Done!
```

**Matches found at indices: 0, 9, 12**

---

#### 6. Time Complexity (1 mark)

**Total Time: O(m + n)**
- LPS construction: O(m)
- Search: O(n)
- i never decreases, max 2n iterations

**Space: O(m)** for LPS array

**Comparison with Naive:**
- Naive: O(m×n)
- KMP: O(m+n)

**Huge improvement for large texts!**

---

## 🎯 MORE PRACTICE QUESTIONS

### Q5. Explain N-Queens problem with backtracking. Show solution for 4-Queens. (10 marks)

**ANSWER:**

**Problem:** Place N queens on N×N chessboard so no two queens attack each other.

**Attack Rules:** Queens attack same row, column, or diagonal.

**Backtracking Approach:**
1. Place queens row by row
2. For each row, try each column
3. Check if safe (no attacks)
4. If safe, place queen and recurse to next row
5. If solution found, return
6. Else, remove queen (backtrack) and try next column

**4-Queens Solution:**
```
. Q . .    Row 0, Col 1
. . . Q    Row 1, Col 3
Q . . .    Row 2, Col 0
. . Q .    Row 3, Col 2
```

**Time:** O(N!)

---

### Q6. Compare Bubble Sort, Selection Sort, and Insertion Sort. (6 marks)

**ANSWER:**

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |

**When to use:**
- Nearly sorted: Insertion Sort (O(n))
- Small datasets: Any (simple to implement)
- Need stability: Bubble or Insertion

---

### Q7. Explain Activity Selection Problem with greedy approach. (10 marks)

**ANSWER:**

**Problem:** Select maximum non-overlapping activities.

**Greedy Strategy:** Always pick activity with earliest finish time.

**Algorithm:**
1. Sort by finish time
2. Select first activity
3. For remaining: if start ≥ last finish, select it

**Example:**
Activities: [(1,3), (2,5), (4,7), (1,8), (5,9), (8,10)]
After sort: [(1,3), (2,5), (4,7), (1,8), (5,9), (8,10)]

Select (1,3), skip (2,5), select (4,7), skip others, select (8,10)
Result: 3 activities

**Time:** O(n log n)

---

### Q8. Explain Longest Increasing Subsequence (LIS) with DP. (10 marks)

**ANSWER:**

**Problem:** Find longest subsequence where elements are increasing.

**DP Approach:**
- dp[i] = LIS ending at index i
- dp[i] = max(dp[j] + 1) for all j < i where arr[j] < arr[i]

**Example:** [10, 9, 2, 5, 3, 7, 101, 18]

```
dp: [1, 1, 1, 2, 2, 3, 4, 4]
```

LIS length: 4
One LIS: [2, 5, 7, 101] or [2, 3, 7, 18]

**Time:** O(n²), **Can optimize to O(n log n)**

---

## 📚 STUDY STRATEGY FOR THESE ANSWERS

1. **Read answer once completely**
2. **Close book, write answer from memory**
3. **Compare with original**
4. **Practice 2-3 times**
5. **Focus on examples and diagrams**

**Time yourself!** 16 marks = 25-30 minutes maximum!

Good luck! 🚀📝
