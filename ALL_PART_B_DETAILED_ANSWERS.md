# 📚 ALL PART B QUESTIONS - COMPLETE DETAILED ANSWERS
## READY FOR EXAM - NOVEMBER 14, 2025

---

## Q4. (16 MARKS) RABIN-KARP PATTERN MATCHING

### 1. PROBLEM UNDERSTANDING (2 marks)

**Problem:** Alice wants to count occurrences of a word (pattern) in text using Rabin-Karp algorithm with hashing.

**Traditional Approach:** Check character-by-character at each position → O(nm) time

**Rabin-Karp Approach:** Use hash values for fast comparison → O(n+m) average time

### 2. RABIN-KARP CONCEPT (3 marks)

**Core Idea:** Rolling hash function for efficient window sliding

**Hash Function:**
```
hash(s) = (s[0]×d^(m-1) + s[1]×d^(m-2) + ... + s[m-1]) mod q

d = base (e.g., 256 for ASCII)
q = large prime (reduce collisions)
m = pattern length
```

**Rolling Hash Magic:**
```
Old window: [a][b][c]
New window:     [b][c][d]

New_hash = ((Old_hash - a×d^(m-1)) × d + d) mod q

Time: O(1) instead of O(m)!
```

**Spurious Hits:** Hash collision possible → verify when hashes match

### 3. ALGORITHM (5 marks)

```
RABIN_KARP(text, pattern):
1. Initialize:
   m = pattern.length
   n = text.length
   d = 256, q = 101
   
2. Compute pattern hash:
   patternHash = 0
   for i = 0 to m-1:
       patternHash = (d × patternHash + pattern[i]) mod q
   
3. Compute h = d^(m-1) mod q
   
4. Compute first window hash:
   textHash = 0
   for i = 0 to m-1:
       textHash = (d × textHash + text[i]) mod q
   
5. Slide through text:
   count = 0
   for i = 0 to n-m:
       if patternHash == textHash:
           if verify_match(text[i..i+m-1], pattern):
               count++
               print "Found at index", i
       
       if i < n-m:
           textHash = (d×(textHash - text[i]×h) + text[i+m]) mod q
           if textHash < 0:
               textHash += q
   
6. Return count
```

### 4. C++ CODE (4 marks)

```cpp
#include <iostream>
#include <string>
#define d 256  // Number of characters
using namespace std;

void searchPattern(string text, string pattern, int q) {
    int m = pattern.length();
    int n = text.length();
    long long patternHash = 0;
    long long textHash = 0;
    long long h = 1;
    int count = 0;
    
    // Calculate h = d^(m-1) % q
    for (int i = 0; i < m-1; i++)
        h = (h * d) % q;
    
    // Calculate hash values
    for (int i = 0; i < m; i++) {
        patternHash = (d * patternHash + pattern[i]) % q;
        textHash = (d * textHash + text[i]) % q;
    }
    
    // Slide pattern
    for (int i = 0; i <= n-m; i++) {
        if (patternHash == textHash) {
            // Verify character by character
            bool match = true;
            for (int j = 0; j < m; j++) {
                if (text[i+j] != pattern[j]) {
                    match = false;
                    break;
                }
            }
            if (match) {
                cout << "Pattern found at index " << i << endl;
                count++;
            }
        }
        
        // Calculate next window hash
        if (i < n-m) {
            textHash = (d*(textHash - text[i]*h) + text[i+m]) % q;
            if (textHash < 0)
                textHash += q;
        }
    }
    
    cout << "Total occurrences: " << count << endl;
}

int main() {
    string text = "AABAACAADAABAABA";
    string pattern = "AABA";
    int q = 101;  // Prime number
    
    cout << "Text: " << text << endl;
    cout << "Pattern: " << pattern << endl << endl;
    
    searchPattern(text, pattern, q);
    
    return 0;
}
```

### 5. EXAMPLE (2 marks)

**Input:**
```
Text:    "AABAACAADAABAABA"
Pattern: "AABA"
```

**Execution:**
```
d=256, q=101, m=4

Pattern hash = hash("AABA")

Window 0: "AABA"
- Hash matches → Verify → MATCH ✓
- Output: "Pattern found at index 0"

Window 1: "ABAA"  
- Hash ≠ pattern hash → Skip

Window 10: "AABA"
- Hash matches → Verify → MATCH ✓
- Output: "Pattern found at index 10"

Window 13: "AABA"
- Hash matches → Verify → MATCH ✓  
- Output: "Pattern found at index 13"

Total occurrences: 3
```

**Complexity:**
- Average: O(n+m)
- Worst: O(nm) - many spurious hits

---

## Q5. (16 MARKS) RAT IN MAZE

### 1. PROBLEM ANALYSIS (2 marks)

**Problem:** N×N binary maze, rat at [0,0] must reach [N-1,N-1]. Can move only right or down.
- 1 = open cell
- 0 = blocked cell

