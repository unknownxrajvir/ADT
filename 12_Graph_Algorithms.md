# Graph Algorithms

## 📌 KEY DEFINITIONS

**Graph**: G = (V, E) where V = vertices, E = edges

**Directed Graph (Digraph)**: Edges have direction

**Undirected Graph**: Edges have no direction

**Weighted Graph**: Edges have weights/costs

**Path**: Sequence of vertices connected by edges

**Cycle**: Path that starts and ends at same vertex

**Connected Graph**: Path exists between any two vertices

**DAG**: Directed Acyclic Graph (no cycles)

**Degree**: Number of edges connected to vertex

---

## 🏗️ GRAPH REPRESENTATIONS

### 1. Adjacency Matrix
```
matrix[i][j] = weight if edge from i to j exists
            = 0 or ∞ otherwise

Space: O(V²)
Edge lookup: O(1)
Add edge: O(1)
Iterate neighbors: O(V)

Best for: Dense graphs
```

**Example**:
```
Graph:
  0 → 1 (5)
  0 → 2 (3)
  1 → 2 (2)

Matrix:
    0  1  2
0   0  5  3
1   ∞  0  2
2   ∞  ∞  0
```

### 2. Adjacency List
```
array of lists: adj[i] = list of vertices adjacent to i

Space: O(V + E)
Edge lookup: O(degree)
Add edge: O(1)
Iterate neighbors: O(degree)

Best for: Sparse graphs
```

**Example**:
```
Graph:
  0 → 1, 2
  1 → 2

List:
0: [1, 2]
1: [2]
2: []
```

---

## 📝 GRAPH TRAVERSALS

### 1. BREADTH-FIRST SEARCH (BFS)

**Use**: Level-order traversal, shortest path in unweighted graph

```
BFS(graph, start):
    visited[V] = false
    queue = empty
    
    visited[start] = true
    queue.enqueue(start)
    
    while queue is not empty:
        u = queue.dequeue()
        print u
        
        for each vertex v adjacent to u:
            if not visited[v]:
                visited[v] = true
                queue.enqueue(v)

Time: O(V + E)
Space: O(V)
```

**Example**:
```
Graph:
    0
   / \
  1   2
 / \   \
3   4   5

BFS from 0: 0, 1, 2, 3, 4, 5
```

#### BFS for Shortest Path
```
BFS_SHORTEST_PATH(graph, start):
    visited[V] = false
    distance[V] = INFINITY
    parent[V] = -1
    queue = empty
    
    distance[start] = 0
    visited[start] = true
    queue.enqueue(start)
    
    while queue is not empty:
        u = queue.dequeue()
        
        for each vertex v adjacent to u:
            if not visited[v]:
                visited[v] = true
                distance[v] = distance[u] + 1
                parent[v] = u
                queue.enqueue(v)
    
    return distance, parent

Time: O(V + E)
```

---

### 2. DEPTH-FIRST SEARCH (DFS)

**Use**: Topological sort, cycle detection, pathfinding

```
DFS(graph, start, visited):
    visited[start] = true
    print start
    
    for each vertex v adjacent to start:
        if not visited[v]:
            DFS(graph, v, visited)

DFS_ALL(graph):
    visited[V] = false
    
    for v = 0 to V-1:
        if not visited[v]:
            DFS(graph, v, visited)

Time: O(V + E)
Space: O(V) - recursion stack
```

**Example**:
```
Graph:
    0
   / \
  1   2
 / \   \
3   4   5

DFS from 0: 0, 1, 3, 4, 2, 5 (one possible order)
```

#### DFS Iterative (using Stack)
```
DFS_ITERATIVE(graph, start):
    visited[V] = false
    stack = empty
    
    stack.push(start)
    
    while stack is not empty:
        u = stack.pop()
        
        if not visited[u]:
            visited[u] = true
            print u
            
            for each vertex v adjacent to u:
                if not visited[v]:
                    stack.push(v)
```

---

## 🛣️ SHORTEST PATH ALGORITHMS

### 1. DIJKSTRA'S ALGORITHM

**Use**: Single source shortest path with non-negative weights

```
DIJKSTRA(graph, source):
    dist[V] = INFINITY
    visited[V] = false
    dist[source] = 0
    
    priority_queue = {(0, source)}
    
    while priority_queue is not empty:
        (d, u) = priority_queue.extractMin()
        
        if visited[u]:
            continue
        
        visited[u] = true
        
        for each edge (u, v, weight):
            if dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                priority_queue.insert((dist[v], v))
    
    return dist

Time: O((V + E) log V) with min heap
      O(V²) with array
Space: O(V)
```

**Example**:
```
Graph:
    0 --(4)-- 1
    |         |
   (2)       (5)
    |         |
    2 --(1)-- 3

Dijkstra from 0:
dist = [0, 4, 2, 3]
```

