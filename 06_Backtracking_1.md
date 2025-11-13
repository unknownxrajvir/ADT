# Backtracking 1

## 📌 KEY DEFINITIONS

**Backtracking**: Systematic method to explore all possible solutions by building candidates incrementally and abandoning ("backtracking") when candidate cannot lead to valid solution

**Search Space**: All possible solutions/states that can be explored

**Candidate Solution**: Partial solution being built

**Constraint**: Rule that valid solution must satisfy

**Pruning**: Eliminating branches of search tree that cannot lead to solution

---

## 🎯 BACKTRACKING TEMPLATE

### General Structure
```
BACKTRACK(candidate):
    if IS_SOLUTION(candidate):
        OUTPUT(candidate)
        return
    
    for each choice in GET_CHOICES(candidate):
        if IS_VALID(choice):
            MAKE_CHOICE(choice)
            BACKTRACK(extended_candidate)
            UNDO_CHOICE(choice)  // Backtrack!

Key Steps:
1. Check if solution found
2. Generate possible choices
3. For each valid choice:
   - Make choice
   - Recurse
   - Undo choice (backtrack)
```

---

## 📝 CLASSIC PROBLEMS

### 1. N-QUEENS PROBLEM

**Problem**: Place N queens on N×N chessboard so no two queens attack each other

**Constraints**:
- No two queens in same row
- No two queens in same column  
- No two queens on same diagonal

```
N_QUEENS(board, row, n):
    // Base case: all queens placed
    if row == n:
        PRINT_SOLUTION(board)
        return
    
    // Try placing queen in each column
    for col = 0 to n-1:
        if IS_SAFE(board, row, col, n):
            // Make choice
            board[row][col] = 1
            
            // Recurse
            N_QUEENS(board, row + 1, n)
            
            // Undo choice (backtrack)
            board[row][col] = 0

IS_SAFE(board, row, col, n):
    // Check column
    for i = 0 to row-1:
        if board[i][col] == 1:
            return false
    
    // Check upper left diagonal
    i = row - 1
    j = col - 1
    while i >= 0 AND j >= 0:
        if board[i][j] == 1:
            return false
        i--
        j--
    
    // Check upper right diagonal
    i = row - 1
    j = col + 1
    while i >= 0 AND j < n:
        if board[i][j] == 1:
            return false
        i--
        j++
    
    return true

Time: O(N!)
Space: O(N²) for board + O(N) for recursion stack
```

**Example (N=4)**:
```
Solution 1:        Solution 2:
. Q . .            . . Q .
. . . Q            Q . . .
Q . . .            . . . Q
. . Q .            . Q . .
```

**Optimization**: Use boolean arrays for columns and diagonals
```
- columns[n]: Track occupied columns
- diagonal1[2n-1]: Track / diagonals (row-col+n-1)
- diagonal2[2n-1]: Track \ diagonals (row+col)
```

---

### 2. SUDOKU SOLVER

**Problem**: Fill 9×9 grid so each row, column, and 3×3 box contains digits 1-9

```
SOLVE_SUDOKU(board):
    // Find next empty cell
    row, col = FIND_EMPTY_CELL(board)
    
    // Base case: no empty cell
    if row == -1:
        return true  // Solved!
    
    // Try digits 1-9
    for num = 1 to 9:
        if IS_VALID(board, row, col, num):
            // Make choice
            board[row][col] = num
            
            // Recurse
            if SOLVE_SUDOKU(board):
                return true
            
            // Undo choice (backtrack)
            board[row][col] = 0
    
    return false  // No solution found

IS_VALID(board, row, col, num):
    // Check row
    for i = 0 to 8:
        if board[row][i] == num:
            return false
    
    // Check column
    for i = 0 to 8:
        if board[i][col] == num:
            return false
    
    // Check 3×3 box
    boxRow = (row / 3) × 3
    boxCol = (col / 3) × 3
    for i = boxRow to boxRow+2:
        for j = boxCol to boxCol+2:
            if board[i][j] == num:
                return false
    
    return true

Time: O(9^m) where m = empty cells
Space: O(m) for recursion stack
```

---

### 3. PERMUTATIONS

**Problem**: Generate all permutations of array

```
PERMUTATIONS(arr, start, end):
    // Base case
    if start == end:
        PRINT(arr)
        return
    
    for i = start to end:
        // Make choice: swap
        swap(arr[start], arr[i])
        
        // Recurse
        PERMUTATIONS(arr, start + 1, end)
        
        // Undo choice (backtrack)
        swap(arr[start], arr[i])

Time: O(N × N!)
Space: O(N) for recursion stack
```

**Example**:
```
Input: [1, 2, 3]
Output:
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 2, 1]
[3, 1, 2]
```

