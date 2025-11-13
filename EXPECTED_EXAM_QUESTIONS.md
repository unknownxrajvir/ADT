# 🎯 EXPECTED EXAM QUESTIONS - PRACTICE SET

## 📋 BASED ON YOUR MODEL QUESTION PAPER ANALYSIS

**Exam Pattern Observed:**
- **Part A:** 10 questions × 2 marks = 20 marks (Short answers)
- **Part B:** 16 questions (choose based on marks) = 80 marks (Detailed answers)
- **Total:** 100 marks, 3 hours
- **Course Outcomes (CO):** CO1-CO5 mapping present

---

## 📝 PART A - SHORT ANSWER QUESTIONS (2 MARKS EACH)

### Answer ANY 10 questions

**RECURSION & TIME COMPLEXITY**

**Q1.** Trace the execution of the following recursive function and predict its output:
```
int fun(int n) {
    if (n == 0) return 1;
    return n * fun(n-1);
}
```
What is fun(5)?

**ANSWER:**
This function calculates factorial recursively.

Execution trace:
- fun(5) = 5 × fun(4)
- fun(4) = 4 × fun(3)
- fun(3) = 3 × fun(2)
- fun(2) = 2 × fun(1)
- fun(1) = 1 × fun(0)
- fun(0) = 1 (base case)

Backtracking:
- fun(1) = 1 × 1 = 1
- fun(2) = 2 × 1 = 2
- fun(3) = 3 × 2 = 6
- fun(4) = 4 × 6 = 24
- fun(5) = 5 × 24 = **120**

**Output: 120** (This is 5! = factorial of 5)

---

**Q2.** List the sequence of numbers generated in the Sieve of Eratosthenes for N = 30.

**ANSWER:**
Sieve of Eratosthenes finds all prime numbers up to N by eliminating multiples.

**Process:**
1. List: 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30
2. Mark 2, cross multiples: ~~4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30~~
3. Mark 3, cross multiples: ~~9, 15, 21, 27~~ (others already crossed)
4. Mark 5, cross multiples: ~~25~~ (others already crossed)
5. 7² = 49 > 30, so stop

**Prime numbers ≤ 30:**
**2, 3, 5, 7, 11, 13, 17, 19, 23, 29** (Total: 10 primes)

---

**Q3.** Identify and explain why the Greedy approach fails for the 0/1 Knapsack problem.

**ANSWER:**
Greedy approach (selecting items by highest value/weight ratio) fails because it makes local optimal choices without considering future consequences.

**Counterexample:**
- Capacity = 10 kg
- Item 1: weight = 5 kg, value = ₹50 (ratio = 10)
- Item 2: weight = 6 kg, value = ₹60 (ratio = 10)
- Item 3: weight = 4 kg, value = ₹40 (ratio = 10)

**Greedy (by ratio):** Takes Item 1 (5kg), then Item 3 (4kg) = **₹90**
**Optimal:** Take Item 2 (6kg) + Item 3 (4kg) = **₹100** ✓

**Conclusion:** Greedy fails because you cannot take fractions. Must use **Dynamic Programming** to explore all combinations for optimal solution.

---

**Q4.** Explain the working of the Knuth-Morris-Pratt (KMP) algorithm and state how it improves efficiency over the Naive pattern matching approach.

**ANSWER:**
**KMP Algorithm:** Uses a Longest Prefix Suffix (LPS) array to avoid re-checking already matched characters in the text.

**Working:**
1. Preprocess pattern to build LPS array (stores length of longest proper prefix that is also suffix)
2. During search, when mismatch occurs, use LPS to skip unnecessary comparisons instead of starting over

**Improvement over Naive:**
- **Naive:** O(n×m) - backtracks in text on mismatch
- **KMP:** O(n+m) - never backtracks in text, uses LPS to jump smartly

**Example:** Pattern = "AAAB", Text = "AAAA..."
- Naive: After 3 matches, mismatch at B, start over from position 1
- KMP: Use LPS to know first 2 A's already matched, continue from there

**Efficiency Gain:** No redundant comparisons!

---

**Q5.** Explain how recursion and backtracking differ in terms of problem-solving strategy.

