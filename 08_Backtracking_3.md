# Backtracking 3

## 📌 KEY DEFINITIONS

**Rat in Maze**: Find path from source to destination in maze

**Word Break**: Segment string into dictionary words

**Expression Evaluation**: Place operators to achieve target value

**Cryptarithmetic**: Assign digits to letters in arithmetic equation

**Constraint Satisfaction**: Find assignment satisfying all constraints

---

## 📝 ADVANCED BACKTRACKING APPLICATIONS

### 1. RAT IN MAZE

**Problem**: Find path from (0,0) to (n-1,n-1) in maze (1=open, 0=blocked)

```
SOLVE_MAZE(maze, n):
    solution = matrix of size n×n, initialized to 0
    
    if RAT_IN_MAZE(maze, 0, 0, solution, n):
        PRINT(solution)
        return true
    
    return false

RAT_IN_MAZE(maze, row, col, solution, n):
    // Base case: reached destination
    if row == n-1 AND col == n-1:
        solution[row][col] = 1
        return true
    
    // Check if current cell is valid
    if IS_SAFE(maze, row, col, n):
        // Mark as part of solution path
        solution[row][col] = 1
        
        // Move forward (right)
        if RAT_IN_MAZE(maze, row, col + 1, solution, n):
            return true
        
        // Move down
        if RAT_IN_MAZE(maze, row + 1, col, solution, n):
            return true
        
        // Backtrack: unmark cell
        solution[row][col] = 0
        return false
    
    return false

IS_SAFE(maze, row, col, n):
    return row >= 0 AND row < n AND 
           col >= 0 AND col < n AND 
           maze[row][col] == 1

Time: O(2^(n²)) worst case
Space: O(n²)
```

**Example**:
```
Maze:
1 0 0 0
1 1 0 1
0 1 0 0
1 1 1 1

Solution:
1 0 0 0
1 1 0 0
0 1 0 0
0 1 1 1
```

### 4-Direction Movement Version
```
RAT_IN_MAZE_4DIR(maze, row, col, solution, n):
    if row == n-1 AND col == n-1:
        solution[row][col] = 1
        return true
    
    if IS_SAFE(maze, row, col, n) AND solution[row][col] == 0:
        solution[row][col] = 1
        
        // Try all 4 directions: Down, Right, Up, Left
        directions = [(1,0), (0,1), (-1,0), (0,-1)]
        
        for (dx, dy) in directions:
            if RAT_IN_MAZE_4DIR(maze, row+dx, col+dy, solution, n):
                return true
        
        solution[row][col] = 0  // Backtrack
        return false
    
    return false
```

### Print All Paths
```
FIND_ALL_PATHS(maze, row, col, path, n):
    // Base case
    if row == n-1 AND col == n-1:
        path.append("D")  // Destination
        PRINT(path)
        return
    
    if IS_SAFE(maze, row, col, n):
        // Mark visited
        maze[row][col] = 0
        
        // Down
        if row + 1 < n:
            path.append("D")
            FIND_ALL_PATHS(maze, row+1, col, path, n)
            path.removeLast()
        
        // Right
        if col + 1 < n:
            path.append("R")
            FIND_ALL_PATHS(maze, row, col+1, path, n)
            path.removeLast()
        
        // Up
        if row - 1 >= 0:
            path.append("U")
            FIND_ALL_PATHS(maze, row-1, col, path, n)
            path.removeLast()
        
        // Left
        if col - 1 >= 0:
            path.append("L")
            FIND_ALL_PATHS(maze, row, col-1, path, n)
            path.removeLast()
        
        // Unmark (backtrack)
        maze[row][col] = 1
```

---

### 2. WORD BREAK

**Problem**: Check if string can be segmented into dictionary words

```
WORD_BREAK(s, dictionary, start, current):
    // Base case: reached end of string
    if start == s.length:
        PRINT(current)
        return true
    
    found = false
    
    // Try all possible prefixes
    for end = start to s.length-1:
        prefix = s[start...end]
        
        if prefix in dictionary:
            // Make choice
            current.append(prefix)
            
            // Recurse
            if WORD_BREAK(s, dictionary, end + 1, current):
                found = true
            
            // Backtrack
            current.removeLast()
    
    return found

Time: O(2^n)
Space: O(n)
```