#### Print Path
```
PRINT_PATH(parent, v):
    if parent[v] == -1:
        print v
        return
    
    PRINT_PATH(parent, parent[v])
    print v
```

---

### 2. BELLMAN-FORD ALGORITHM

**Use**: Single source shortest path, handles negative weights, detects negative cycles

```
BELLMAN_FORD(graph, source, V, E):
    dist[V] = INFINITY
    dist[source] = 0
    
    // Relax all edges V-1 times
    for i = 1 to V-1:
        for each edge (u, v, weight):
            if dist[u] != INFINITY AND dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
    
    // Check for negative cycles
    for each edge (u, v, weight):
        if dist[u] != INFINITY AND dist[u] + weight < dist[v]:
            return "Negative cycle detected"
    
    return dist

Time: O(V × E)
Space: O(V)
```

**Why V-1 iterations?**
- Shortest path has at most V-1 edges
- Each iteration relaxes one more edge in the path

---

### 3. FLOYD-WARSHALL ALGORITHM

**Use**: All pairs shortest path

```
FLOYD_WARSHALL(graph, V):
    dist[V][V]
    
    // Initialize
    for i = 0 to V-1:
        for j = 0 to V-1:
            if i == j:
                dist[i][j] = 0
            else if edge (i, j) exists:
                dist[i][j] = weight(i, j)
            else:
                dist[i][j] = INFINITY
    
    // Consider each vertex as intermediate
    for k = 0 to V-1:
        for i = 0 to V-1:
            for j = 0 to V-1:
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
    
    return dist

Time: O(V³)
Space: O(V²)
```

**Example**:
```
Graph:
  0 --(3)-- 1
  |         |
 (5)       (2)
  |         |
  2 --(1)-- 3

Floyd-Warshall result:
     0  1  2  3
0    0  3  5  5
1    ∞  0  ∞  2
2    ∞  1  0  1
3    ∞  ∞  ∞  0
```

---

## 🌲 MINIMUM SPANNING TREE (MST)

### 1. KRUSKAL'S ALGORITHM

**Use**: Find MST using greedy approach (edges)

```
KRUSKAL(graph, V, E):
    mst = []
    sort edges by weight (ascending)
    
    parent = MAKE_SET(V)  // Union-Find
    
    for each edge (u, v, weight) in sorted order:
        rootU = FIND(parent, u)
        rootV = FIND(parent, v)
        
        if rootU != rootV:
            mst.append((u, v, weight))
            UNION(parent, rootU, rootV)
            
            if mst.size == V-1:
                break
    
    return mst

FIND(parent, i):
    if parent[i] != i:
        parent[i] = FIND(parent, parent[i])  // Path compression
    return parent[i]

UNION(parent, x, y):
    parent[x] = y

Time: O(E log E) or O(E log V)
Space: O(V)
```

---

### 2. PRIM'S ALGORITHM

**Use**: Find MST using greedy approach (vertices)

```
PRIM(graph, V):
    key[V] = INFINITY
    parent[V] = -1
    inMST[V] = false
    
    key[0] = 0
    priority_queue = {(0, 0)}
    
    while priority_queue is not empty:
        (weight, u) = priority_queue.extractMin()
        
        if inMST[u]:
            continue
        
        inMST[u] = true
        
        for each edge (u, v, w):
            if not inMST[v] AND w < key[v]:
                key[v] = w
                parent[v] = u
                priority_queue.insert((w, v))
    
    return parent

Time: O((V + E) log V) with min heap
      O(V²) with array
Space: O(V)
```

**Example**:
```
Graph:
    0 --(2)-- 1
    |    \    |
   (3)   (8) (5)
    |      \  |
    2 --(1)-- 3

MST edges: (2,3,1), (0,1,2), (0,2,3)
Total weight: 6
```

---

## 🔄 OTHER GRAPH ALGORITHMS

### 1. TOPOLOGICAL SORT (DFS-based)

**Use**: Order vertices in DAG such that for edge u→v, u comes before v

```
TOPOLOGICAL_SORT(graph):
    visited[V] = false
    stack = empty
    
    for v = 0 to V-1:
        if not visited[v]:
            DFS_TOPO(graph, v, visited, stack)
    
    while stack is not empty:
        print stack.pop()

DFS_TOPO(graph, v, visited, stack):
    visited[v] = true
    
    for each vertex u adjacent to v:
        if not visited[u]:
            DFS_TOPO(graph, u, visited, stack)
    
    stack.push(v)  // Push after visiting all neighbors

Time: O(V + E)
Space: O(V)
```

### 2. TOPOLOGICAL SORT (Kahn's Algorithm - BFS)

