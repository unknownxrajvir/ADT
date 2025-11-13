# Backtracking 2

## 📌 KEY DEFINITIONS

**Subset Sum**: Find subset of numbers that sum to target

**Hamiltonian Path**: Path visiting each vertex exactly once

**Hamiltonian Cycle**: Hamiltonian path that returns to start

**Graph Coloring**: Assign colors to vertices such that no adjacent vertices have same color

**Constraint Satisfaction Problem (CSP)**: Find values for variables satisfying all constraints

---

## 📝 ADVANCED BACKTRACKING PROBLEMS

### 1. SUBSET SUM PROBLEM

**Problem**: Find if subset exists with given sum

```
SUBSET_SUM(arr, n, target, current, index):
    // Base case: target reached
    if target == 0:
        PRINT(current)
        return true
    
    // Base case: no elements left or target negative
    if index == n OR target < 0:
        return false
    
    // Choice 1: Include current element
    current.append(arr[index])
    if SUBSET_SUM(arr, n, target - arr[index], current, index + 1):
        return true
    current.removeLast()
    
    // Choice 2: Exclude current element
    if SUBSET_SUM(arr, n, target, current, index + 1):
        return true
    
    return false

Time: O(2^n)
Space: O(n) for recursion stack
```

**Example**:
```
arr = [3, 34, 4, 12, 5, 2]
target = 9

Solutions:
[4, 5]
[3, 4, 2]
```

**Optimized with Sorting**:
```
SUBSET_SUM_OPTIMIZED(arr, target, current, index):
    if target == 0:
        PRINT(current)
        return true
    
    if index == arr.size OR target < 0:
        return false
    
    // Pruning: Skip if current element too large
    if arr[index] > target:
        return false
    
    // Include current element
    current.append(arr[index])
    if SUBSET_SUM_OPTIMIZED(arr, target - arr[index], current, index + 1):
        return true
    current.removeLast()
    
    // Exclude current element
    return SUBSET_SUM_OPTIMIZED(arr, target, current, index + 1)
```

---

### 2. HAMILTONIAN PATH

**Problem**: Find path that visits each vertex exactly once

```
HAMILTONIAN_PATH(graph, path, visited, v, count, n):
    // Mark current vertex as visited
    visited[v] = true
    path.append(v)
    count++
    
    // Base case: all vertices visited
    if count == n:
        PRINT(path)
        return true
    
    // Try all adjacent vertices
    for each vertex u adjacent to v:
        if not visited[u]:
            if HAMILTONIAN_PATH(graph, path, visited, u, count, n):
                return true
    
    // Backtrack
    visited[v] = false
    path.removeLast()
    return false

Time: O(N!)
Space: O(N)
```

**Visual Example**:
```
Graph:
  0---1
  |   |
  3---2

Hamiltonian Paths:
0 → 1 → 2 → 3
0 → 3 → 2 → 1
1 → 0 → 3 → 2
etc.
```

---

### 3. HAMILTONIAN CYCLE

**Problem**: Find cycle that visits each vertex exactly once and returns to start

```
HAMILTONIAN_CYCLE(graph, path, visited, v, count, n, start):
    visited[v] = true
    path.append(v)
    count++
    
    // Base case: all vertices visited
    if count == n:
        // Check if there's edge back to start
        if graph[v][start] == 1:
            path.append(start)
            PRINT(path)
            return true
        return false
    
    // Try all adjacent vertices
    for each vertex u adjacent to v:
        if not visited[u]:
            if HAMILTONIAN_CYCLE(graph, path, visited, u, count, n, start):
                return true
    
    // Backtrack
    visited[v] = false
    path.removeLast()
    return false

Time: O(N!)
Space: O(N)
```

---

### 4. GRAPH COLORING (M-COLORING)

**Problem**: Color graph with m colors such that no adjacent vertices have same color

```
GRAPH_COLORING(graph, m, colors, vertex, n):
    // Base case: all vertices colored
    if vertex == n:
        PRINT(colors)
        return true
    
    // Try each color for current vertex
    for color = 1 to m:
        if IS_SAFE_COLOR(graph, colors, vertex, color):
            // Make choice
            colors[vertex] = color
            
            // Recurse
            if GRAPH_COLORING(graph, m, colors, vertex + 1, n):
                return true
            
            // Backtrack
            colors[vertex] = 0
    
    return false

IS_SAFE_COLOR(graph, colors, vertex, color):
    // Check all adjacent vertices
    for i = 0 to n-1:
        if graph[vertex][i] == 1 AND colors[i] == color:
            return false
    return true

Time: O(m^n)
Space: O(n)
```

**Example**:
```
Graph:
  0---1
  |\ /|
  | X |
  |/ \|
  3---2

3-Coloring:
0: Red
1: Green
2: Blue
3: Blue
```

### Chromatic Number
```
Minimum number of colors needed to color graph

FIND_CHROMATIC_NUMBER(graph, n):
    for m = 1 to n:
        if GRAPH_COLORING(graph, m, colors, 0, n):
            return m
    return n
```

---

### 5. PARTITION PROBLEM

**Problem**: Partition set into two subsets with equal sum