**Goal:** Find path and mark solution matrix

### 2. BACKTRACKING STRATEGY (4 marks)

**Approach:**
1. Try forward (right): If valid, mark and recurse
2. If blocked, try down: Alternative direction
3. Mark path: sol[i][j] = 1
4. Backtrack: If dead-end, unmark sol[i][j] = 0

**Valid Move:**
- Within bounds
- Cell = 1 (open)
- Not visited

### 3. ALGORITHM (4 marks)

```
SOLVE_MAZE(maze, N):
    sol[N][N] = {0}
    if solve_util(maze, 0, 0, sol, N):
        print sol
        return true
    print "No solution"
    return false

SOLVE_UTIL(maze, x, y, sol, N):
    if x==N-1 && y==N-1 && maze[x][y]==1:
        sol[x][y] = 1
        return true
    
    if is_safe(maze, x, y, sol, N):
        sol[x][y] = 1
        
        if solve_util(maze, x, y+1, sol, N):
            return true
        
        if solve_util(maze, x+1, y, sol, N):
            return true
        
        sol[x][y] = 0  // BACKTRACK
        return false
    
    return false
```

### 4. C++ CODE (4 marks)

```cpp
#include <iostream>
#define N 4
using namespace std;

bool isSafe(int maze[N][N], int x, int y, int sol[N][N]) {
    return (x >= 0 && x < N && y >= 0 && y < N && 
            maze[x][y] == 1 && sol[x][y] == 0);
}

bool solveMazeUtil(int maze[N][N], int x, int y, int sol[N][N]) {
    if (x == N-1 && y == N-1 && maze[x][y] == 1) {
        sol[x][y] = 1;
        return true;
    }
    
    if (isSafe(maze, x, y, sol)) {
        sol[x][y] = 1;
        
        if (solveMazeUtil(maze, x, y+1, sol))
            return true;
        
        if (solveMazeUtil(maze, x+1, y, sol))
            return true;
        
        sol[x][y] = 0;
        return false;
    }
    
    return false;
}

void printSolution(int sol[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++)
            cout << sol[i][j] << " ";
        cout << endl;
    }
}

bool solveMaze(int maze[N][N]) {
    int sol[N][N] = {0};
    
    if (!solveMazeUtil(maze, 0, 0, sol)) {
        cout << "No solution!" << endl;
        return false;
    }
    
    printSolution(sol);
    return true;
}

int main() {
    int maze[N][N] = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {0, 1, 0, 0},
        {1, 1, 1, 1}
    };
    
    solveMaze(maze);
    return 0;
}
```

### 5. EXAMPLE (2 marks)

**Maze:**
```
1 0 0 0
1 1 0 1
0 1 0 0
1 1 1 1
```

**Solution:**
```
1 0 0 0
1 1 0 0
0 1 0 0
0 1 1 1
```

**Path:** [0,0]→[1,0]→[1,1]→[2,1]→[3,1]→[3,2]→[3,3]

---

## Q6. (16 MARKS) SUBSET SUM PROBLEM

### 1. PROBLEM DEFINITION (2 marks)

**Problem:** Given array and target sum, find if subset exists with that sum.

**Example:** arr=[3,5,2,7], target=10 → {3,7} or {3,5,2}

### 2. BACKTRACKING APPROACH (5 marks)

**Decision Tree:**
```
              Start(0)
            /         \
       Include3      Exclude3
        (3)            (0)
       /    \         /    \
    Inc5  Exc5    Inc5  Exc5
    (8)   (3)     (5)   (0)
   / \    / \     / \   / \
  I2 E2  I2 E2   I2 E2 I2 E2
 (10)✓
```

**Algorithm:**
```
SUBSET_SUM(arr, n, target, idx, sum):
    if sum == target:
        return true
    if idx == n || sum > target:
        return false
    
    // Include current
    if subset_sum(arr, n, target, idx+1, sum+arr[idx]):
        return true
    
    // Exclude current
    return subset_sum(arr, n, target, idx+1, sum)
```

### 3. EXAMPLE (4 marks)

**arr=[3,5,2,7], target=10**

Trace:
1. Include 3, sum=3
2. Include 5, sum=8
3. Include 2, sum=10 ✓ Found!

### 4. COMPLEXITY ANALYSIS (3 marks)

**Backtracking:**
- Time: O(2ⁿ)
- Space: O(n)

**DP:**
- Time: O(n×target)
- Space: O(n×target)

**Use DP when:** target small, n large

### 5. CODE OUTLINE (2 marks)

```cpp
bool subsetSum_DP(int arr[], int n, int target) {
    bool dp[n+1][target+1];
    
    for (int i = 0; i <= n; i++) dp[i][0] = true;
    for (int j = 1; j <= target; j++) dp[0][j] = false;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= target; j++) {
            if (arr[i-1] <= j)
                dp[i][j] = dp[i-1][j] || dp[i-1][j-arr[i-1]];
            else
                dp[i][j] = dp[i-1][j];
        }
    }
    
    return dp[n][target];
}
```