```
TOPOLOGICAL_SORT_KAHN(graph, V):
    inDegree[V] = 0
    
    // Calculate in-degrees
    for v = 0 to V-1:
        for each vertex u adjacent to v:
            inDegree[u]++
    
    queue = empty
    
    // Enqueue vertices with in-degree 0
    for v = 0 to V-1:
        if inDegree[v] == 0:
            queue.enqueue(v)
    
    result = []
    
    while queue is not empty:
        v = queue.dequeue()
        result.append(v)
        
        for each vertex u adjacent to v:
            inDegree[u]--
            if inDegree[u] == 0:
                queue.enqueue(u)
    
    if result.size != V:
        return "Cycle exists"
    
    return result

Time: O(V + E)
```

---

### 3. CYCLE DETECTION

#### Undirected Graph
```
HAS_CYCLE_UNDIRECTED(graph, v, visited, parent):
    visited[v] = true
    
    for each vertex u adjacent to v:
        if not visited[u]:
            if HAS_CYCLE_UNDIRECTED(graph, u, visited, v):
                return true
        else if u != parent:
            return true  // Back edge found
    
    return false
```

#### Directed Graph
```
HAS_CYCLE_DIRECTED(graph, v, visited, recStack):
    visited[v] = true
    recStack[v] = true
    
    for each vertex u adjacent to v:
        if not visited[u]:
            if HAS_CYCLE_DIRECTED(graph, u, visited, recStack):
                return true
        else if recStack[u]:
            return true  // Back edge in current path
    
    recStack[v] = false
    return false
```

---

### 4. STRONGLY CONNECTED COMPONENTS (SCC)

#### Kosaraju's Algorithm
```
KOSARAJU(graph, V):
    // Step 1: Fill stack with finish times
    visited[V] = false
    stack = empty
    
    for v = 0 to V-1:
        if not visited[v]:
            DFS_FILL_ORDER(graph, v, visited, stack)
    
    // Step 2: Create transpose graph
    transpose = CREATE_TRANSPOSE(graph)
    
    // Step 3: DFS on transpose in stack order
    visited[V] = false
    
    while stack is not empty:
        v = stack.pop()
        if not visited[v]:
            print "SCC:"
            DFS_PRINT(transpose, v, visited)
            print ""

Time: O(V + E)
```

---

### 5. BIPARTITE CHECK (2-Coloring)

```
IS_BIPARTITE(graph, V):
    color[V] = -1
    
    for v = 0 to V-1:
        if color[v] == -1:
            queue = empty
            queue.enqueue(v)
            color[v] = 0
            
            while queue is not empty:
                u = queue.dequeue()
                
                for each vertex w adjacent to u:
                    if color[w] == -1:
                        color[w] = 1 - color[u]
                        queue.enqueue(w)
                    else if color[w] == color[u]:
                        return false
    
    return true

Time: O(V + E)
```

---

## 📊 ALGORITHM COMPARISON

| Problem | Algorithm | Time | Space | Conditions |
|---------|-----------|------|-------|------------|
| **Shortest Path (single)** | Dijkstra | O((V+E) log V) | O(V) | Non-negative |
| **Shortest Path (single)** | Bellman-Ford | O(VE) | O(V) | Any weights |
| **Shortest Path (all)** | Floyd-Warshall | O(V³) | O(V²) | Any weights |
| **MST** | Kruskal | O(E log E) | O(V) | Any |
| **MST** | Prim | O((V+E) log V) | O(V) | Any |
| **Topological Sort** | DFS | O(V+E) | O(V) | DAG |
| **Topological Sort** | Kahn | O(V+E) | O(V) | DAG |
| **SCC** | Kosaraju | O(V+E) | O(V) | Directed |

---

## 📖 MUST REMEMBER

1. **BFS**: Queue, level-order, shortest path unweighted
2. **DFS**: Stack/recursion, explores deep first
3. **Dijkstra**: Greedy, non-negative weights only, use min heap
4. **Bellman-Ford**: DP, handles negative weights, V-1 iterations
5. **Floyd-Warshall**: All pairs, O(V³), DP approach
6. **Kruskal**: Sort edges, use Union-Find for cycle detection
7. **Prim**: Grow MST from vertex, use min heap
8. **Topological Sort**: Only for DAG, DFS or Kahn's algorithm
9. **Cycle Detection**: Track parent (undirected) or recursion stack (directed)
10. **Bipartite**: 2-colorable, use BFS/DFS with alternating colors

---

## 🎯 EXAM TIPS

✅ Draw graph for small examples

✅ Know when to use which algorithm

✅ Dijkstra vs Bellman-Ford: Check for negative weights

✅ MST: Both algorithms work, choose based on graph density

✅ Topological sort only for DAG

✅ Practice tracing algorithms step by step
