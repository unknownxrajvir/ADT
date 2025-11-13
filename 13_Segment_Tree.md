# Range Query Algorithms - Segment Tree

## 📌 KEY DEFINITIONS

**Range Query**: Query that asks about a range of elements (e.g., sum, min, max)

**Segment Tree**: Binary tree data structure for storing intervals/segments

**Lazy Propagation**: Technique to defer updates to children nodes

**Point Update**: Update single element

**Range Update**: Update all elements in a range

---

## 🌲 SEGMENT TREE STRUCTURE

### Concept
- Each node represents an interval [L, R]
- Leaf nodes represent single elements
- Internal nodes represent union of children's intervals
- Parent covers [L, R], left child [L, M], right child [M+1, R]

### Array Representation
```
For array of size n:
- Tree size = 4n (worst case)
- Root at index 1
- Left child of node i: 2i
- Right child of node i: 2i + 1
- Parent of node i: i/2
```

**Visual Example**:
```
Array: [1, 3, 5, 7, 9, 11]
Segment Tree (sum):

           36[0-5]
          /        \
       9[0-2]      36[3-5]
       /    \      /      \
    4[0-1] 5[2]  16[3-4]  11[5]
    /   \         /    \
  1[0] 3[1]     7[3]  9[4]
```

---

## 📝 SEGMENT TREE OPERATIONS

### 1. BUILD TREE

```
BUILD(arr, tree, node, start, end):
    // Base case: leaf node
    if start == end:
        tree[node] = arr[start]
        return
    
    mid = (start + end) / 2
    leftChild = 2 * node
    rightChild = 2 * node + 1
    
    // Recursively build left and right subtrees
    BUILD(arr, tree, leftChild, start, mid)
    BUILD(arr, tree, rightChild, mid+1, end)
    
    // Internal node stores sum of both children
    tree[node] = tree[leftChild] + tree[rightChild]

Time: O(n)
Space: O(4n) = O(n)
```

**Example**:
```
arr = [1, 3, 5, 7, 9, 11]

tree[1] = 36  // Root [0-5]
tree[2] = 9   // Left [0-2]
tree[3] = 27  // Right [3-5]
tree[4] = 4   // [0-1]
tree[5] = 5   // [2]
tree[6] = 16  // [3-4]
tree[7] = 11  // [5]
```

---

### 2. RANGE QUERY (Sum/Min/Max)

```
QUERY(tree, node, start, end, L, R):
    // No overlap
    if R < start OR end < L:
        return 0  // or INFINITY for min, -INFINITY for max
    
    // Complete overlap
    if L <= start AND end <= R:
        return tree[node]
    
    // Partial overlap
    mid = (start + end) / 2
    leftSum = QUERY(tree, 2*node, start, mid, L, R)
    rightSum = QUERY(tree, 2*node+1, mid+1, end, L, R)
    
    return leftSum + rightSum

Time: O(log n)
```

**Query Types**:
```
// Range Sum Query
return leftSum + rightSum

// Range Min Query
return min(leftMin, rightMin)

// Range Max Query
return max(leftMax, rightMax)

// Range GCD Query
return gcd(leftGCD, rightGCD)
```

**Example**:
```
arr = [1, 3, 5, 7, 9, 11]
Query: sum(1, 4) = 3 + 5 + 7 + 9 = 24

Tree traversal:
- Start at root [0-5]
- Partial overlap, split
- Left [0-2]: Partial overlap, split further
- Right [3-5]: Partial overlap
- Collect results from relevant nodes
```

---

### 3. POINT UPDATE

```
UPDATE(tree, node, start, end, index, value):
    // Base case: leaf node
    if start == end:
        tree[node] = value
        return
    
    mid = (start + end) / 2
    
    // Update left or right subtree
    if index <= mid:
        UPDATE(tree, 2*node, start, mid, index, value)
    else:
        UPDATE(tree, 2*node+1, mid+1, end, index, value)
    
    // Update current node
    tree[node] = tree[2*node] + tree[2*node+1]

Time: O(log n)
```