---

## Q7. (16 MARKS) LCS WITH OCCURRENCE COUNTING

### 1. PROBLEM ANALYSIS (2 marks)

Dr. Maya needs:
1. Find LCS of proteinA and proteinB
2. Count LCS occurrences in proteinA

### 2. LCS ALGORITHM (5 marks)

**Recurrence:**
```
If X[i-1] == Y[j-1]:
    dp[i][j] = 1 + dp[i-1][j-1]
Else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

**Example:**
```
X = "AGGTAB"
Y = "GXTXAYB"

DP Table:
    ""  G  X  T  X  A  Y  B
""   0  0  0  0  0  0  0  0
A    0  0  0  0  0  1  1  1
G    0  1  1  1  1  1  1  1
G    0  1  1  1  1  1  1  1
T    0  1  1  2  2  2  2  2
A    0  1  1  2  2  3  3  3
B    0  1  1  2  2  3  3  4

LCS = "GTAB" (length 4)
```

### 3. COUNTING OCCURRENCES (4 marks)

**Algorithm:**
```
COUNT_SUBSEQUENCES(text, pattern):
    count[m+1][n+1]
    
    for i = 0 to m:
        count[i][0] = 1
    
    for i = 1 to m:
        for j = 1 to n:
            count[i][j] = count[i-1][j]
            if text[i-1] == pattern[j-1]:
                count[i][j] += count[i-1][j-1]
    
    return count[m][n]
```

### 4. C++ CODE (4 marks)

```cpp
string findLCS(string X, string Y) {
    int m = X.length(), n = Y.length();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (X[i-1] == Y[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    // Backtrack to find LCS
    int i = m, j = n;
    string lcs = "";
    while (i > 0 && j > 0) {
        if (X[i-1] == Y[j-1]) {
            lcs = X[i-1] + lcs;
            i--; j--;
        } else if (dp[i-1][j] > dp[i][j-1])
            i--;
        else
            j--;
    }
    return lcs;
}

int countOccurrences(string text, string pattern) {
    int m = text.length(), n = pattern.length();
    vector<vector<int>> count(m+1, vector<int>(n+1, 0));
    
    for (int i = 0; i <= m; i++)
        count[i][0] = 1;
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            count[i][j] = count[i-1][j];
            if (text[i-1] == pattern[j-1])
                count[i][j] += count[i-1][j-1];
        }
    }
    return count[m][n];
}
```

### 5. COMPLEXITY (1 mark)

Time: O(m×n), Space: O(m×n)

---

## Q11. (16 MARKS) ALGORITHM DESIGN TECHNIQUES

### GREEDY vs DP (8 marks)

**Greedy:**
- Local optimal choice
- May fail
- Fast O(n log n)
- Example: Activity Selection ✓, 0/1 Knapsack ✗

**DP:**
- Try all possibilities
- Guaranteed optimal
- Slower O(n²)
- Example: 0/1 Knapsack ✓

**Comparison:**
```
Problem: 0/1 Knapsack
Items: (5,50), (6,60), (4,40)
Capacity: 10

Greedy (by ratio): 50/5=10, 60/6=10, 40/4=10
Pick (5)+( 4) = 90

DP: Try all → (6)+(4) = 100 ✓
```

### D&C vs BACKTRACKING (8 marks)

**Divide & Conquer:**
- Split into independent subproblems
- No backtracking
- Example: Merge Sort

**Backtracking:**
- Try choices, undo if fail
- Explores solution space
- Example: N-Queens

**Visual:**
```
D&C: Split → Solve → Combine
Backtracking: Try → Fail → Undo → Try Next
```

---

## Q12. (10 MARKS) TIME COMPLEXITY

### 1. BIG-O CONCEPT (2 marks)

Upper bound of growth rate
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ)
```

### 2. BINARY SEARCH (2 marks)

**Algorithm:** Halve search space each iteration
```
n → n/2 → n/4 → ... → 1
k iterations: n/2^k = 1
k = log₂(n)
```
**Time: O(log n)**

### 3. MERGE SORT (3 marks)

**Recurrence:** T(n) = 2T(n/2) + O(n)
```
Tree height: log n
Work per level: n
Total: n × log n
```
**Time: O(n log n)**

### 4. MATRIX MULTIPLICATION (3 marks)

**Three nested loops:**
```cpp
for i: n times
    for j: n times
        for k: n times
            C[i][j] += A[i][k] * B[k][j]
```
**Time: O(n³)**

---

**All answers include:**
✅ Detailed explanations
✅ Complete C++ code
✅ Step-by-step examples
✅ Complexity analysis
✅ Proper mark distribution

**Best of luck for your exam tomorrow! 🎯📚**
