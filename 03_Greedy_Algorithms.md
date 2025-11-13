# Greedy Algorithms

## 📌 KEY DEFINITIONS

**Greedy Algorithm**: Makes locally optimal choice at each step, hoping to find global optimum

**Greedy Choice Property**: Local optimum leads to global optimum

**Optimal Substructure**: Optimal solution contains optimal solutions to subproblems

**Matroid**: Mathematical structure where greedy works (theory)

---

## 🎯 GREEDY vs DYNAMIC PROGRAMMING

| Greedy | Dynamic Programming |
|--------|---------------------|
| Makes choice without looking ahead | Considers all possibilities |
| Never reconsiders choices | May reconsider via subproblems |
| Usually faster - O(n log n) | Usually slower - O(n²) or more |
| Doesn't always work | Always finds optimal if applicable |
| Example: Fractional Knapsack | Example: 0/1 Knapsack |

---

## 📝 CLASSIC GREEDY PROBLEMS

### 1. ACTIVITY SELECTION PROBLEM

**Problem**: Select maximum non-overlapping activities

**Greedy Choice**: Pick activity that finishes earliest

```
ACTIVITY_SELECTION(start[], finish[], n):
    // Sort by finish time
    sort activities by finish time
    
    selected = [0]  // First activity
    lastFinish = finish[0]
    
    for i = 1 to n-1:
        if start[i] >= lastFinish:
            selected.append(i)
            lastFinish = finish[i]
    
    return selected

Time: O(n log n) - sorting
Space: O(1)
```

**Example**:
```
Activities: (1,3), (2,5), (4,7), (1,8), (5,9), (8,10)
Sorted by finish: (1,3), (2,5), (4,7), (1,8), (5,9), (8,10)

Select: (1,3) → (4,7) → (8,10)
Answer: 3 activities
```

---

### 2. FRACTIONAL KNAPSACK

**Problem**: Maximize value with weight limit (can take fractions)

**Greedy Choice**: Pick item with highest value/weight ratio

```
FRACTIONAL_KNAPSACK(values[], weights[], W, n):
    // Calculate value per unit weight
    for i = 0 to n-1:
        ratio[i] = values[i] / weights[i]
    
    // Sort by ratio (descending)
    sort items by ratio in decreasing order
    
    totalValue = 0
    remainingWeight = W
    
    for i = 0 to n-1:
        if weights[i] <= remainingWeight:
            // Take full item
            totalValue += values[i]
            remainingWeight -= weights[i]
        else:
            // Take fraction
            totalValue += ratio[i] * remainingWeight
            break
    
    return totalValue

Time: O(n log n)
Space: O(n)
```

---

### 3. HUFFMAN CODING

**Problem**: Optimal prefix-free binary codes for data compression

**Greedy Choice**: Combine two least frequent symbols

```
HUFFMAN_CODING(freq[], n):
    // Create min heap with frequencies
    minHeap = BUILD_MIN_HEAP(freq)
    
    for i = 1 to n-1:
        // Extract two minimum frequency nodes
        left = EXTRACT_MIN(minHeap)
        right = EXTRACT_MIN(minHeap)
        
        // Create new internal node
        newNode.freq = left.freq + right.freq
        newNode.left = left
        newNode.right = right
        
        INSERT(minHeap, newNode)
    
    // Root contains the Huffman tree
    return EXTRACT_MIN(minHeap)

Time: O(n log n)
Space: O(n)
```

**Example**:
```
Chars: a(5), b(9), c(12), d(13), e(16), f(45)

Step 1: Combine a(5) + b(9) = 14
Step 2: Combine c(12) + d(13) = 25
Step 3: Combine 14 + e(16) = 30
Step 4: Combine 25 + 30 = 55
Step 5: Combine f(45) + 55 = 100

Codes: f=0, c=100, d=101, a=1100, b=1101, e=111
```

---

### 4. JOB SEQUENCING WITH DEADLINES

**Problem**: Maximize profit by scheduling jobs before deadlines

**Greedy Choice**: Pick job with maximum profit that fits

```
JOB_SEQUENCING(profits[], deadlines[], n):
    // Sort jobs by profit (descending)
    sort jobs by profit in decreasing order
    
    maxDeadline = max(deadlines)
    slots = array of size maxDeadline, initialized to -1
    totalProfit = 0
    
    for i = 0 to n-1:
        // Find free slot before deadline
        for j = min(deadlines[i], maxDeadline) - 1 down to 0:
            if slots[j] == -1:
                slots[j] = i
                totalProfit += profits[i]
                break
    
    return totalProfit

Time: O(n²) or O(n log n) with Union-Find
Space: O(n)
```

**Example**:
```
Jobs: (profit, deadline)
J1(20,2), J2(15,2), J3(10,1), J4(5,3), J5(1,3)

Sorted by profit: J1, J2, J3, J4, J5

J1(20,2): Schedule at slot 1 → [_, J1, _]
J2(15,2): Schedule at slot 0 → [J2, J1, _]
J3(10,1): Can't fit before deadline 1
J4(5,3): Schedule at slot 2 → [J2, J1, J4]
J5(1,3): Can't fit

Total Profit = 20 + 15 + 5 = 40
```