**Example**:
```
arr = [1, 3, 5, 7, 9, 11]
Update arr[2] = 10

Affected nodes:
tree[5] = 10   // Leaf [2]
tree[2] = 14   // Parent [0-2]
tree[1] = 41   // Root [0-5]
```

---

### 4. LAZY PROPAGATION (Range Update)

**Problem**: Update all elements in range efficiently

**Idea**: Defer updates to children until necessary

```
CLASS SegmentTree:
    tree[]  // Store values
    lazy[]  // Store pending updates

RANGE_UPDATE_LAZY(tree, lazy, node, start, end, L, R, value):
    // Apply pending update
    if lazy[node] != 0:
        tree[node] += (end - start + 1) * lazy[node]
        
        // Propagate to children if not leaf
        if start != end:
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        
        lazy[node] = 0
    
    // No overlap
    if start > R OR end < L:
        return
    
    // Complete overlap
    if L <= start AND end <= R:
        tree[node] += (end - start + 1) * value
        
        // Mark children as lazy
        if start != end:
            lazy[2*node] += value
            lazy[2*node+1] += value
        
        return
    
    // Partial overlap
    mid = (start + end) / 2
    RANGE_UPDATE_LAZY(tree, lazy, 2*node, start, mid, L, R, value)
    RANGE_UPDATE_LAZY(tree, lazy, 2*node+1, mid+1, end, L, R, value)
    
    tree[node] = tree[2*node] + tree[2*node+1]

Time: O(log n)
```

**Query with Lazy Propagation**:
```
QUERY_LAZY(tree, lazy, node, start, end, L, R):
    // Apply pending update first
    if lazy[node] != 0:
        tree[node] += (end - start + 1) * lazy[node]
        
        if start != end:
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        
        lazy[node] = 0
    
    // No overlap
    if start > R OR end < L:
        return 0
    
    // Complete overlap
    if L <= start AND end <= R:
        return tree[node]
    
    // Partial overlap
    mid = (start + end) / 2
    leftSum = QUERY_LAZY(tree, lazy, 2*node, start, mid, L, R)
    rightSum = QUERY_LAZY(tree, lazy, 2*node+1, mid+1, end, L, R)
    
    return leftSum + rightSum

Time: O(log n)
```

---

## 💡 SEGMENT TREE VARIANTS

### 1. Range Minimum Query (RMQ)
```
BUILD_MIN(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
        return
    
    mid = (start + end) / 2
    BUILD_MIN(arr, tree, 2*node, start, mid)
    BUILD_MIN(arr, tree, 2*node+1, mid+1, end)
    
    tree[node] = min(tree[2*node], tree[2*node+1])

QUERY_MIN(tree, node, start, end, L, R):
    if R < start OR end < L:
        return INFINITY
    
    if L <= start AND end <= R:
        return tree[node]
    
    mid = (start + end) / 2
    return min(QUERY_MIN(tree, 2*node, start, mid, L, R),
               QUERY_MIN(tree, 2*node+1, mid+1, end, L, R))
```

### 2. Range Maximum Query
```
Similar to RMQ, but use max() instead of min()
Return -INFINITY for no overlap
```

### 3. Range GCD Query
```
tree[node] = gcd(tree[2*node], tree[2*node+1])
Return 0 for no overlap
```

### 4. Count Elements in Range
```
Store count of elements satisfying condition
Useful for counting numbers less than x in range
```

---

## 🎯 APPLICATIONS

### 1. Range Sum Query with Updates
```
Problem: Given array, support:
- Update single element
- Query sum of range

Solution: Basic segment tree
Operations: O(log n) each
```

### 2. Range Minimum/Maximum Query
```
Problem: Find min/max in range with updates

Solution: Min/Max segment tree
Operations: O(log n) each
```