**Alternative (Without Swapping)**:
```
PERMUTATIONS_BOOL(arr, used, current, result):
    if current.size == arr.size:
        result.add(current.copy())
        return
    
    for i = 0 to arr.size-1:
        if not used[i]:
            used[i] = true
            current.append(arr[i])
            
            PERMUTATIONS_BOOL(arr, used, current, result)
            
            current.removeLast()
            used[i] = false
```

---

### 4. COMBINATIONS

**Problem**: Generate all k-sized combinations from n elements

```
COMBINATIONS(arr, k, start, current, result):
    // Base case: found k elements
    if current.size == k:
        result.add(current.copy())
        return
    
    // Try each element from start
    for i = start to arr.size-1:
        // Make choice
        current.append(arr[i])
        
        // Recurse with next element
        COMBINATIONS(arr, k, i + 1, current, result)
        
        // Undo choice (backtrack)
        current.removeLast()

Time: O(C(n,k) × k) = O(n! / ((n-k)! × k!) × k)
Space: O(k) for recursion depth
```

**Example**:
```
Input: arr = [1,2,3,4], k = 2
Output:
[1,2], [1,3], [1,4]
[2,3], [2,4]
[3,4]
```

---

### 5. SUBSETS (POWER SET)

**Problem**: Generate all possible subsets

```
SUBSETS(arr, index, current, result):
    // Add current subset to result
    result.add(current.copy())
    
    // Try adding each remaining element
    for i = index to arr.size-1:
        // Make choice: include arr[i]
        current.append(arr[i])
        
        // Recurse
        SUBSETS(arr, i + 1, current, result)
        
        // Undo choice (backtrack)
        current.removeLast()

Time: O(2^n × n)
Space: O(n) for recursion depth
```

**Example**:
```
Input: [1, 2, 3]
Output:
[]
[1]
[1,2]
[1,2,3]
[1,3]
[2]
[2,3]
[3]
```

**Alternative (Include/Exclude)**:
```
SUBSETS_RECURSIVE(arr, index, current, result):
    if index == arr.size:
        result.add(current.copy())
        return
    
    // Exclude current element
    SUBSETS_RECURSIVE(arr, index + 1, current, result)
    
    // Include current element
    current.append(arr[index])
    SUBSETS_RECURSIVE(arr, index + 1, current, result)
    current.removeLast()
```

---

## 🎓 BACKTRACKING CHARACTERISTICS

### Decision Tree
```
Every backtracking can be visualized as tree:
- Root: Starting state
- Nodes: Partial solutions
- Edges: Choices made
- Leaves: Complete solutions or dead ends
```

### Time Complexity
Usually exponential:
- **Permutations**: O(N!)
- **Combinations**: O(2^N)
- **N-Queens**: O(N!)
- **Sudoku**: O(9^m)

### Space Complexity
- **Recursion stack**: O(depth of tree)
- **Auxiliary space**: O(state size)

---

## 💡 OPTIMIZATION TECHNIQUES

### 1. Pruning
```
Skip branches that cannot lead to solution:
- In N-Queens: Skip columns already occupied
- In Sudoku: Skip numbers that violate constraints
```

### 2. Constraint Propagation
```
Reduce search space by enforcing constraints early:
- Maintain validity at each step
- Use additional data structures for O(1) checks
```

### 3. Ordering Heuristics
```
Choose most constrained variable first:
- In Sudoku: Fill cell with fewest possibilities
- Reduces branching factor
```

### 4. Memoization
```
Cache results of subproblems:
- Only applicable when subproblems repeat
- Converts to dynamic programming
```

---

## 🚨 COMMON PATTERNS

### Pattern 1: Fixed Position (N-Queens)
```
for row = 0 to n-1:
    Try each column
```

### Pattern 2: Include/Exclude (Subsets)
```
For each element:
    - Exclude it (recurse)
    - Include it (recurse + backtrack)
```

### Pattern 3: Try All Options (Sudoku)
```
For each empty cell:
    Try each possible value
```

---

## 📖 MUST REMEMBER

1. **Three key steps**: Make choice → Recurse → Undo choice
2. **Base case**: When to stop (solution found or impossible)
3. **Validity check**: Prune invalid branches early
4. **Time complexity**: Usually exponential (O(2^N) or O(N!))
5. **Space complexity**: O(depth) for recursion stack
6. **N-Queens**: Place row by row, check column and diagonals
7. **Permutations**: Swap or use boolean array
8. **Combinations**: Start index prevents duplicates
9. **Subsets**: Include/exclude or iterative from index
10. **Always restore state** after recursive call (backtrack)

---

## 🎯 EXAM TIPS

✅ Draw recursion tree for small examples

✅ Identify base case first

✅ Check pruning opportunities

✅ Count states to estimate time complexity

✅ Remember to undo choices (backtrack step)

✅ Use helper functions for validity checks