**ANSWER:**

**Recursion:**
- Problem-solving technique that breaks problem into smaller subproblems
- Function calls itself with reduced input until base case reached
- Builds solution by combining results from subproblems
- Example: Factorial, Fibonacci

**Backtracking:**
- Uses recursion but adds "trial and error" approach
- Tries a solution, if it fails, **undoes** (backtracks) and tries another path
- Explores all possibilities systematically with pruning
- Example: N-Queens, Sudoku

**Key Difference:** Recursion is a general technique; Backtracking is recursion + decision making + undoing wrong choices when exploring solution space.

---

**Q6.** Identify the role of "safe position checking" in solving the N-Queens problem using backtracking.

**ANSWER:**
**Safe Position Checking** ensures no two queens attack each other before placing a queen.

**Checks Required:**
1. **Same Column:** No other queen in same column
2. **Main Diagonal:** No queen on same \ diagonal (row-col constant)
3. **Anti-Diagonal:** No queen on same / diagonal (row+col constant)
(Row check not needed as we place one queen per row)

**Purpose:**
- Prunes invalid branches early (doesn't waste time exploring dead ends)
- Ensures constraint satisfaction before making choice
- Critical for backtracking efficiency

**Example:** When placing queen at position (2,3), check if any existing queen at (0,1), (1,2), (0,3), (1,4), etc. threatens it.

---

**Q7.** Define overlapping subproblems and illustrate one example where this property appears in dynamic programming.

**ANSWER:**
**Overlapping Subproblems:** When a recursive algorithm solves the same subproblem multiple times.

**Example - Fibonacci:**
```
Computing F(5):
           F(5)
         /      \
      F(4)      F(3)  ← computed once
     /   \      /   \
  F(3)  F(2) F(2) F(1)
  /  \   ↑     ↑
F(2) F(1) Already computed!
```

**Problem:** F(3) computed twice, F(2) computed thrice - wasteful!

**DP Solution:** Store results in array/table
- First time F(3) computed → store result
- Next time needed → retrieve stored value (no recomputation)

This reduces exponential O(2ⁿ) to linear O(n) time!

---

**Q8.** Differentiate between memoization and tabulation with a suitable example.

**ANSWER:**

| Aspect | Memoization | Tabulation |
|--------|-------------|------------|
| **Approach** | Top-down | Bottom-up |
| **Method** | Recursive + caching | Iterative + table filling |
| **Storage** | Hash map/array | Array/table |
| **Computation** | On-demand (lazy) | All subproblems computed |

**Example - Fibonacci:**

**Memoization:**
```
memo[n] = -1 (initially)
fib(n):
    if n ≤ 1: return n
    if memo[n] != -1: return memo[n]
    memo[n] = fib(n-1) + fib(n-2)
    return memo[n]
```

**Tabulation:**
```
dp[0]=0, dp[1]=1
for i=2 to n:
    dp[i] = dp[i-1] + dp[i-2]
return dp[n]
```

**Memoization:** Solves F(5) → calls F(4), F(3)... (recursive)
**Tabulation:** Fills dp[0], dp[1], dp[2]... dp[5] (iterative)

---

**Q9.** Explain how the relaxation step works in Dijkstra's shortest path algorithm.

**ANSWER:**
**Relaxation** is the process of updating shortest distance to a vertex if a shorter path is found through current vertex.

**Formula:**
```
For edge u → v with weight w:
if dist[u] + w < dist[v]:
    dist[v] = dist[u] + w
    parent[v] = u
```

**Intuition:** "Can I reach v cheaper by going through u?"

**Example:**
```
Current distances: dist[A]=0, dist[B]=5, dist[C]=∞
Edge: B → C with weight 2

Relaxation:
dist[B] + 2 = 5 + 2 = 7 < ∞
Update: dist[C] = 7
```

This ensures we always maintain shortest known distance to each vertex as we explore the graph.

---

**Q10.** Describe the concept of a Segment Tree and explain how it supports range sum queries efficiently.

**ANSWER:**
**Segment Tree:** A binary tree data structure for range queries where each node represents an interval/segment.

**Structure:**
- Leaf nodes: Individual array elements
- Internal nodes: Aggregate (sum/min/max) of child ranges
- Root: Represents entire array

**Range Sum Query:**
```
Example: arr = [1, 3, 5, 7, 9, 11]
Query: sum(2, 5) = 5+7+9+11 = 32

Tree structure splits query into O(log n) segments
Navigate tree: O(log n) time
```

**Efficiency:**
- **Build:** O(n)
- **Query:** O(log n) - only visit relevant segments
- **Update:** O(log n) - update path from leaf to root

**Advantage:** Much faster than naive O(n) iteration for multiple queries!

---

**Q11.** What is the time complexity of Kruskal's algorithm and why?

**ANSWER:**
**Time Complexity: O(E log E)** where E = number of edges

**Breakdown:**
1. **Sort edges by weight:** O(E log E) ← **Dominates!**
2. **Process each edge:**
   - Union-Find operations: O(α(V)) ≈ O(1) amortized
   - Total for E edges: O(E × α(V)) ≈ O(E)

**Why O(E log E)?**
- Sorting step takes longest time
- Union-Find with path compression and rank is nearly constant per operation
- Since E can be at most V², O(E log E) = O(E log V²) = O(2E log V) = O(E log V)

**In practice:** Can write as **O(E log E)** or **O(E log V)** (both equivalent)

---

**Q12.** Explain the difference between Prim's and Kruskal's algorithm for finding MST.

**ANSWER:**

| Aspect | Kruskal's | Prim's |
|--------|-----------|--------|
| **Approach** | Edge-based | Vertex-based |
| **Strategy** | Select minimum edge that doesn't form cycle | Grow tree from source vertex |
| **Data Structure** | Union-Find (Disjoint Set) | Priority Queue (Min-Heap) |
| **Initial Step** | Sort all edges | Start from any vertex |
| **Process** | Add edges globally | Add edges from current tree |
| **Complexity** | O(E log E) | O(E log V) |
| **Best For** | Sparse graphs (fewer edges) | Dense graphs (more edges) |
| **Graph Type** | Works on disconnected components | Requires connected graph |

**Example:** For 4 vertices:
- **Kruskal:** Considers ALL edges, picks globally minimum
- **Prim:** Grows from vertex 0 → adds neighbors → expands tree

---

**Q13.** What is the limitation of Dijkstra's algorithm? Which algorithm overcomes it?

**ANSWER:**
**Limitation:** Dijkstra's algorithm **fails with negative edge weights**.

**Why?**
- Dijkstra assumes once a vertex is processed, its shortest distance is final
- Negative edges can create shorter paths discovered later
- Greedy approach doesn't reconsider processed vertices

**Example:**
```
    A --(-3)--> B
    |           |
   (2)         (1)
    |           |
    v           v
    C -------> D
        (5)

Dijkstra from A: Processes C(2), then B(infinity)... incorrect!
Correct: A→B→D = -3+1 = -2 (but B processed with wrong distance)
```

**Solution: Bellman-Ford Algorithm**
- Time: O(VE) - slower but correct
- Handles negative weights ✓
- Detects negative cycles ✓
- Relaxes all edges V-1 times

---

**Q14.** Define the Longest Common Subsequence (LCS) and how it differs from substring.

**ANSWER:**
**Longest Common Subsequence (LCS):** Longest sequence of characters that appear in same relative order in both strings, but **not necessarily consecutive**.

**Longest Common Substring:** Longest sequence that appears **consecutively** in both strings.

**Example:**
```
String X: "ABCDGH"
String Y: "AEDFHR"

LCS: "ADH" (length = 3)
- Characters A, D, H appear in order but not consecutive
- Skip B, C, G from X and E, F, R from Y

Longest Common Substring: "A" (length = 1)
- Only "A" appears consecutively in both
- "DH" doesn't appear consecutively in X
```

**Key Difference:** Subsequence allows gaps; Substring requires continuity.

---

**Q15.** Explain the recurrence relation for the 0/1 Knapsack problem.

**ANSWER:**
**Problem:** Given items with weights and values, maximize value within capacity W. Each item can be taken (1) or left (0).

**Recurrence Relation:**
```
dp[i][w] = maximum value using first i items with capacity w

Base Case:
dp[0][w] = 0 (no items)
dp[i][0] = 0 (no capacity)

Recurrence:
If weight[i] <= w:
    dp[i][w] = max(
        value[i] + dp[i-1][w-weight[i]],  // Include item i
        dp[i-1][w]                          // Exclude item i
    )
Else:
    dp[i][w] = dp[i-1][w]  // Can't include (too heavy)
```

**Meaning:** For each item, choose maximum between:
- **Taking it:** Get its value + best solution with remaining capacity
- **Leaving it:** Keep best solution without this item

---

## 📝 PART B - DETAILED ANSWER QUESTIONS

### SECTION 1: RECURSION & SIEVE (CO1)

**Q1. (10 marks)** Explain in detail the working principle of the Sieve of Sundaram algorithm for generating prime numbers. Derive the mathematical formulation used in the algorithm. Illustrate the steps involved with a suitable example.

**COMPLETE ANSWER:**

**1. INTRODUCTION (1 mark)**

The Sieve of Sundaram is an algorithm for finding all prime numbers up to a specified integer. It generates all odd prime numbers less than 2n+2, where n is a given positive integer. Named after Indian mathematician S.P. Sundaram, it works by eliminating composite numbers in a systematic way.

**2. MATHEMATICAL FORMULATION (3 marks)**

**Core Principle:**
All odd composite numbers can be expressed as: (2i+1)(2j+1) where i, j are positive integers and i ≤ j

Expanding: (2i+1)(2j+1) = 4ij + 2i + 2j + 1 = 2(2ij + i + j) + 1

Let k = i + j + 2ij, then the composite number = 2k + 1

**Derivation:**
- Every odd number can be written as 2m+1
- If 2m+1 is composite odd, then m = i + j + 2ij for some i, j
- By removing all numbers of form (i + j + 2ij), remaining numbers give primes when doubled and incremented

**Algorithm removes:** All k where k = i + j + 2ij, for 1 ≤ i ≤ j ≤ n
**Result:** Remaining numbers k transformed to 2k+1 give odd primes

**3. ALGORITHM STEPS (3 marks)**

```
SIEVE_OF_SUNDARAM(n):
1. Create list L of integers from 1 to n
2. For i = 1 to n:
      For j = i to (n-i)/(2i+1):
          Remove i + j + 2ij from list L
3. For each remaining number k in L:
      Print 2k + 1 as prime
4. Additionally print 2 (only even prime)
```

**4. EXAMPLE: Find primes < 22 (n=10) (3 marks)**

**Step 1:** Create list from 1 to 10
L = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

**Step 2:** Remove numbers of form i + j + 2ij
- i=1, j=1: 1+1+2(1)(1) = 4 → Remove 4
- i=1, j=2: 1+2+2(1)(2) = 7 → Remove 7
- i=1, j=3: 1+3+2(1)(3) = 10 → Remove 10
- i=2, j=2: 2+2+2(2)(2) = 12 > 10 (stop)

**After removal:** L = {1, 2, 3, 5, 6, 8, 9}

**Step 3:** Transform (2k+1)
- k=1 → 3, k=2 → 5, k=3 → 7, k=5 → 11, k=6 → 13, k=8 → 17, k=9 → 19

**Step 4:** Add 2

**RESULT: Primes < 22 are: 2, 3, 5, 7, 11, 13, 17, 19**

**Complexity:** Time O(n log n), Space O(n)

---

**Q2. (6 marks)** Explain the role of the call stack in recursive function execution. Illustrate with an example how activation records are created and removed during recursive calls. Analyze how base and recursive cases control the termination of recursion.

**COMPLETE ANSWER:**

**1. CALL STACK CONCEPT (2 marks)**

**Call Stack** is a Last-In-First-Out (LIFO) data structure managing function calls.

**Activation Record (Stack Frame) contains:**
- Function parameters
- Local variables  
- Return address
- Return value

**Role in Recursion:** Each recursive call creates new activation record pushed onto stack. When function returns, record is popped.

**2. EXAMPLE - FACTORIAL(4) (3 marks)**

**Function:**
```
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);
}
```

**STACK GROWTH:**
```
factorial(1) n=1, return 1     ← Top
factorial(2) n=2, wait for f(1)
factorial(3) n=3, wait for f(2)
factorial(4) n=4, wait for f(3) ← Bottom
```

**STACK UNWINDING:**
- fact(1) returns 1
- fact(2) = 2 × 1 = 2
- fact(3) = 3 × 2 = 6
- fact(4) = 4 × 6 = 24

**3. BASE AND RECURSIVE CASES (1 mark)**

**Base Case:** Termination condition, stops recursion, prevents stack overflow
Example: `if (n <= 1) return 1`

**Recursive Case:** Calls function with reduced input, moves toward base case
Example: `return n * factorial(n-1)`

Without base case → infinite recursion → Stack Overflow!

---

### SECTION 2: GREEDY ALGORITHMS (CO2)

**Q3. (16 marks)** Explain the Huffman Coding algorithm and illustrate how it constructs an optimal prefix code using a greedy strategy. Discuss its efficiency in terms of time complexity and storage reduction.

**COMPLETE ANSWER:**

**1. INTRODUCTION (2 marks)**

**Huffman Coding** is a lossless data compression algorithm assigning variable-length codes to characters based on frequencies. Frequent characters get shorter codes.

**Properties:** Prefix-free (no code is prefix of another), Optimal (minimum average length), Greedy approach

**2. GREEDY STRATEGY (3 marks)**

**Greedy Choice:** Always select two nodes with **lowest frequencies** to merge.

**Why it works:** Characters with lowest frequencies should have longest codes (deepest in tree). Merging them minimizes total weighted path length.

**3. ALGORITHM (4 marks)**

```
HUFFMAN_CODING(C):
1. Create leaf node for each character with frequency
2. Build min-heap H with all nodes
3. While H has > 1 node:
   - Extract two minimum frequency nodes
   - Create internal node with freq = sum of two
   - Insert new node into H
4. Remaining node is root
5. Generate codes: left edge='0', right='1'
```

**4. EXAMPLE (5 marks)**

**Given:** A:45, B:13, C:12, D:16, E:9, F:5

**Step 1:** Min-Heap: [F:5, E:9, C:12, B:13, D:16, A:45]

**Step 2:** Merge F(5)+E(9)=14
```
    (14)
    / \
  F:5 E:9
```

**Step 3:** Merge C(12)+B(13)=25
```
    (25)
    /  \
  C:12 B:13
```

**Step 4:** Merge (14)+D(16)=30
```
      (30)
     /    \
   (14)   D:16
   / \
 F:5 E:9
```

**Step 5:** Merge (25)+(30)=55
```
        (55)
       /    \
    (25)    (30)
    / \     / \
  C:12 B:13 (14) D:16
            / \
          F:5 E:9
```

**Step 6:** Merge A(45)+(55)=100
```
           (100)
          /     \
       A:45     (55)
               /    \
            (25)    (30)
            / \     / \
          C:12 B:13 (14) D:16
                    / \
                  F:5 E:9
```

**CODES:**
- A: 0 (1 bit)
- C: 100 (3 bits)
- B: 101 (3 bits)
- F: 1100 (4 bits)
- E: 1101 (4 bits)
- D: 111 (3 bits)

**5. EFFICIENCY (2 marks)**

**Time:** O(n log n) - n-1 merges, each O(log n)
**Space:** O(n)

**Compression:**
- Fixed: 3 bits/char × 100 chars = 300 bits
- Huffman: (45×1+13×3+12×3+16×3+9×4+5×4)/100 = 2.24 bits/char = 224 bits
- **Savings: 25%**

---

**Q4. (16 marks)** Alice found a mysterious text while reading in Wonderland. She is curious to know how many times a given word appears in the text. Your task is to implement a CPP program that can help Alice count the occurrences of the given word in the text using the Rabin-Karp algorithm.

**Expected Answer:**
1. **Problem Understanding (2 marks):** Pattern matching with hashing
2. **Rabin-Karp Concept (3 marks):** Rolling hash function explanation
3. **Algorithm (5 marks):**
   - Hash pattern
   - Hash first window of text
   - Slide window, update hash
   - Compare on hash match
4. **CPP Code (4 marks):** Implementation with rolling hash
5. **Example (2 marks):** Show with sample text

---

### SECTION 3: BACKTRACKING (CO3)

**Q5. (16 marks)** A Maze is given as an N×N binary matrix of blocks where the source block is the upper leftmost block i.e., maze[0][0] and the destination block is the lower rightmost block i.e., maze[N-1][N-1]. A rat starts from the source and has to reach its destination. The rat can move only in two directions: forward and down. Implement a CPP program to build the N×N matrix of block by applying the given constraints.

**Hint: Use Backtracking Approach**

**Expected Answer:**
1. **Problem Analysis (2 marks):** Path finding with constraints
2. **Backtracking Strategy (4 marks):**
   - Try forward, if blocked try down
   - Mark path
   - If dead-end, backtrack (unmark)
3. **Algorithm (4 marks):**
   - Base case: reached destination
   - Recursive: try valid moves
   - Backtrack on failure
4. **CPP Code (4 marks):** Complete implementation
5. **Example (2 marks):** 4×4 maze solution

---

**Q6. (16 marks)** Discuss the subset sum problem and explain how it can be solved using the backtracking approach. Illustrate the decision tree formed during recursive exploration of subsets. Analyze how time complexity varies between the backtracking and dynamic programming approaches.

**Expected Answer:**
1. **Problem Definition (2 marks):** Find subset with given sum
2. **Backtracking Approach (5 marks):**
   - Include/exclude decisions
   - Prune when sum exceeds target
   - Decision tree illustration
3. **Example (4 marks):** Array [3,5,2,7], target=10
4. **Complexity Analysis (3 marks):**
   - Backtracking: O(2^n) worst case
   - DP: O(n × sum)
   - When to use which
5. **Code Outline (2 marks):** Pseudocode

---

### SECTION 4: DYNAMIC PROGRAMMING (CO4)

**Q7. (16 marks)** Dr. Maya, a bioinformatics researcher, is analyzing protein sequences to study evolutionary relationships between different species. She is comparing two protein sequences, proteinA and proteinB, to identify key similarities and repetitions.

Dr. Maya needs to determine: The Longest Common Subsequence (LCS) shared by both protein sequences. The number of times the LCS appears as a subsequence within proteinA.

Build a CPP program to assist Dr. Maya by writing a program to calculate the LCS and count its occurrences in proteinA.

**Hint: Use Dynamic Programming Approach**

**Expected Answer:**
1. **Problem Analysis (2 marks):** LCS finding + counting occurrences
2. **LCS Algorithm (5 marks):**
   - DP table construction
   - Recurrence relation
   - Example with two sequences
3. **Counting Occurrences (4 marks):**
   - After finding LCS, count in proteinA
   - DP approach for counting
4. **CPP Implementation (4 marks):** Complete code
5. **Complexity (1 mark):** Time O(mn), Space O(mn)

---

**Q8. (16 marks)** Describe the 0/1 Knapsack problem and explain how Dynamic Programming can be applied to solve it. Develop the recurrence relation and tabular representation used in the algorithm. Illustrate the process with a numerical example and analyze its time and space complexity.

**Expected Answer:**
1. **Problem Definition (2 marks):** Items with weights/values, capacity W
2. **Why DP? (2 marks):** Overlapping subproblems, optimal substructure
3. **Recurrence Relation (3 marks):**
   ```
   dp[i][w] = max(value[i] + dp[i-1][w-weight[i]], dp[i-1][w])
   ```
4. **Tabulation (4 marks):** Show DP table construction step-by-step
5. **Example (3 marks):** 3-4 items, show table filling
6. **Complexity (2 marks):** Time O(nW), Space O(nW), optimization to O(W)

---

### SECTION 5: GRAPH ALGORITHMS (CO5)

**Q9. (16 marks)** In the world of TitanCode Arena, N elite players compete on a global leaderboard, where each player has a combat power score. The leaderboard is dynamic. Players constantly win battles, gain upgrades, or suffer defeats, causing their power scores to change. As the system architect, you are responsible for building a real-time leaderboard engine that supports the following operations:

**Query:** Find the maximum combat power among all players in a given rank range [L, R].

**Update:** When a player's score changes, update it instantly on the leaderboard.

To handle large numbers of players and frequent operations efficiently, implement the system using a Segment Tree in CPP.

**Expected Answer:**
1. **Problem Analysis (2 marks):** Range maximum query + point update
2. **Segment Tree Concept (4 marks):**
   - Binary tree structure
   - Each node stores max of range
   - Build, query, update operations
3. **Build Operation (3 marks):** Recursive construction, O(n)
4. **Query Operation (3 marks):** Range query in O(log n)
5. **Update Operation (2 marks):** Point update in O(log n)
6. **CPP Code (2 marks):** Key functions

---

**Q10. (16 marks)** Describe the working principle of the Floyd-Warshall algorithm for finding all-pairs shortest paths in a weighted graph. Demonstrate the algorithm with a suitable example showing intermediate matrix updates.

**Expected Answer:**
1. **Introduction (2 marks):** All-pairs shortest path, works with negative weights (no negative cycles)
2. **Algorithm Principle (4 marks):**
   - Dynamic programming approach
   - Try all vertices as intermediate
   - dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
3. **Example (6 marks):**
   - 4-vertex graph with weights
   - Show matrix after each k iteration
   - Highlight updates
4. **Complexity (2 marks):** Time O(V³), Space O(V²)
5. **Applications (2 marks):** When to use vs Dijkstra/Bellman-Ford

---

### MIXED TOPICS

**Q11. (16 marks)** Compare and contrast the following algorithm design techniques with examples:
- Greedy vs Dynamic Programming
- Divide and Conquer vs Backtracking

Discuss scenarios where each technique is most effective.

**Expected Answer:**
1. **Greedy vs DP (8 marks):**
   - Greedy: Local optimal → global optimal (may fail)
   - DP: Try all, store results → guaranteed optimal
   - Examples: Activity Selection (greedy works), Knapsack (DP needed)
   - When to use each
2. **D&C vs Backtracking (8 marks):**
   - D&C: Split, solve independently, combine (no going back)
   - Backtracking: Try, if fail undo and try next
   - Examples: Merge Sort (D&C), N-Queens (Backtracking)

---

**Q12. (10 marks)** Explain the concept of time complexity using Big-O notation. Analyze the time complexity of the following:
- Binary Search
- Merge Sort  
- Matrix Multiplication

**Expected Answer:**
1. **Big-O Concept (2 marks):** Upper bound, worst-case growth rate
2. **Binary Search (2 marks):** O(log n) - halves search space each time
3. **Merge Sort (3 marks):** O(n log n) - divides log n times, merges n elements
4. **Matrix Multiplication (3 marks):** O(n³) - three nested loops for n×n matrices

---

**Q13. (16 marks)** Write a detailed algorithm for Kruskal's Minimum Spanning Tree with Union-Find optimization. Explain:
- Why sorting is necessary
- How Union-Find prevents cycles
- Time complexity analysis

Illustrate with a graph example.

**Expected Answer:**
1. **MST Concept (2 marks):** Connects all vertices, minimum total weight, no cycles
2. **Algorithm Steps (4 marks):**
   - Sort edges by weight
   - Initialize Union-Find
   - For each edge, if doesn't create cycle, add it
3. **Union-Find (4 marks):**
   - Find: Check if vertices in same set
   - Union: Merge sets
   - Path compression + rank optimization
4. **Example (4 marks):** 5-6 vertex graph, show edge selection
5. **Complexity (2 marks):** O(E log E) for sorting, nearly O(E) for Union-Find

---

**Q14. (16 marks)** Implement and explain the KMP (Knuth-Morris-Pratt) string matching algorithm. Show:
- LPS (Longest Prefix Suffix) array construction
- How LPS array avoids backtracking
- Complete algorithm with example

**Expected Answer:**
1. **Problem & Motivation (2 marks):** Pattern matching, avoid redundant comparisons
2. **LPS Array (5 marks):**
   - Definition: Longest proper prefix which is also suffix
   - Construction algorithm
   - Example: "AABAACAABAA" → LPS array
3. **Search Algorithm (5 marks):**
   - Use LPS to skip comparisons
   - When mismatch, jump using LPS
   - Example search in text
4. **Complexity (2 marks):** O(m) for LPS, O(n) for search, total O(n+m)
5. **Code Outline (2 marks):** Key functions

---

## 🎯 HOW TO PRACTICE THESE QUESTIONS

### For 2-Mark Questions:
- **Time limit:** 3-4 minutes per question
- **Length:** 5-7 lines maximum
- **Must include:** Definition + key point + example (if space)

### For 16-Mark Questions:
- **Time limit:** 25-30 minutes
- **Structure:**
  1. Introduction/Definition (2-3 marks)
  2. Algorithm/Concept explanation (5-6 marks)
  3. Detailed example (4-5 marks)
  4. Complexity analysis (2 marks)
  5. Code/Applications (2-3 marks)

### For 10-Mark Questions:
- **Time limit:** 15-18 minutes
- Similar structure but less detailed

---

## ⚡ EXPECTED HIGH-PROBABILITY QUESTIONS

Based on your model paper and teacher's emphasis:

### MUST PREPARE (90% chance):
1. ✅ **Kruskal's Algorithm** - Full algorithm with example
2. ✅ **Dijkstra's Algorithm** - With limitation discussion
3. ✅ **0/1 Knapsack** - DP approach with table
4. ✅ **LCS Problem** - With example and code
5. ✅ **KMP Algorithm** - LPS array construction
6. ✅ **N-Queens** - Backtracking approach
7. ✅ **Fibonacci** - Recursion vs DP comparison

### HIGH PROBABILITY (70% chance):
8. ✅ **Coin Change** - Greedy failure + DP solution
9. ✅ **Activity Selection** - Greedy approach
10. ✅ **Rabin-Karp** - String matching with hashing
11. ✅ **Subset Sum** - Backtracking approach
12. ✅ **Huffman Coding** - Greedy strategy
13. ✅ **Segment Tree** - Range queries

### MEDIUM PROBABILITY (50% chance):
14. ✅ **Tower of Hanoi** - Recursive solution
15. ✅ **Graph Coloring** - Backtracking
16. ✅ **LIS (Longest Increasing Subsequence)**
17. ✅ **Floyd-Warshall** - All-pairs shortest path
18. ✅ **Manacher's Algorithm** - Palindrome

---

## 📝 ANSWER WRITING TIPS FROM MODEL PAPER ANALYSIS

### What Examiners Look For:
1. ✅ **Clear definitions** at start
2. ✅ **Step-by-step explanation** (numbered)
3. ✅ **Diagrams/examples** (especially for graphs, trees)
4. ✅ **Complexity analysis** (time & space)
5. ✅ **Code snippets** where asked

### Common Mistakes to Avoid:
❌ Jumping to code without explanation
❌ No examples/diagrams
❌ Forgetting complexity analysis
❌ Not highlighting key concepts
❌ Poor time management

### Scoring Pattern:
- **Theory/Concept:** 40-50%
- **Example/Illustration:** 30-40%
- **Code/Implementation:** 10-20%
- **Analysis:** 10-20%

---

## 🚀 FINAL EXAM STRATEGY

**Time Allocation (3 hours = 180 minutes):**
- Reading & planning: 10 min
- Part A (10 × 2 marks): 40 min (4 min each)
- Part B (80 marks): 110 min
  - 16-mark questions: 25-30 min each
  - 10-mark questions: 15-18 min each
- Review: 20 min

**Question Selection Strategy:**
- Read all Part B questions first
- Pick questions you're MOST confident in
- Do easy ones first (confidence boost!)
- Save hardest for last

**During Exam:**
- Underline keywords in questions
- Plan answer structure before writing
- Draw diagrams clearly with labels
- Box important formulas/complexity
- Leave space between answers (for additions)

---

**PRACTICE THESE QUESTIONS TONIGHT!**

Pick 2-3 from each section and write full answers. Time yourself! 🎯

Good luck! 💪📚