### 3. Range Update Range Query
```
Problem: Add value to all elements in range, query sum

Solution: Lazy propagation
Operations: O(log n) each
```

### 4. Count Inversions
```
Problem: Count pairs (i,j) where i<j and arr[i]>arr[j]

Solution: Use segment tree while merging
Operations: O(n log n)
```

### 5. Kth Smallest Element in Range
```
Problem: Find kth smallest in range [L, R]

Solution: Merge sort tree (segment tree + sorted subarrays)
Operations: O(log²n)
```

---

## 📊 COMPLETE IMPLEMENTATION

```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.lazy = [0] * (4 * self.n)
        self.build(arr, 1, 0, self.n - 1)
    
    def build(self, arr, node, start, end):
        if start == end:
            self.tree[node] = arr[start]
            return
        
        mid = (start + end) // 2
        self.build(arr, 2*node, start, mid)
        self.build(arr, 2*node+1, mid+1, end)
        self.tree[node] = self.tree[2*node] + self.tree[2*node+1]
    
    def update_lazy(self, node, start, end):
        if self.lazy[node] != 0:
            self.tree[node] += (end - start + 1) * self.lazy[node]
            if start != end:
                self.lazy[2*node] += self.lazy[node]
                self.lazy[2*node+1] += self.lazy[node]
            self.lazy[node] = 0
    
    def range_update(self, node, start, end, L, R, value):
        self.update_lazy(node, start, end)
        
        if start > R or end < L:
            return
        
        if L <= start and end <= R:
            self.tree[node] += (end - start + 1) * value
            if start != end:
                self.lazy[2*node] += value
                self.lazy[2*node+1] += value
            return
        
        mid = (start + end) // 2
        self.range_update(2*node, start, mid, L, R, value)
        self.range_update(2*node+1, mid+1, end, L, R, value)
        self.tree[node] = self.tree[2*node] + self.tree[2*node+1]
    
    def query(self, node, start, end, L, R):
        self.update_lazy(node, start, end)
        
        if start > R or end < L:
            return 0
        
        if L <= start and end <= R:
            return self.tree[node]
        
        mid = (start + end) // 2
        return (self.query(2*node, start, mid, L, R) +
                self.query(2*node+1, mid+1, end, L, R))
```

---

## 📖 COMPLEXITY ANALYSIS

| Operation | Time | Space | Notes |
|-----------|------|-------|-------|
| **Build** | O(n) | O(4n) | One-time construction |
| **Query** | O(log n) | O(log n) | Recursion stack |
| **Point Update** | O(log n) | O(log n) | Update path to root |
| **Range Update** | O(log n) | O(4n) | With lazy propagation |
| **Space** | O(n) | - | 4n worst case |

---

## 🚨 COMMON MISTAKES

❌ Not handling lazy propagation before query

❌ Wrong tree size (use 4n, not 2n)

❌ Forgetting to update parent nodes after point update

❌ Wrong overlap conditions in query

❌ Not propagating lazy values to children

---

## 📖 MUST REMEMBER

1. **Tree size**: 4n for safety (2*2^⌈log n⌉)
2. **Build**: O(n) - bottom-up construction
3. **Query**: O(log n) - three cases: no overlap, complete, partial
4. **Point update**: O(log n) - update path from leaf to root
5. **Range update**: O(log n) with lazy propagation
6. **Lazy propagation**: Defer updates, apply when needed
7. **Node indexing**: Left = 2i, Right = 2i+1, Parent = i/2
8. **Overlap types**: None, complete, partial
9. **Merge function**: Sum, min, max, gcd, etc.
10. **Always update lazy** before processing node

---

## 🎯 EXAM TIPS

✅ Draw small tree to visualize structure

✅ Trace query/update on example

✅ Remember 3 overlap cases

✅ Lazy propagation: update before use

✅ Know when segment tree is needed (range queries + updates)

✅ Practice identifying merge function for different problems

✅ Understand why O(log n): tree height = log n
