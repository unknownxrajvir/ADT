# Heaps

## 📌 KEY DEFINITIONS

**Heap**: Complete binary tree that satisfies the heap property

**Complete Binary Tree**: All levels filled except possibly last, which is filled left to right

**Max Heap Property**: Parent node ≥ All children nodes

**Min Heap Property**: Parent node ≤ All children nodes

**Heapify**: Process of converting a binary tree into a heap

---

## 🏗️ HEAP STRUCTURE

### Array Representation (0-indexed)
```
For node at index i:
- Parent: (i-1)/2
- Left Child: 2i + 1
- Right Child: 2i + 2
```
### Array Representation (1-indexed)

```

For node at index i:
- Parent: i/2
- Left Child: 2i
- Right Child: 2i + 1
```

### Visual Example
```
       10                Array: [10, 7, 9, 3, 5, 8, 2]
      /  \               Index:   0  1  2  3  4  5  6
     7    9
    / \  / \
   3  5 8  2
```

---

## 📝 PSEUDOCODE - CORE OPERATIONS

### 1. HEAPIFY DOWN (Bubble Down) - O(log n)
```
HEAPIFY_DOWN(arr, n, i):
    // For Max Heap
    largest = i
    left = 2*i + 1
    right = 2*i + 2
    
    // Check if left child is larger
    if left < n AND arr[left] > arr[largest]:
        largest = left
    
    // Check if right child is larger
    if right < n AND arr[right] > arr[largest]:
        largest = right
    
    // If largest is not root
    if largest != i:
        swap(arr[i], arr[largest])
        HEAPIFY_DOWN(arr, n, largest)  // Recursively heapify

Time: O(log n)
Space: O(log n) for recursion stack
```

### 2. HEAPIFY UP (Bubble Up) - O(log n)
```
HEAPIFY_UP(arr, i):
    // For Max Heap
    parent = (i-1) / 2
    
    // If current node is greater than parent
    if i > 0 AND arr[i] > arr[parent]:
        swap(arr[i], arr[parent])
        HEAPIFY_UP(arr, parent)  // Recursively heapify up

Time: O(log n)
Space: O(log n) for recursion stack
```

### 3. BUILD HEAP - O(n)
```
BUILD_MAX_HEAP(arr, n):
    // Start from last non-leaf node
    // Last non-leaf node: (n/2 - 1)
    
    for i = n/2 - 1 down to 0:
        HEAPIFY_DOWN(arr, n, i)

Time: O(n) - NOT O(n log n)!
Space: O(1) auxiliary
```

**Why O(n)?** Most nodes are leaves, only few at top need full heapify

### 4. INSERT - O(log n)
```
INSERT(heap, key):
    // Add new element at end
    heap.size = heap.size + 1
    heap[heap.size - 1] = key
    
    // Heapify up from last position
    HEAPIFY_UP(heap, heap.size - 1)

Time: O(log n)
Space: O(1)
```

### 5. EXTRACT MAX/MIN - O(log n)
```
EXTRACT_MAX(heap):
    if heap.size < 1:
        return ERROR
    
    // Store max value (root)
    max = heap[0]
    
    // Replace root with last element
    heap[0] = heap[heap.size - 1]
    heap.size = heap.size - 1
    
    // Heapify down from root
    HEAPIFY_DOWN(heap, heap.size, 0)
    
    return max

Time: O(log n)
Space: O(1)
```

### 6. GET MAX/MIN - O(1)
```
GET_MAX(heap):
    if heap.size < 1:
        return ERROR
    return heap[0]

Time: O(1)
Space: O(1)
```

### 7. DELETE - O(log n)
```
DELETE(heap, i):
    // Replace element with last element
    heap[i] = heap[heap.size - 1]
    heap.size = heap.size - 1
    
    // Heapify (may need up or down)
    parent = (i-1) / 2
    if i > 0 AND heap[i] > heap[parent]:
        HEAPIFY_UP(heap, i)
    else:
        HEAPIFY_DOWN(heap, heap.size, i)

Time: O(log n)
```

---

## 📊 HEAP SORT - O(n log n)

