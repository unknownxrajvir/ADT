# ⚡ LAST MINUTE REVISION - 30 MINUTES BEFORE EXAM

## 🔥 MUST REMEMBER DEFINITIONS

### GRAPHS
**Kruskal:** "Greedy algorithm for MST. Sorts edges, picks smallest without cycle. Uses Union-Find. O(E log E)"

**Dijkstra:** "Shortest path from source. Uses min-heap. **ONLY non-negative weights!** O((V+E) log V)"

### RECURSION
**Key:** Always write **BASE CASE** first!
- Fibonacci: F(0)=0, F(1)=1, F(n)=F(n-1)+F(n-2)
- Tower of Hanoi: 2^n - 1 moves
- GCD: Euclidean algorithm, O(log n)

### GREEDY
**Works:** Activity Selection, Fractional Knapsack, MST, Dijkstra
**Fails:** 0/1 Knapsack, Arbitrary Coin Change → **Use DP!**

### STRINGS
**Rabin-Karp:** Rolling hash, O(n+m) average
**KMP:** LPS array prevents backtracking, O(n+m) guaranteed
**Manacher's:** Longest palindrome in O(n)

### BACKTRACKING
**Pattern:** Try → Recurse → Backtrack (undo)
- N-Queens: Check row, column, diagonals
- Graph Coloring: Check adjacent vertices
- Sudoku: Check row, column, 3×3 box

### DYNAMIC PROGRAMMING
**Key Properties:**
1. Overlapping subproblems
2. Optimal substructure

**Patterns:**
- **0/1 Knapsack:** Include/exclude each item
- **Coin Change:** dp[i] = min(dp[i], 1 + dp[i-coin])
- **LCS:** Match → diagonal+1, No match → max(top,left)
- **LIS:** Check all previous smaller elements
- **LPS:** Match ends → 2+middle, else max(skip left, skip right)

---

## ⚡ QUICK COMPLEXITY TABLE

| Algorithm | Time | When to Use |
|-----------|------|-------------|
| Kruskal | O(E log E) | Sparse graphs, MST |
| Dijkstra | O((V+E)logV) | Non-negative weights only! |
| KMP | O(n+m) | Pattern matching |
| Activity Selection | O(n log n) | Greedy works! |
| 0/1 Knapsack | O(n×W) | Need DP (greedy fails) |
| LCS | O(m×n) | Comparing sequences |
| N-Queens | O(N!) | Backtracking |

---

## 🎯 ANSWER WRITING FORMAT

**For ANY Algorithm Question:**

1. **Definition** (2 lines)
   > "[Algorithm] is a [type] that [purpose] by [method]."

2. **Example** (Show small case)
   > Draw or show steps clearly

3. **Complexity** (1 line)
   > Time: O(...), Space: O(...)

4. **Key Point** (1 line)
   > Limitation/advantage/when to use

---

## ⚠️ COMMON MISTAKES - AVOID THESE!

❌ Dijkstra with negative weights → **Wrong! Use Bellman-Ford**
❌ Greedy for 0/1 Knapsack → **Wrong! Use DP**
❌ Forgetting base case in recursion → **Will fail!**
❌ Confusing LCS (subsequence) with substring
❌ Not stating time complexity → **Lose marks!**

---

## 💡 MAGIC PHRASES (Use These for Extra Marks!)

✨ "This exhibits **optimal substructure** property..."
✨ "The **greedy choice** is safe because..."
✨ "We avoid **overlapping subproblems** using DP..."
✨ "Time complexity is O(...) due to **[reason]**..."
✨ "This is **not optimal** when [case]..."

---

## 🚀 TOP 5 MUST-KNOW ALGORITHMS

### 1. KRUSKAL'S (Graph - MST)
- Sort edges by weight
- Pick smallest without cycle
- Uses Union-Find
- **O(E log E)**

### 2. DIJKSTRA'S (Shortest Path)
- Uses min-heap
- Pick closest unvisited vertex
- **NON-NEGATIVE WEIGHTS ONLY!**
- **O((V+E) log V)**

### 3. 0/1 KNAPSACK (DP)
- Include or exclude each item
- **dp[i][w] = max(include, exclude)**
- **O(n × W)**

### 4. LCS (DP - Strings)
- Match → diagonal + 1
- No match → max(top, left)
- **O(m × n)**

### 5. N-QUEENS (Backtracking)
- Place row by row
- Check column & diagonals
- Backtrack if stuck
- **O(N!)**

---

## 📝 IF YOU ONLY HAVE 10 MINUTES

Read these sections in **00_EXAM_PRIORITY_TOPICS.md**:
1. Kruskal's (page 1)
2. Dijkstra's (page 2)
3. 0/1 Knapsack (DP section)
4. Coin Change (DP section)
5. LCS (DP section)

**Look at the Complexity Table!**

---

## 🎯 EXAM STRATEGY

**Time Management:**
- Read all questions first (5 min)
- Do easy ones first (confidence boost!)
- Save hardest for last
- Keep 10 min for review

**If Stuck:**
- Write definition at least
- Draw a diagram/example
- State complexity
- Move on (partial marks matter!)

**Must Do:**
- Start with what you know
- Show your work (step by step)
- Box final answers
- Use diagrams

---

## 💪 CONFIDENCE BOOSTERS

✅ Teacher gave you priority list - these WILL come!
✅ You have studied ALL priority topics
✅ Theory exam = Explain concepts (you can do this!)
✅ Partial marks exist - write something for every question
✅ Examples show understanding - always include them

---

## 🔥 LAST WORDS

**Don't panic during exam!**
- If you forget exact algorithm, explain the concept
- Draw diagrams (worth many words!)
- State complexity (easy marks!)
- Read question carefully
- Manage your time

**You know this material. Trust yourself!** 🚀

---

**NOW CLOSE THIS AND GO TO EXAM WITH CONFIDENCE!** 💪✨