**Example**:
```
s = "catsanddog"
dictionary = ["cat", "cats", "and", "sand", "dog"]

Solutions:
["cats", "and", "dog"]
["cat", "sand", "dog"]
```

### With Memoization
```
WORD_BREAK_MEMO(s, dictionary, start, memo):
    if start == s.length:
        return true
    
    if start in memo:
        return memo[start]
    
    for end = start to s.length-1:
        if s[start...end] in dictionary:
            if WORD_BREAK_MEMO(s, dictionary, end + 1, memo):
                memo[start] = true
                return true
    
    memo[start] = false
    return false

Time: O(n² × m) where m = avg word length
```

---

### 3. EXPRESSION EVALUATION

**Problem**: Place operators (+, -, ×) between digits to get target

```
EXPRESSION_ADD_OPERATORS(num, target, pos, value, path, result):
    // Base case: processed all digits
    if pos == num.length:
        if value == target:
            result.append(path)
        return
    
    for i = pos to num.length-1:
        // Current number
        curr = num[pos...i]
        currNum = parseInt(curr)
        
        // Skip numbers with leading zeros
        if i != pos AND num[pos] == '0':
            break
        
        if pos == 0:
            // First number, no operator
            EXPRESSION_ADD_OPERATORS(num, target, i+1, currNum, curr, result)
        else:
            // Try + operator
            EXPRESSION_ADD_OPERATORS(num, target, i+1, value+currNum, 
                                    path + "+" + curr, result)
            
            // Try - operator
            EXPRESSION_ADD_OPERATORS(num, target, i+1, value-currNum, 
                                    path + "-" + curr, result)
            
            // Try * operator (more complex due to precedence)
            EXPRESSION_ADD_OPERATORS(num, target, i+1, 
                                    value - last + last*currNum,
                                    path + "*" + curr, result)

Time: O(4^n)
Space: O(n)
```

**Example**:
```
Input: num = "123", target = 6
Output:
"1+2+3"
"1*2*3"
```

---

### 4. CRYPTARITHMETIC

**Problem**: Assign digits (0-9) to letters such that arithmetic equation is valid

**Example**: SEND + MORE = MONEY

```
SOLVE_CRYPTARITHMETIC(letters, usedDigits, assignment, index):
    // Base case: all letters assigned
    if index == letters.length:
        if IS_VALID_EQUATION(assignment):
            PRINT_SOLUTION(assignment)
            return true
        return false
    
    currentLetter = letters[index]
    
    // Try each digit 0-9
    for digit = 0 to 9:
        if not usedDigits[digit]:
            // Leading letters can't be 0
            if digit == 0 AND IS_LEADING_LETTER(currentLetter):
                continue
            
            // Make choice
            assignment[currentLetter] = digit
            usedDigits[digit] = true
            
            // Recurse
            if SOLVE_CRYPTARITHMETIC(letters, usedDigits, assignment, index+1):
                return true
            
            // Backtrack
            assignment[currentLetter] = -1
            usedDigits[digit] = false
    
    return false

IS_VALID_EQUATION(assignment):
    // For SEND + MORE = MONEY
    SEND = getValue("SEND", assignment)
    MORE = getValue("MORE", assignment)
    MONEY = getValue("MONEY", assignment)
    
    return SEND + MORE == MONEY

Time: O(10!)
Space: O(1)
```

**Solution Example**:
```
SEND + MORE = MONEY
9567 + 1085 = 10652

S=9, E=5, N=6, D=7
M=1, O=0, R=8, Y=2
```

---

### 5. GENERATE PARENTHESES

**Problem**: Generate all valid combinations of n pairs of parentheses

```
GENERATE_PARENTHESES(n, open, close, current, result):
    // Base case: generated all parentheses
    if current.length == 2 × n:
        result.append(current)
        return
    
    // Add opening parenthesis if available
    if open < n:
        GENERATE_PARENTHESES(n, open+1, close, current + "(", result)
    
    // Add closing parenthesis if valid
    if close < open:
        GENERATE_PARENTHESES(n, open, close+1, current + ")", result)

Time: O(4^n / √n) - Catalan number
Space: O(n)
```

**Example**:
```
n = 3
Output:
"((()))"
"(()())"
"(())()"
"()(())"
"()()()"
```

---

### 6. LETTER COMBINATIONS (Phone Keypad)

**Problem**: Given digit string, return all letter combinations