---

### 5. MINIMUM SPANNING TREE (MST)

#### A. Kruskal's Algorithm

**Greedy Choice**: Pick minimum weight edge that doesn't form cycle

```
KRUSKAL_MST(graph):
    // Sort edges by weight
    sort edges by weight (ascending)
    
    mst = []
    parent = MAKE_SET for all vertices  // Union-Find
    
    for each edge (u, v, weight) in sorted order:
        // Check if u and v are in different sets
        if FIND(u) != FIND(v):
            mst.append(edge)
            UNION(u, v)
            
            if mst.size == V - 1:
                break
    
    return mst

Time: O(E log E) or O(E log V)
Space: O(V)
```

#### B. Prim's Algorithm

**Greedy Choice**: Pick minimum weight edge connecting tree to non-tree vertex

```
PRIM_MST(graph, start):
    visited = {start}
    mst = []
    minHeap = all edges from start
    
    while visited.size < V:
        // Get minimum weight edge
        edge = EXTRACT_MIN(minHeap)
        u, v, weight = edge
        
        if v not in visited:
            visited.add(v)
            mst.append(edge)
            
            // Add all edges from v
            for each edge from v:
                if other end not in visited:
                    INSERT(minHeap, edge)
    
    return mst

Time: O(E log V) with binary heap
Space: O(V + E)
```

---

### 6. DIJKSTRA'S SHORTEST PATH

**Problem**: Find shortest path from source to all vertices (non-negative weights)

**Greedy Choice**: Pick unvisited vertex with minimum distance

```
DIJKSTRA(graph, source):
    dist[] = INFINITY for all vertices
    dist[source] = 0
    visited = empty set
    minHeap = {(0, source)}
    
    while minHeap is not empty:
        d, u = EXTRACT_MIN(minHeap)
        
        if u in visited:
            continue
        
        visited.add(u)
        
        // Relax all adjacent edges
        for each edge (u, v, weight):
            if dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                INSERT(minHeap, (dist[v], v))
    
    return dist

Time: O((V + E) log V) with min heap
Space: O(V)
```

---

### 7. COIN CHANGE (GREEDY - MAY NOT WORK)

**Problem**: Make change with minimum coins

**Greedy Choice**: Use largest coin first

```
COIN_CHANGE_GREEDY(coins[], amount):
    sort coins in decreasing order
    count = 0
    
    for each coin in coins:
        while amount >= coin:
            amount -= coin
            count++
    
    return count

Time: O(n log n + amount/smallest_coin)
```

⚠️ **WARNING**: Greedy doesn't always work!
- Works for standard coins: {1, 5, 10, 25}
- Fails for: {1, 3, 4} with amount = 6
  - Greedy: 4 + 1 + 1 = 3 coins
  - Optimal: 3 + 3 = 2 coins

---

## 🎓 GREEDY PROOF TECHNIQUES

### 1. Greedy Choice Property
Prove that making greedy choice leads to optimal solution

### 2. Exchange Argument
- Assume optimal solution differs from greedy
- Show you can exchange to get greedy solution without worsening
- Contradiction → greedy is optimal

### 3. Staying Ahead
- Show greedy solution is always "ahead" of any other solution at each step

---

## 💡 WHEN TO USE GREEDY

✅ **Use Greedy When**:
- Problem has greedy choice property
- Problem has optimal substructure
- Local optimum = global optimum
- Examples: MST, Activity Selection, Huffman

❌ **Don't Use Greedy When**:
- Need to explore all possibilities
- Local choice may block global optimum
- Examples: 0/1 Knapsack, Longest Path

---

## 📊 QUICK COMPARISON

| Problem | Greedy Works? | Alternative |
|---------|---------------|-------------|
| Fractional Knapsack | ✅ Yes | - |
| 0/1 Knapsack | ❌ No | DP |
| Activity Selection | ✅ Yes | - |
| Coin Change (standard) | ✅ Yes | - |
| Coin Change (arbitrary) | ❌ No | DP |
| MST | ✅ Yes | - |
| Shortest Path (non-neg) | ✅ Yes (Dijkstra) | - |
| Shortest Path (negative) | ❌ No | Bellman-Ford |

---

## 🚨 COMMON EXAM MISTAKES

❌ Assuming greedy always works → Must prove greedy choice property

❌ Not sorting before greedy → Most greedy algorithms need sorting

❌ Confusing fractional vs 0/1 knapsack

❌ Using Dijkstra with negative weights → Use Bellman-Ford

---

## 📖 MUST REMEMBER

1. **Greedy = Local optimum** at each step
2. **Must prove** greedy choice property for new problems
3. **Sorting** is usually the first step in greedy
4. **Activity Selection**: Sort by finish time
5. **Fractional Knapsack**: Sort by value/weight ratio
6. **Huffman**: Use min heap, combine two minimum
7. **Job Sequencing**: Sort by profit, fill slots
8. **Kruskal**: Sort edges, use Union-Find
9. **Prim**: Use min heap, grow MST
10. **Dijkstra**: Min heap, non-negative weights only
