# Time Complexity Analysis & Recursion

## 📌 KEY DEFINITIONS

**Time Complexity**: Measure of amount of time an algorithm takes as a function of input size

**Space Complexity**: Amount of memory an algorithm uses as a function of input size

**Asymptotic Notation**: Mathematical way to describe algorithm's efficiency
- **Big O (O)**: Upper bound (worst case)
- **Omega (Ω)**: Lower bound (best case)  
- **Theta (Θ)**: Tight bound (average case)

**Recursion**: Function that calls itself with smaller input until base case is reached

---

## ⏱️ TIME COMPLEXITY RANKINGS (BEST TO WORST)

```
O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

### Common Complexities with Examples

| Complexity | Name | Example |
|------------|------|---------|
| **O(1)** | Constant | Array access, hash table lookup |
| **O(log n)** | Logarithmic | Binary search, balanced BST operations |
| **O(n)** | Linear | Single loop, linear search |
| **O(n log n)** | Linearithmic | Merge sort, quick sort (avg), heap sort |
| **O(n²)** | Quadratic | Nested loops, bubble sort, selection sort |
| **O(2ⁿ)** | Exponential | Recursive fibonacci, subset generation |
| **O(n!)** | Factorial | Permutation generation, TSP brute force |

---

## 🔁 RECURSION COMPONENTS

### 1. Base Case
- **Definition**: Stopping condition that doesn't call itself
- **Purpose**: Prevents infinite recursion
- **Must Have**: Every recursive function needs at least one

### 2. Recursive Case
- **Definition**: Function calls itself with modified parameters
- **Purpose**: Breaks problem into smaller subproblems
- **Progress**: Must move towards base case

### 3. Return Statement
- Combines results from recursive calls

---

## 📝 PSEUDOCODE EXAMPLES

### Example 1: Factorial (Classic Recursion)
```
FACTORIAL(n):
    // Base case
    if n == 0 or n == 1:
        return 1
    
    // Recursive case
    return n * FACTORIAL(n - 1)

Time: O(n)
Space: O(n) - due to call stack
```

### Example 2: Fibonacci (Inefficient Recursion)
```
FIBONACCI(n):
    // Base cases
    if n == 0:
        return 0
    if n == 1:
        return 1
    
    // Recursive case
    return FIBONACCI(n-1) + FIBONACCI(n-2)

Time: O(2ⁿ) - exponential!
Space: O(n) - maximum call stack depth
```

### Example 3: Binary Search (Efficient Recursion)
```
BINARY_SEARCH(arr, target, left, right):
    // Base case - not found
    if left > right:
        return -1
    
    mid = (left + right) / 2
    
    // Base case - found
    if arr[mid] == target:
        return mid
    
    // Recursive cases
    if arr[mid] > target:
        return BINARY_SEARCH(arr, target, left, mid-1)
    else:
        return BINARY_SEARCH(arr, target, mid+1, right)

Time: O(log n)
Space: O(log n) - call stack
```

---

## 📊 RECURRENCE RELATIONS

**Definition**: Equation that defines sequence based on previous terms

### Common Recurrence Forms

1. **Linear**: T(n) = T(n-1) + O(1)
   - Example: Factorial
   - Solution: O(n)

2. **Binary Tree**: T(n) = 2T(n/2) + O(1)
   - Example: Binary tree traversal
   - Solution: O(n)

3. **Divide & Conquer**: T(n) = 2T(n/2) + O(n)
   - Example: Merge sort
   - Solution: O(n log n)

4. **Multiple Branches**: T(n) = T(n-1) + T(n-2) + O(1)
   - Example: Fibonacci
   - Solution: O(2ⁿ)

---

## 🎯 MASTER THEOREM

**Definition**: Solves recurrences of form: **T(n) = aT(n/b) + f(n)**

Where:
- **a** ≥ 1: Number of subproblems
- **b** > 1: Factor by which problem size is reduced
- **f(n)**: Cost of work outside recursive calls

### Three Cases

**Case 1**: If f(n) = O(n^c) where c < log_b(a)
- **Result**: T(n) = Θ(n^(log_b(a)))

**Case 2**: If f(n) = Θ(n^c × log^k(n)) where c = log_b(a)
- **Result**: T(n) = Θ(n^c × log^(k+1)(n))

**Case 3**: If f(n) = Ω(n^c) where c > log_b(a) and regularity condition holds
- **Result**: T(n) = Θ(f(n))

### Examples Using Master Theorem

```
1. T(n) = 2T(n/2) + O(n)        [Merge Sort]
   a=2, b=2, f(n)=n
   log₂(2) = 1, so Case 2
   Result: O(n log n)

2. T(n) = 4T(n/2) + O(n)        [Strassen's Matrix]
   a=4, b=2, f(n)=n
   log₂(4) = 2 > 1, so Case 1
   Result: O(n²)

3. T(n) = T(n/2) + O(1)         [Binary Search]
   a=1, b=2, f(n)=1
   log₂(1) = 0, so Case 2
   Result: O(log n)
```

---

## 🎓 ANALYSIS TECHNIQUES

### 1. Counting Operations
```
for i = 1 to n:           // n times
    for j = 1 to n:       // n times
        print(i, j)       // O(1)

Total: n × n × 1 = O(n²)
```

### 2. Recursive Analysis
```
Draw recursion tree
Count nodes at each level
Sum total work
```

### 3. Amortized Analysis
Average cost per operation over sequence of operations

---

## 💡 QUICK TIPS FOR EXAM

✅ **Nested loops**: Multiply complexities (usually O(n²))

✅ **Sequential loops**: Add complexities (take larger, e.g., O(n) + O(n²) = O(n²))

✅ **Recursive calls**: Draw tree or use Master Theorem

✅ **Logarithmic**: Dividing problem size (binary search, balanced trees)

✅ **Exponential**: Multiple recursive calls (fibonacci, subsets)

✅ **Space complexity**: Consider call stack depth + auxiliary space

---

## 🚨 COMMON MISTAKES TO AVOID

❌ Forgetting base case in recursion → infinite loop

❌ Confusing O(n) + O(n) with O(n²)

❌ Ignoring space complexity of recursion call stack

❌ Wrong Master Theorem case selection

---

## 📖 MUST REMEMBER

1. **O(log n)** appears when you divide problem size (binary search)
2. **O(n log n)** appears in efficient sorting (merge, heap, quick sort average)
3. **Recursive space** = Maximum depth of call stack
4. **Tail recursion** can be optimized to iterative (O(1) space)
5. Always look for **base case first** when analyzing recursion