```
Keypad mapping:
2 → "abc"
3 → "def"
4 → "ghi"
5 → "jkl"
6 → "mno"
7 → "pqrs"
8 → "tuv"
9 → "wxyz"

LETTER_COMBINATIONS(digits, index, current, result, mapping):
    // Base case: processed all digits
    if index == digits.length:
        result.append(current)
        return
    
    currentDigit = digits[index]
    letters = mapping[currentDigit]
    
    // Try each letter for current digit
    for each letter in letters:
        LETTER_COMBINATIONS(digits, index+1, current + letter, result, mapping)

Time: O(4^n) worst case (digit 7 or 9)
Space: O(n)
```

**Example**:
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

---

### 7. RESTORE IP ADDRESSES

**Problem**: Generate all valid IP addresses from string of digits

```
RESTORE_IP(s, start, parts, result):
    // Base case: 4 parts formed
    if parts.size == 4:
        if start == s.length:
            result.append(parts.join("."))
        return
    
    // Try 1, 2, or 3 digit numbers
    for len = 1 to 3:
        if start + len > s.length:
            break
        
        segment = s[start...start+len-1]
        
        // Validate segment
        if IS_VALID_IP_SEGMENT(segment):
            parts.append(segment)
            RESTORE_IP(s, start + len, parts, result)
            parts.removeLast()

IS_VALID_IP_SEGMENT(segment):
    // No leading zeros except "0"
    if segment.length > 1 AND segment[0] == '0':
        return false
    
    // Must be 0-255
    num = parseInt(segment)
    return num >= 0 AND num <= 255

Time: O(3^4) = O(1) - at most 4 parts
Space: O(1)
```

**Example**:
```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```

---

## 💡 PRUNING TECHNIQUES

### 1. Boundary Checking
```
In maze: Check bounds before recursion
Saves unnecessary function calls
```

### 2. Early Exit
```
In word break: If no valid prefix, return early
```

### 3. Constraint Checking
```
In cryptarithmetic: Leading digit ≠ 0
In parentheses: close ≤ open
```

### 4. Memoization
```
Cache results of subproblems
Converts pure backtracking to DP
```

---

## 📊 COMPLEXITY SUMMARY

| Problem | Time Complexity | Space | Notes |
|---------|----------------|-------|-------|
| Rat in Maze | O(2^(n²)) | O(n²) | 2 directions |
| Rat in Maze (4-dir) | O(4^(n²)) | O(n²) | 4 directions |
| Word Break | O(2^n) | O(n) | Can use DP |
| Expression Add Ops | O(4^n) | O(n) | 4 choices per digit |
| Cryptarithmetic | O(10!) | O(1) | 10 digits to assign |
| Generate Parentheses | O(4^n/√n) | O(n) | Catalan number |
| Letter Combinations | O(4^n) | O(n) | Max 4 letters per digit |
| Restore IP | O(1) | O(1) | Fixed 4 parts |

---

## 🚨 COMMON PATTERNS

### Pattern 1: Path Finding (Maze)
```
1. Mark current cell as visited
2. Try all possible moves
3. Unmark when backtracking
```

### Pattern 2: String Partitioning (Word Break)
```
1. Try all possible prefixes
2. Recurse on remaining string
3. Backtrack by removing last choice
```

### Pattern 3: Digit Assignment (Cryptarithmetic)
```
1. Try each unused digit
2. Check validity constraints
3. Backtrack by unmarking digit
```

---

## 📖 MUST REMEMBER

1. **Rat in Maze**: Mark visited, try moves, unmark
2. **Word Break**: Try all prefixes, use memoization for efficiency
3. **Expression**: Handle operator precedence carefully
4. **Cryptarithmetic**: Leading letters ≠ 0, check equation validity
5. **Parentheses**: open ≤ n, close ≤ open
6. **Always restore state** after recursive call
7. **Pruning**: Check constraints before recursion
8. **Base case**: Complete solution or no more choices
9. **Memoization**: Cache when subproblems repeat
10. **Draw recursion tree** for small examples to understand

---

## 🎯 EXAM TIPS

✅ Identify state variables (what changes during recursion)

✅ Define clear base cases (success and failure)

✅ Check validity before making choice (pruning)

✅ Always backtrack (undo changes)

✅ Consider memoization for optimization

✅ Draw small example to verify logic

✅ Count recursive calls to estimate complexity