```
HEAP_SORT(arr, n):
    // Step 1: Build max heap
    BUILD_MAX_HEAP(arr, n)
    
    // Step 2: Extract elements one by one
    for i = n-1 down to 1:
        // Move current root to end
        swap(arr[0], arr[i])
        
        // Heapify reduced heap
        HEAPIFY_DOWN(arr, i, 0)

Time: O(n log n)
Space: O(1) - in-place sorting
Stable: No
```

---

## 🎯 OPERATIONS COMPLEXITY TABLE

| Operation | Time Complexity | Space |
|-----------|----------------|-------|
| **Build Heap** | O(n) | O(1) |
| **Insert** | O(log n) | O(1) |
| **Extract Max/Min** | O(log n) | O(1) |
| **Get Max/Min** | O(1) | O(1) |
| **Delete** | O(log n) | O(1) |
| **Search** | O(n) | O(1) |
| **Heapify** | O(log n) | O(log n) |
| **Heap Sort** | O(n log n) | O(1) |

---

## 💡 APPLICATIONS

### 1. Priority Queue
```
PRIORITY_QUEUE using Max Heap:
- enqueue(x): INSERT(heap, x)
- dequeue(): EXTRACT_MAX(heap)
- peek(): GET_MAX(heap)
```

### 2. Kth Largest Element
```
FIND_KTH_LARGEST(arr, k):
    // Build min heap of size k
    minHeap = BUILD_MIN_HEAP(arr[0...k-1])
    
    // Process remaining elements
    for i = k to n-1:
        if arr[i] > minHeap[0]:
            minHeap[0] = arr[i]
            HEAPIFY_DOWN(minHeap, k, 0)
    
    return minHeap[0]  // Root is kth largest

Time: O(n log k)
```

### 3. Merge K Sorted Arrays
```
MERGE_K_SORTED(arrays):
    minHeap = empty
    
    // Insert first element of each array with array index
    for i = 0 to k-1:
        INSERT(minHeap, {arrays[i][0], i, 0})
    
    result = []
    while minHeap is not empty:
        node = EXTRACT_MIN(minHeap)
        result.append(node.value)
        
        // Insert next element from same array
        if node.elementIndex + 1 < arrays[node.arrayIndex].size:
            INSERT(minHeap, {
                arrays[node.arrayIndex][node.elementIndex + 1],
                node.arrayIndex,
                node.elementIndex + 1
            })
    
    return result

Time: O(n log k) where n = total elements
```

### 4. Running Median
```
Use two heaps:
- Max heap for smaller half
- Min heap for larger half

Median is either:
- Root of max heap (if odd)
- Average of both roots (if even)
```

---

## 🔥 IMPORTANT POINTS FOR EXAM

✅ **Heap is NOT a sorted structure** - only root is guaranteed max/min

✅ **Build heap is O(n)**, not O(n log n) - due to mathematical analysis

✅ **Complete binary tree** - all levels filled except last (filled left to right)

✅ **Array representation** - no pointers needed, space efficient

✅ **In-place heap sort** - O(1) extra space

✅ **Not stable** - heap sort doesn't preserve relative order

✅ **Height = log n** - for complete binary tree with n nodes

---

## 🚨 COMMON MISTAKES

❌ Thinking heap is sorted → Only parent-child relationship matters

❌ Confusing heapify (O(log n)) with build heap (O(n))

❌ Wrong index calculations for parent/children

❌ Forgetting heap is 0-indexed or 1-indexed

---

## 📖 MUST REMEMBER FOR EXAM

1. **Max Heap**: Root is maximum, used for ascending heap sort
2. **Min Heap**: Root is minimum, used for priority queues
3. **Build Heap**: O(n) - bottom-up approach
4. **Insert/Delete**: O(log n) - height of tree
5. **Extract Max/Min**: O(log n) - remove root and heapify
6. **Get Max/Min**: O(1) - just return root
7. **Heap Sort**: O(n log n) time, O(1) space, NOT stable
8. **Applications**: Priority queue, K largest/smallest, median, merge K sorted