```
CAN_PARTITION(arr, n):
    totalSum = sum(arr)
    
    // If total sum is odd, can't partition
    if totalSum % 2 != 0:
        return false
    
    target = totalSum / 2
    return SUBSET_SUM_EXISTS(arr, n, target, 0)

SUBSET_SUM_EXISTS(arr, n, target, index):
    // Base cases
    if target == 0:
        return true
    
    if index == n OR target < 0:
        return false
    
    // Include or exclude current element
    return SUBSET_SUM_EXISTS(arr, n, target - arr[index], index + 1) OR
           SUBSET_SUM_EXISTS(arr, n, target, index + 1)

Time: O(2^n)
Space: O(n)
```

**Example**:
```
arr = [1, 5, 11, 5]
sum = 22, target = 11

Partition 1: {1, 5, 5} = 11
Partition 2: {11} = 11
Answer: true
```

---

### 6. PALINDROME PARTITIONING

**Problem**: Partition string into substrings where each is palindrome

```
PALINDROME_PARTITION(s, start, current, result):
    // Base case: reached end
    if start == s.length:
        result.add(current.copy())
        return
    
    // Try all possible partitions
    for end = start to s.length-1:
        if IS_PALINDROME(s, start, end):
            // Make choice
            current.append(s[start...end])
            
            // Recurse
            PALINDROME_PARTITION(s, end + 1, current, result)
            
            // Backtrack
            current.removeLast()

IS_PALINDROME(s, start, end):
    while start < end:
        if s[start] != s[end]:
            return false
        start++
        end--
    return true

Time: O(n × 2^n)
Space: O(n)
```

**Example**:
```
Input: "aab"
Output:
["a", "a", "b"]
["aa", "b"]
```

---

### 7. WORD SEARCH

**Problem**: Find if word exists in 2D grid (can move up/down/left/right)

```
WORD_SEARCH(board, word):
    m = rows, n = cols
    
    for i = 0 to m-1:
        for j = 0 to n-1:
            if board[i][j] == word[0]:
                if DFS_SEARCH(board, word, i, j, 0, m, n):
                    return true
    return false

DFS_SEARCH(board, word, row, col, index, m, n):
    // Base case: found word
    if index == word.length:
        return true
    
    // Check boundaries and character match
    if row < 0 OR row >= m OR col < 0 OR col >= n:
        return false
    
    if board[row][col] != word[index]:
        return false
    
    // Mark as visited
    temp = board[row][col]
    board[row][col] = '#'
    
    // Try all 4 directions
    found = DFS_SEARCH(board, word, row+1, col, index+1, m, n) OR
            DFS_SEARCH(board, word, row-1, col, index+1, m, n) OR
            DFS_SEARCH(board, word, row, col+1, index+1, m, n) OR
            DFS_SEARCH(board, word, row, col-1, index+1, m, n)
    
    // Backtrack
    board[row][col] = temp
    
    return found

Time: O(m × n × 4^L) where L = word length
Space: O(L) for recursion
```

---

## 💡 OPTIMIZATION TECHNIQUES

### 1. Early Termination
```
If solution impossible, return immediately:
- Subset sum: If remaining elements too small
- Graph coloring: If no valid color exists
```

### 2. Sorting for Pruning
```
Sort array in subset sum:
- Skip remaining if current element > target
```

### 3. Most Constrained Variable First
```
In graph coloring:
- Color vertex with most colored neighbors first
- Reduces branching factor
```

### 4. Forward Checking
```
Eliminate values from domains of unassigned variables:
- Catch failures earlier
```

### 5. Bitmask for Visited State
```
Use integer to track visited vertices:
- Faster than boolean array
- Useful for small graphs (N ≤ 20)
```

---

## 🎯 PROBLEM IDENTIFICATION

### How to recognize backtracking?

✅ **"Find all solutions"** → Likely backtracking

✅ **"Find if solution exists"** → Could be backtracking

✅ **Constraints on combinations** → Backtracking

✅ **No greedy or DP approach obvious** → Try backtracking

### Backtracking vs DP

| Backtracking | Dynamic Programming |
|--------------|---------------------|
| Find ALL solutions | Find OPTIMAL solution |
| Enumerate possibilities | Overlapping subproblems |
| Exponential time | Polynomial time |
| Example: N-Queens | Example: 0/1 Knapsack |

---

## 📊 COMPLEXITY SUMMARY

| Problem | Time Complexity | Space |
|---------|----------------|-------|
| Subset Sum | O(2^n) | O(n) |
| Hamiltonian Path | O(n!) | O(n) |
| Hamiltonian Cycle | O(n!) | O(n) |
| Graph Coloring | O(m^n) | O(n) |
| Partition | O(2^n) | O(n) |
| Palindrome Partition | O(n × 2^n) | O(n) |
| Word Search | O(mn × 4^L) | O(L) |

---

## 🚨 COMMON MISTAKES

❌ Forgetting to backtrack (undo choice)

❌ Not checking base cases properly

❌ Modifying global state without restoring

❌ Infinite recursion due to wrong base case

❌ Not pruning search space (inefficient)

---

## 📖 MUST REMEMBER

1. **Subset Sum**: Include/exclude pattern, O(2^n)
2. **Hamiltonian Path**: Visit each vertex once
3. **Hamiltonian Cycle**: Path + return to start
4. **Graph Coloring**: Try each color, check adjacency
5. **Always backtrack**: Restore state after recursive call
6. **Prune early**: Check validity before recursing
7. **Mark visited**: Use boolean array or modify board
8. **Unmark when backtracking**: Restore original state
9. **Base cases**: Solution found OR impossible to continue
10. **Optimization**: Sort, prune, constraint propagation
