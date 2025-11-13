# 🔥 EXAM PRIORITY TOPICS - THEORY EXAM GUIDE

## ⚠️ Teacher's High Priority List - EXPLAINED FOR BEGINNERS

**DON'T PANIC!** This guide explains everything from scratch. Read slowly and understand the concepts.

---

## 📖 HOW TO USE THIS GUIDE (THEORY EXAM STRATEGY)

For theory exams, focus on:
1. **Definitions** - What is it?
2. **Purpose** - Why do we use it?
3. **Steps** - How does it work?
4. **Example** - Small walkthrough
5. **Complexity** - Time/Space (Big O notation)
6. **When to use** - Applications

You DON'T need to write actual code, just understand the logic!

---

## 📊 **GRAPH ALGORITHMS** ⭐⭐⭐⭐⭐

### 1. KRUSKAL'S ALGORITHM (Minimum Spanning Tree)

**📖 WHAT IS IT?**
Kruskal's algorithm finds the cheapest way to connect all vertices (cities/nodes) in a graph using edges (roads) without forming cycles (loops).

**🎯 WHY USE IT?**
- Design minimum cost road/cable networks
- Connect all cities with minimum total distance
- Network design problems

**📝 DEFINITION FOR EXAM:**
"Kruskal's algorithm is a greedy algorithm that finds a Minimum Spanning Tree (MST) for a weighted undirected graph by selecting edges in increasing order of weight, avoiding cycles using Union-Find data structure."

**🔢 HOW IT WORKS (Simple Steps):**
1. **Sort all edges** by weight (smallest to largest)
2. **Pick smallest edge** that doesn't create a cycle
3. **Repeat** until you have (V-1) edges (V = number of vertices)
4. **Done!** You have the MST

**👉 SIMPLE EXAMPLE:**
```
Graph with 4 cities (0,1,2,3):

Roads with costs:
- Road between city 1 and 2: cost 1 ₹
- Road between city 0 and 1: cost 2 ₹
- Road between city 0 and 2: cost 3 ₹
- Road between city 1 and 3: cost 4 ₹
- Road between city 2 and 3: cost 5 ₹

VISUAL DIAGRAM:

Initial Graph (all edges):
        (2)
    0 --------- 1
    |     (1)  /|
 (3)|        /  |(4)
    |      /    |
    |    /      |
    2 --------- 3
        (5)

Step-by-Step MST Construction:

Step 1: Pick edge (1-2, cost=1) ✓
    0           1
                |
             (1)|
                |
                2           3

Step 2: Pick edge (0-1, cost=2) ✓
        (2)
    0 --------- 1
                |
             (1)|
                |
                2           3

Step 3: Skip edge (0-2, cost=3) ✗
    (Would create cycle: 0-1-2-0)
    Already connected via 0→1→2

Step 4: Pick edge (1-3, cost=4) ✓
        (2)
    0 --------- 1
                |\
             (1)| \(4)
                |  \
                2   3

FINAL MST:
        (2)
    0 --------- 1
                |\
             (1)| \(4)
                |  \
                2   3

RESULT: 
- Edges selected: (1-2), (0-1), (1-3)
- Total cost = 1 + 2 + 4 = 7 ₹
- All cities connected with minimum cost! ✓
- Number of edges = 3 (which is V-1 = 4-1) ✓
```

**⏱️ COMPLEXITY:**
- **Time**: O(E log E) - mainly due to sorting edges
- **Space**: O(V) - for storing parent/rank arrays

**✍️ FOR THEORY EXAM WRITE:**
- "Kruskal's uses **greedy approach** on edges"
- "Uses **Union-Find** to detect cycles"
- "Produces MST with (V-1) edges"
- "Better for **sparse graphs** (fewer edges)"

**🆚 COMPARISON:**
Kruskal vs Prim:
- Kruskal: Works on edges, good for sparse graphs
- Prim: Works on vertices, good for dense graphs

### 2. DIJKSTRA'S ALGORITHM (Shortest Path)

**📖 WHAT IS IT?**
Dijkstra's algorithm finds the shortest path from a starting point (source) to all other points in a graph.

**🎯 WHY USE IT?**
- GPS navigation (find shortest route)
- Network routing (find fastest data path)
- Game AI (find shortest path for character)

**📝 DEFINITION FOR EXAM:**
"Dijkstra's algorithm is a greedy algorithm that finds the shortest path from a source vertex to all other vertices in a weighted graph with non-negative edge weights, using a priority queue (min-heap)."

**🔢 HOW IT WORKS (Simple Steps):**
1. **Start** at source, mark distance as 0
2. **All other** vertices: mark distance as ∞ (infinity)
3. **Pick vertex** with smallest distance (not yet visited)
4. **Update** distances to its neighbors if shorter path found
5. **Repeat** until all vertices visited

**👉 SIMPLE EXAMPLE:**
```
Cities: Delhi(0), Mumbai(1), Bangalore(2), Chennai(3)
Find shortest distance from Delhi to all cities

Connections:
Delhi → Mumbai: 4 km
Mumbai → Bangalore: 2 km
Bangalore → Chennai: 3 km
Delhi → Bangalore: 6 km (direct)

SOLUTION:
Initial: [0, ∞, ∞, ∞]
After visiting Delhi: [0, 4, 6, ∞]
After visiting Mumbai: [0, 4, 6, 9]
After visiting Bangalore: [0, 4, 6, 9]

RESULT:
Delhi → Mumbai: 4 km
Delhi → Bangalore: 6 km
Delhi → Chennai: 9 km (via Mumbai, Bangalore)
```

**⏱️ COMPLEXITY:**
- **Time**: O((V+E) log V) with min-heap
- **Space**: O(V) for distance array

**⚠️ IMPORTANT LIMITATION:**
- **ONLY works** with NON-NEGATIVE weights!
- If graph has negative weights, use Bellman-Ford instead

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **greedy approach** - always picks closest unvisited vertex"
- "Uses **min-heap/priority queue** for efficiency"
- "Cannot handle **negative weights**"
- "Finds shortest path from **one source** to all vertices"

**🆚 WHEN TO USE WHAT:**
- **Dijkstra**: Non-negative weights, faster → USE THIS!
- **Bellman-Ford**: Any weights (including negative), slower
- **Floyd-Warshall**: All pairs shortest paths

---

## 🔁 **RECURSION** ⭐⭐⭐⭐⭐

**📖 WHAT IS RECURSION?**
Recursion means a function calling itself to solve a smaller version of the same problem.

**Think of it like:** Russian dolls - each doll contains a smaller doll inside!

### 1. FIBONACCI SEQUENCE

**📖 WHAT IS IT?**
A series of numbers where each number is the sum of the previous two:
0, 1, 1, 2, 3, 5, 8, 13, 21...

**📝 DEFINITION FOR EXAM:**
"Fibonacci is a recursive sequence where F(n) = F(n-1) + F(n-2), with base cases F(0)=0 and F(1)=1."

**🔢 HOW IT WORKS:**
```
Fibonacci(n):
    If n = 0: return 0  ← BASE CASE
    If n = 1: return 1  ← BASE CASE
    Otherwise: return Fibonacci(n-1) + Fibonacci(n-2)  ← RECURSIVE CALL
```

**👉 SIMPLE EXAMPLE:**
```
Find Fibonacci(5):

F(5) = F(4) + F(3)
F(4) = F(3) + F(2)
F(3) = F(2) + F(1) = 2 + 1 = 3
F(2) = F(1) + F(0) = 1 + 0 = 1

So: F(5) = 5

Sequence: 0, 1, 1, 2, 3, 5
          ↑  ↑  ↑  ↑  ↑  ↑
         F0 F1 F2 F3 F4 F5
```

**⏱️ COMPLEXITY:**
- **Naive Recursion**: O(2^n) - Very slow! (exponential)
- **With DP (Dynamic Programming)**: O(n) - Much faster!

**✍️ FOR THEORY EXAM WRITE:**
- "Shows concept of **overlapping subproblems**"
- "Base cases are F(0)=0 and F(1)=1"
- "Can be optimized using Dynamic Programming"
- "Real-world uses: Nature patterns, algorithm analysis"

### 2. TOWER OF HANOI

**📖 WHAT IS IT?**
A puzzle with 3 pegs (rods) and n disks of different sizes. Move all disks from first peg to last peg.

**🎯 RULES:**
1. Move only ONE disk at a time
2. Larger disk can NEVER be on top of smaller disk
3. Use 3 pegs: Source, Auxiliary (helper), Destination

**📝 DEFINITION FOR EXAM:**
"Tower of Hanoi is a classic recursive problem where n disks must be moved from source to destination using an auxiliary peg, following the rule that no larger disk can be placed on a smaller disk."

**🔢 HOW IT WORKS (3 Steps):**
```
To move n disks from A to C using B:

1. Move (n-1) disks from A to B (using C as helper)
2. Move largest disk from A to C
3. Move (n-1) disks from B to C (using A as helper)
```

**👉 SIMPLE EXAMPLE (2 disks):**
```
Initial: A has disks 1,2 (small=1, large=2)
Goal: Move all to C

Step 1: Move disk 1 from A to B
   A: [2]    B: [1]    C: []

Step 2: Move disk 2 from A to C
   A: []     B: [1]    C: [2]

Step 3: Move disk 1 from B to C
   A: []     B: []     C: [2,1]

DONE! Total moves = 3 (which is 2^2 - 1)
```

**📐 FORMULA:**
Total moves for n disks = **2^n - 1**
- 1 disk: 1 move
- 2 disks: 3 moves
- 3 disks: 7 moves
- 4 disks: 15 moves

**⏱️ COMPLEXITY:**
- **Time**: O(2^n) - exponential
- **Moves needed**: 2^n - 1

**✍️ FOR THEORY EXAM WRITE:**
- "Classic example of **divide and conquer**"
- "Recurrence: T(n) = 2T(n-1) + 1"
- "Minimum moves = 2^n - 1"
- "Shows exponential time complexity"

### 3. FACTORIAL

**📖 WHAT IS IT?**
Factorial of n (written as n!) means multiply all numbers from 1 to n.

**📝 DEFINITION FOR EXAM:**
"Factorial of n (n!) is the product of all positive integers from 1 to n. Recursively: n! = n × (n-1)!"

**🔢 HOW IT WORKS:**
```
Factorial(n):
    If n = 0 or n = 1: return 1  ← BASE CASE
    Otherwise: return n × Factorial(n-1)  ← RECURSIVE CALL
```

**👉 SIMPLE EXAMPLE:**
```
Find 5!:

5! = 5 × 4!
4! = 4 × 3!
3! = 3 × 2!
2! = 2 × 1!
1! = 1  ← BASE CASE

Backtrack:
2! = 2 × 1 = 2
3! = 3 × 2 = 6
4! = 4 × 6 = 24
5! = 5 × 24 = 120

Answer: 5! = 120
```

**⏱️ COMPLEXITY:**
- **Time**: O(n) - calls n times
- **Space**: O(n) - recursive stack

**✍️ FOR THEORY EXAM WRITE:**
- "Simple example of **linear recursion**"
- "Base case: 0! = 1 and 1! = 1"
- "Used in permutations and combinations"
- "Can be easily converted to iterative (loop)"

### 4. GCD (Greatest Common Divisor)

**📖 WHAT IS IT?**
GCD is the largest number that divides both given numbers evenly.

**📝 DEFINITION FOR EXAM:**
"GCD of two numbers is the largest positive integer that divides both numbers without leaving a remainder. Uses Euclidean algorithm: GCD(a,b) = GCD(b, a mod b)."

**🔢 HOW IT WORKS (Euclidean Method):**
```
GCD(a, b):
    If b = 0: return a  ← BASE CASE
    Otherwise: return GCD(b, a % b)  ← RECURSIVE CALL
    
% means remainder (modulo)
```

**👉 SIMPLE EXAMPLE:**
```
Find GCD(48, 18):

Step 1: GCD(48, 18)
        48 ÷ 18 = 2 remainder 12
        So: GCD(18, 12)

Step 2: GCD(18, 12)
        18 ÷ 12 = 1 remainder 6
        So: GCD(12, 6)

Step 3: GCD(12, 6)
        12 ÷ 6 = 2 remainder 0
        So: GCD(6, 0)

Step 4: GCD(6, 0)
        b = 0, so return 6  ← ANSWER!

GCD(48, 18) = 6
```

**✅ VERIFY:** 48÷6=8, 18÷6=3 ✓

**⏱️ COMPLEXITY:**
- **Time**: O(log(min(a,b))) - Very fast!
- **Space**: O(log(min(a,b)))

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **Euclidean algorithm** - ancient and efficient"
- "Repeatedly divides until remainder = 0"
- "Logarithmic time complexity"
- "Used in: simplifying fractions, cryptography"

### 5. LCM (Least Common Multiple)

**📖 WHAT IS IT?**
LCM is the smallest number that is divisible by both given numbers.

**📝 DEFINITION FOR EXAM:**
"LCM of two numbers is the smallest positive integer that is a multiple of both numbers. Formula: LCM(a,b) = (a × b) / GCD(a,b)."

**👉 SIMPLE EXAMPLE:**
```
Find LCM(12, 18):

Step 1: Find GCD(12, 18) = 6
Step 2: Apply formula:
        LCM = (12 × 18) / 6
        LCM = 216 / 6
        LCM = 36

✅ Verify: 36÷12=3, 36÷18=2 ✓
```

**📐 IMPORTANT FORMULA:**
**LCM(a,b) × GCD(a,b) = a × b**

**⏱️ COMPLEXITY:**
- **Time**: O(log(min(a,b))) - same as GCD

**✍️ FOR THEORY EXAM WRITE:**
- "Uses GCD for calculation"
- "Formula: LCM = (a×b) / GCD(a,b)"
- "Used in: finding common denominators"

---

### 6. SUM (Recursive)

**📖 WHAT IS IT?**
Adding numbers recursively - either array elements or digits of a number.

**📝 DEFINITION FOR EXAM:**
"Recursive sum breaks down the addition problem into smaller subproblems until reaching a base case."

**🔢 TWO TYPES:**

**A) Sum of Array Elements:**
```
Sum([5, 3, 8, 2]):
    = 5 + Sum([3, 8, 2])
    = 5 + 3 + Sum([8, 2])
    = 5 + 3 + 8 + Sum([2])
    = 5 + 3 + 8 + 2 + Sum([]) ← BASE CASE (empty = 0)
    = 18
```

**B) Sum of Digits:**
```
Find sum of digits of 1234:

1234 → last digit = 4, remaining = 123
4 + SumDigits(123)

123 → last digit = 3, remaining = 12
4 + 3 + SumDigits(12)

12 → last digit = 2, remaining = 1
4 + 3 + 2 + SumDigits(1)

1 → last digit = 1, remaining = 0
4 + 3 + 2 + 1 + SumDigits(0) ← BASE CASE

Answer: 10
```

**⏱️ COMPLEXITY:**
- **Time**: O(n) where n = array size or number of digits
- **Space**: O(n) for recursive stack

**✍️ FOR THEORY EXAM WRITE:**
- "Example of **linear recursion**"
- "Base case: empty array returns 0, or number = 0"
- "Simple but demonstrates recursion concept well"

---

## 🎯 **GREEDY ALGORITHMS** ⭐⭐⭐⭐

**📖 WHAT ARE GREEDY ALGORITHMS?**
Greedy algorithms make the **locally optimal choice** at each step, hoping to find a global optimum.

**Think of it like:** Always taking the best option RIGHT NOW, without looking ahead!

### 1. ACTIVITY SELECTION PROBLEM

**📖 WHAT IS IT?**
Given activities with start and end times, select maximum number of activities that don't overlap.

**🎯 REAL-WORLD USE:**
- Meeting room scheduling
- Class timetable planning
- TV show scheduling

**📝 DEFINITION FOR EXAM:**
"Activity Selection is a greedy algorithm that selects maximum number of non-overlapping activities by always choosing the activity that finishes earliest."

**🔢 HOW IT WORKS:**
```
1. SORT all activities by finish time (earliest first)
2. SELECT first activity
3. For remaining activities:
   - If start time ≥ previous finish time → SELECT it
   - Otherwise → SKIP it
```

**👉 SIMPLE EXAMPLE:**
```
Activities (start, finish):
A1: (1, 3)
A2: (2, 5)  
A3: (4, 7)
A4: (1, 8)
A5: (5, 9)
A6: (8, 10)

After sorting by finish time (already sorted above):

Step 1: Select A1 (1,3) ✓
Step 2: A2 starts at 2, but A1 finishes at 3 → SKIP
Step 3: A3 starts at 4 ≥ 3 → SELECT ✓
Step 4: A4 overlaps → SKIP
Step 5: A5 starts at 5 < 7 → SKIP
Step 6: A6 starts at 8 ≥ 7 → SELECT ✓

ANSWER: 3 activities (A1, A3, A6)
```

**⏱️ COMPLEXITY:**
- **Time**: O(n log n) - due to sorting
- **Space**: O(1)

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **greedy strategy** - earliest finish time first"
- "Guarantees optimal solution"
- "Can prove correctness using **exchange argument**"

---

### 2. FRACTIONAL KNAPSACK

**📖 WHAT IS IT?**
Given items with weights and values, fill a bag of limited capacity to maximize total value. Can take fractions of items.

**🎯 REAL-WORLD USE:**
- Loading cargo in truck (can break items)
- Resource allocation
- Portfolio management

**📝 DEFINITION FOR EXAM:**
"Fractional Knapsack uses greedy approach to maximize value by selecting items in decreasing order of value-to-weight ratio, taking fractions if needed."

**🔢 HOW IT WORKS:**
```
1. Calculate VALUE/WEIGHT ratio for each item
2. SORT items by ratio (highest first)
3. Keep ADDING items until bag is full
4. If item doesn't fit completely, take FRACTION
```

**👉 SIMPLE EXAMPLE:**
```
Bag capacity: 50 kg

Items:
Item 1: Weight=10kg, Value=₹60  → Ratio=6
Item 2: Weight=20kg, Value=₹100 → Ratio=5
Item 3: Weight=30kg, Value=₹120 → Ratio=4

Sorted by ratio: Item1, Item2, Item3

Step 1: Take Item1 (10kg) → Remaining: 40kg, Value: ₹60
Step 2: Take Item2 (20kg) → Remaining: 20kg, Value: ₹160
Step 3: Take 20kg of Item3 (20/30 = 2/3)
        → Value = 2/3 × ₹120 = ₹80

TOTAL VALUE: ₹60 + ₹100 + ₹80 = ₹240
```

**⏱️ COMPLEXITY:**
- **Time**: O(n log n) - sorting
- **Space**: O(1)

**⚠️ IMPORTANT:**
- **FRACTIONAL** Knapsack → Greedy WORKS ✓
- **0/1** Knapsack → Greedy FAILS ✗ (use DP instead!)

**✍️ FOR THEORY EXAM WRITE:**
- "Greedy strategy: **highest value/weight ratio first**"
- "Works only for **fractional** version"
- "For 0/1 Knapsack, must use Dynamic Programming"

---

### 3. COIN CHANGE (Greedy Approach)

**📖 WHAT IS IT?**
Make change for an amount using minimum number of coins.

**📝 DEFINITION FOR EXAM:**
"Coin Change greedy approach selects largest denomination coins first to minimize coin count. Works for standard denominations but NOT always optimal for arbitrary coin sets."

**🔢 HOW IT WORKS:**
```
1. SORT coins in decreasing order
2. Take LARGEST coin that fits
3. REPEAT until amount becomes 0
```

**👉 EXAMPLE 1 (Greedy WORKS):**
```
Coins: [1, 5, 10, 25]
Amount: 41

Step 1: Take 25 → Remaining: 16
Step 2: Take 10 → Remaining: 6
Step 3: Take 5 → Remaining: 1
Step 4: Take 1 → Remaining: 0

Answer: 4 coins (25+10+5+1) ✓ OPTIMAL
```

**👉 EXAMPLE 2 (Greedy FAILS!):**
```
Coins: [1, 3, 4]
Amount: 6

Greedy approach:
Take 4 → Remaining: 2
Take 1 → Remaining: 1
Take 1 → Remaining: 0
Result: 3 coins (4+1+1) ✗

OPTIMAL approach:
Take 3 → Remaining: 3
Take 3 → Remaining: 0
Result: 2 coins (3+3) ✓ BETTER!
```

**⏱️ COMPLEXITY:**
- **Time**: O(n log n) for sorting + O(amount/smallest_coin)
- **Space**: O(1)

**⚠️ CRITICAL FOR EXAM:**
**When Greedy WORKS:**
- Standard currency: {1, 5, 10, 25, 50, 100} ✓
- Each coin divides the next: {1, 2, 4, 8} ✓

**When Greedy FAILS:**
- Arbitrary denominations like {1, 3, 4} ✗
- **USE DYNAMIC PROGRAMMING instead!**

**✍️ FOR THEORY EXAM WRITE:**
- "Greedy: **largest coin first**"
- "Not always optimal - depends on coin system"
- "For arbitrary coins, use **DP approach**"
- "Know examples where greedy fails!"

---

## 🔤 **STRING ALGORITHMS** ⭐⭐⭐⭐

**📖 WHAT ARE STRING ALGORITHMS?**
Algorithms for searching patterns in text efficiently.

### 1. RABIN-KARP (Pattern Matching with Hashing)

**📖 WHAT IS IT?**
Finds a pattern in text using a **rolling hash** function - like a sliding window with fingerprints!

**🎯 WHY USE IT?**
- Searching in documents (Ctrl+F)
- Plagiarism detection
- Finding multiple patterns at once

**📝 DEFINITION FOR EXAM:**
"Rabin-Karp uses hashing to find pattern matches in text. It computes hash values for pattern and text windows, using a rolling hash to slide efficiently through the text."

**🔢 HOW IT WORKS:**
```
1. Calculate HASH of pattern
2. Calculate HASH of first window in text
3. COMPARE hashes:
   - If match → verify character by character
   - If no match → slide window
4. Use ROLLING HASH to update window hash efficiently
5. REPEAT until end of text
```

**👉 SIMPLE EXAMPLE:**
```
Text: "HELLO WORLD"
Pattern: "WORLD"

Imagine hash as sum of ASCII values (simplified):

Pattern "WORLD" hash = (some value X)

Slide through text:
Window 1: "HELLO" → hash ≠ X
Window 2: "ELLO " → hash ≠ X
Window 3: "LLO W" → hash ≠ X
...
Window 7: "WORLD" → hash = X ✓ MATCH!
         → Verify: W=W, O=O, R=R, L=L, D=D ✓

Pattern found at position 6!
```

**⏱️ COMPLEXITY:**
- **Average**: O(n+m) - Very good!
- **Worst case**: O(nm) - when many hash collisions
- n = text length, m = pattern length

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **rolling hash function**"
- "Fast average case: O(n+m)"
- "Good for **multiple pattern** search"
- "Hash collision needs character verification"

---

### 2. KMP (Knuth-Morris-Pratt Algorithm)

**📖 WHAT IS IT?**
Smart pattern matching that **never backtracks** in the text - it "remembers" what it already matched!

**🎯 WHY USE IT?**
- Faster than naive search
- Good when pattern has repeating characters
- Used in text editors

**📝 DEFINITION FOR EXAM:**
"KMP algorithm uses a Longest Prefix Suffix (LPS) array to avoid re-checking already matched characters, achieving O(n+m) time complexity."

**🔢 HOW IT WORKS:**
```
STEP 1: Build LPS array (preprocessing)
        LPS tells: when mismatch occurs, where to continue

STEP 2: Search using LPS
        - Match characters
        - On mismatch, use LPS to skip unnecessary comparisons
```

**👉 SIMPLE EXAMPLE:**
```
Pattern: "ABABC"

STEP 1: Build LPS array
A B A B C
0 0 1 2 0

Why? 
- A: no prefix/suffix → 0
- AB: no match → 0
- ABA: "A" matches → 1
- ABAB: "AB" matches → 2
- ABABC: no match → 0

STEP 2: Search in text
Text: "ABABDABACDABABCABAB"
           ↑ mismatch at D
Instead of starting over, LPS says:
"You already matched AB, continue from there!"

Positions found: 10
```

**👉 SIMPLER VISUALIZATION:**
```
Normal search: ABABC
Text:         ABABDABABC...
              ✓✓✓✓✗
              Start again from B? NO!
              
KMP:          Already know first 2 matched
              Jump smartly using LPS!
```

**⏱️ COMPLEXITY:**
- **Time**: O(n+m) - guaranteed!
- **Space**: O(m) - for LPS array

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **LPS (Longest Prefix Suffix)** array"
- "Never backtracks in text - efficient!"
- "Preprocessing: O(m), Search: O(n)"
- "Best for patterns with repeated characters"

---

### 3. MANACHER'S ALGORITHM (Longest Palindrome)

**📖 WHAT IS IT?**
Finds the longest palindromic substring in **linear time** - amazingly fast!

**🎯 WHY USE IT?**
- Finding palindromes in DNA sequences
- Text analysis
- Competitive programming

**📝 DEFINITION FOR EXAM:**
"Manacher's algorithm finds the longest palindromic substring in O(n) time by cleverly using previously computed palindrome information to avoid redundant checks."

**🔢 WHAT IS A PALINDROME?**
Reads same forwards and backwards:
- "racecar" ✓
- "madam" ✓
- "noon" ✓
- "hello" ✗

**👉 SIMPLE EXAMPLE:**
```
String: "babad"

Check all substrings for palindrome:
"b" → palindrome ✓
"ba" → not palindrome
"bab" → palindrome ✓ (3 chars)
"baba" → not palindrome
"babad" → not palindrome
"a" → palindrome ✓
"aba" → palindrome ✓ (3 chars)
"abad" → not palindrome
...

Longest palindromes: "bab" or "aba" (length = 3)
```

**⏱️ COMPLEXITY:**
- **Naive approach**: O(n³) - check all substrings
- **Better approach**: O(n²) - expand around centers
- **Manacher's**: O(n) - uses smart mirroring!

**🔑 KEY CONCEPT:**
Manacher's uses the idea that palindromes **mirror** around center:
```
If "abcba" is palindrome:
     ↓ center
   a b c b a
   ↑       ↑
   mirror!
```

**✍️ FOR THEORY EXAM WRITE:**
- "Finds longest palindrome in **O(n) time**"
- "Uses concept of **palindrome mirroring**"
- "Processes both odd & even length palindromes"
- "Much faster than naive O(n³) approach"
- "Uses preprocessing to avoid redundant checks"

---
- Never backtracks in text
- Time: O(n+m)
- Space: O(m)

Example:
pattern = "AABAACAABAA"
LPS = [0,1,0,1,2,0,1,2,3,4,5]
```

### 3. MANACHER'S ALGORITHM (Longest Palindrome)
```
MANACHER(s):
    // Transform: "aba" → "#a#b#a#"
    T = "#"
    for char in s:
        T += char + "#"
    
    n = T.length
    P = [0] * n  // Palindrome radius
    center = 0
    right = 0
    
    for i = 0 to n-1:
        mirror = 2 * center - i
        
        if i < right:
            P[i] = min(right - i, P[mirror])
        
        // Expand around center
        while i + P[i] + 1 < n and i - P[i] - 1 >= 0:
            if T[i + P[i] + 1] == T[i - P[i] - 1]:
                P[i]++
            else:
                break
        
        // Update center and right
        if i + P[i] > right:
            center = i
            right = i + P[i]
    
    // Find max palindrome
    maxLen = max(P)
    centerIndex = P.index(maxLen)
    start = (centerIndex - maxLen) / 2
    
    return s[start...start+maxLen-1]

Key Points:
- Finds longest palindrome in O(n)
- Transform string to handle even/odd lengths
- Reuses previously computed information
- Space: O(n)

Example:
s = "babad"
T = "#b#a#b#a#d#"
Result: "bab" or "aba"
```

---

## 🔄 **BACKTRACKING** ⭐⭐⭐⭐

**📖 WHAT IS BACKTRACKING?**
Backtracking means **trying all possibilities** and going back (backtracking) when you hit a dead end.

**Think of it like:** Solving a maze - try path, if blocked, go back and try another!

### 1. GRAPH COLORING (M-Coloring Problem)

**📖 WHAT IS IT?**
Color a graph with M colors such that no two adjacent (connected) vertices have the same color.

**🎯 REAL-WORLD USE:**
- Map coloring (countries on map)
- Time table scheduling (no conflicts)
- Register allocation in compilers

**📝 DEFINITION FOR EXAM:**
"Graph Coloring assigns colors to vertices such that no adjacent vertices share the same color, using backtracking to try all color combinations."

**🔢 HOW IT WORKS:**
```
1. Start with first vertex
2. Try COLOR 1:
   - If safe (no neighbor has this color) → assign it
   - Move to next vertex
3. If stuck (no color works):
   - BACKTRACK → change previous vertex's color
4. Repeat until all vertices colored
```

**👉 SIMPLE EXAMPLE:**
```
Graph: Triangle (3 vertices, all connected)
      1
     /  \
    2----3

Colors available: Red, Blue, Green

Step 1: Color vertex 1 → RED
Step 2: Color vertex 2 → BLUE (can't be RED, neighbor!)
Step 3: Color vertex 3 → GREEN (can't be RED or BLUE!)

Result: [RED, BLUE, GREEN] ✓

Minimum colors needed: 3
```

**⏱️ COMPLEXITY:**
- **Time**: O(m^n) - try m colors for n vertices
- **Space**: O(n) - recursion stack

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **backtracking** - try and revert"
- "Check safety: no adjacent same color"
- "Applications: scheduling, map coloring"
- "Chromatic number = minimum colors needed"

---

### 2. N-QUEENS PROBLEM

**📖 WHAT IS IT?**
Place N queens on N×N chessboard so that no queen attacks another.

**🎯 RULES:**
Queens can attack:
- Same ROW ↔
- Same COLUMN ↕
- Same DIAGONAL ↗↘

**📝 DEFINITION FOR EXAM:**
"N-Queens places N queens on N×N board using backtracking such that no two queens threaten each other (no shared row, column, or diagonal)."

**🔢 HOW IT WORKS:**
```
1. Place queen in ROW 0, try each column
2. Move to ROW 1, find safe column
3. If no safe column in current row:
   - BACKTRACK to previous row
   - Try next position
4. Continue until all N queens placed
```

**👉 SIMPLE EXAMPLE (4-Queens):**
```
4×4 Board:

Try placing queens:

Row 0: Place at column 1
. Q . .    ← Queen 1

Row 1: Can't use col 0,1,2 (attacked)
. . . Q    ← Queen 2

Row 2: Try col 0
Q . . .    ← SAFE? Check...
   
If stuck, BACKTRACK!

Final solution:
. Q . .    Row 0
. . . Q    Row 1
Q . . .    Row 2
. . Q .    Row 3

No queen attacks another! ✓
```

**⏱️ COMPLEXITY:**
- **Time**: O(N!) - factorial
- **Space**: O(N) - recursion

**✍️ FOR THEORY EXAM WRITE:**
- "Classic **backtracking** problem"
- "Check row, column, and **both diagonals**"
- "Place row by row"
- "Time complexity: O(N!)"

---

### 3. SUDOKU SOLVER

**📖 WHAT IS IT?**
Fill 9×9 grid with digits 1-9 following Sudoku rules.

**🎯 SUDOKU RULES:**
Each digit 1-9 must appear:
- Once in each ROW
- Once in each COLUMN
- Once in each 3×3 BOX

**📝 DEFINITION FOR EXAM:**
"Sudoku Solver uses backtracking to fill empty cells with digits 1-9, checking row, column, and 3×3 box constraints."

**🔢 HOW IT WORKS:**
```
1. Find empty cell
2. Try digits 1 to 9:
   - Check if VALID (row, column, box)
   - If valid → place it, move to next cell
3. If no digit works:
   - BACKTRACK → remove last placed digit
   - Try next digit
4. Repeat until board complete
```

**👉 SIMPLE EXAMPLE (4×4 mini-Sudoku):**
```
Rules: 1-4 in each row, column, and 2×2 box

Initial:
. 2 | 3 .
4 . | . 1
----+----
. 4 | 1 .
3 . | . 4

Solve:
Step 1: Top-left must be 1 (only option)
Step 2: Continue filling...

Final:
1 2 | 3 4
4 3 | 2 1
----+----
2 4 | 1 3
3 1 | 4 2  ✓
```

**⏱️ COMPLEXITY:**
- **Time**: O(9^m) where m = empty cells
- **Space**: O(1) if modifying in-place

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **backtracking** with constraint checking"
- "Checks: row, column, and 3×3 sub-grid"
- "Fills cells one by one"
- "Backtracks when no valid number"

---

## 💎 **DYNAMIC PROGRAMMING (DP)** ⭐⭐⭐⭐⭐

**📖 WHAT IS DYNAMIC PROGRAMMING?**
DP solves complex problems by **breaking them into smaller overlapping subproblems** and **storing results** to avoid recomputing!

**Think of it like:** Writing answers in your notebook so you don't solve same problem twice!

**🔑 TWO KEY PROPERTIES:**
1. **Overlapping Subproblems** - same smaller problems appear multiple times
2. **Optimal Substructure** - optimal solution uses optimal solutions of subproblems

### 1. KNAPSACK (0/1) - Cannot Break Items

**📖 WHAT IS IT?**
Fill a bag with items to maximize value, but you can only take FULL items (can't break them).

**🎯 REAL-WORLD USE:**
- Loading cargo (full boxes only)
- Investment decisions (buy whole stocks)
- Resource allocation

**📝 DEFINITION FOR EXAM:**
"0/1 Knapsack uses Dynamic Programming to find maximum value that can fit in weight capacity W, where each item can be taken (1) or left (0)."

**🔢 HOW IT WORKS:**
```
Build a TABLE (2D array):
Rows = items
Columns = weights from 0 to W

For each cell:
  If item weight ≤ current capacity:
    CHOOSE MAX of:
      - Take item: item_value + dp[previous items][remaining weight]
      - Skip item: dp[previous items][same weight]
  Else:
    Skip item (too heavy)
```

**👉 SIMPLE EXAMPLE:**
```
Items:
Item 1: weight=2kg, value=₹6
Item 2: weight=3kg, value=₹10
Item 3: weight=4kg, value=₹12

Bag capacity: 5kg

DP Table:
       Weight→  0  1  2  3  4  5
Item ↓
0 (none)        0  0  0  0  0  0
1 (2kg,₹6)      0  0  6  6  6  6
2 (3kg,₹10)     0  0  6 10 10 16
3 (4kg,₹12)     0  0  6 10 12 16

Answer: ₹16 (take items 1 and 2)
```

**⏱️ COMPLEXITY:**
- **Time**: O(n × W) where n=items, W=capacity
- **Space**: O(n × W), can optimize to O(W)

**⚠️ CRITICAL:**
- **0/1 Knapsack**: Cannot break items → Use DP!
- **Fractional Knapsack**: Can break items → Use Greedy!

**✍️ FOR THEORY EXAM WRITE:**
- "Uses **DP table** - rows=items, cols=weights"
- "Choice: include or exclude each item"
- "Time: O(n×W), Space: O(n×W)"
- "Different from fractional (greedy fails here!)"

---

### 2. COIN CHANGE PROBLEM (DP Approach)

**📖 WHAT IS IT?**
Make change for amount using minimum coins OR count total ways to make change.

**📝 TWO VERSIONS:**

**A) MINIMUM COINS NEEDED:**

**Definition:** "Find minimum number of coins needed to make amount, using DP to try all combinations."

**👉 EXAMPLE:**
```
Coins: [1, 5, 10]
Amount: 14

DP approach:
Amount 0: 0 coins ← BASE
Amount 1: 1 coin (1)
Amount 2: 2 coins (1+1)
...
Amount 10: 1 coin (10)
Amount 11: 2 coins (10+1)
Amount 14: 2 coins (10+1+1+1+1) NO!
         = 5 coins? NO!
         = Optimal: 5+5+1+1+1+1 = 6 coins? NO!
         = Best: 10+1+1+1+1 = 5 coins

Actually: 14 = 10+1+1+1+1 = 5 coins
Better: No! That's wrong too!

CORRECT: 14 = 10 + 1 + 1 + 1 + 1 (5 coins)
NO WAIT: Can't we do better?
14 = 5 + 5 + 1 + 1 + 1 + 1 (6 coins)

ACTUALLY BEST: Let me recalculate:
14 with [1,5,10]
Try: 10 + ? → need 4 more → 1+1+1+1 = 4 ones
Total: 1+4 = 5 coins ✓ (10,1,1,1,1)
```

**B) COUNT NUMBER OF WAYS:**

**👉 EXAMPLE:**
```
Coins: [1, 2, 3]
Amount: 4

Ways:
1. 1+1+1+1 (4 ones)
2. 1+1+2 (two ones, one two)
3. 2+2 (two twos)
4. 1+3 (one one, one three)

Total: 4 ways
```

**⏱️ COMPLEXITY:**
- **Time**: O(amount × number of coins)
- **Space**: O(amount)

**✍️ FOR THEORY EXAM WRITE:**
- "Use DP when coin set is **arbitrary** (greedy fails!)"
- "Min coins: dp[i] = min(dp[i], 1 + dp[i-coin])"
- "Count ways: iterate coins first to avoid duplicates"
- "Example where greedy fails: coins=[1,3,4], amount=6"

---

### 3. LCS (Longest Common Subsequence)

**📖 WHAT IS IT?**
Find longest sequence that appears in same order in both strings (but not necessarily consecutive).

**🎯 DIFFERENCE:**
- **Subsequence**: Can skip characters → "ace" from "abcde"
- **Substring**: Must be consecutive → "abc" from "abcde"

**📝 DEFINITION FOR EXAM:**
"LCS finds the longest subsequence common to two sequences using DP, where characters appear in same order but need not be consecutive."

**👉 SIMPLE EXAMPLE:**
```
String X: "ABCDGH"
String Y: "AEDFHR"

Find matches:
A - A ✓
B - (skip)
C - (skip)
D - D ✓
(skip E, F)
H - H ✓

LCS: "ADH" (length = 3)
```

**🔢 HOW IT WORKS:**
```
Build DP table:

If characters match:
  dp[i][j] = 1 + dp[i-1][j-1]
  
If don't match:
  dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

**DP TABLE VISUALIZATION:**
```
       ""  A  E  D  F  H  R
    "" 0   0  0  0  0  0  0
    A  0   1  1  1  1  1  1
    B  0   1  1  1  1  1  1
    C  0   1  1  1  1  1  1
    D  0   1  1  2  2  2  2
    G  0   1  1  2  2  2  2
    H  0   1  1  2  2  3  3

Answer: dp[6][6] = 3 ✓
```

**⏱️ COMPLEXITY:**
- **Time**: O(m × n) where m, n are string lengths
- **Space**: O(m × n)

**✍️ FOR THEORY EXAM WRITE:**
- "**Subsequence** not substring!"
- "DP table: match → diagonal+1, no match → max(top,left)"
- "Applications: diff tools, DNA comparison"
- "Can reconstruct actual LCS by backtracking table"

---

Example:
X = "ABCDGH", Y = "AEDFHR"
LCS = 3 (ADH)
```

### 4. LIS (Longest Increasing Subsequence)

**📖 WHAT IS IT?**
Find longest subsequence where numbers are in **increasing order**.

**📝 DEFINITION FOR EXAM:**
"LIS finds the longest subsequence from an array where elements are in strictly increasing order, using DP to check all possibilities."

**👉 SIMPLE EXAMPLE:**
```
Array: [10, 9, 2, 5, 3, 7, 101, 18]

Find increasing subsequences:
[10] - length 1
[10, 101] - length 2
[2, 5, 7, 101] - length 4 ✓
[2, 3, 7, 18] - length 4 ✓
[2, 5, 7, 18] - length 4 ✓

Longest Length: 4
```

**🔢 HOW IT WORKS:**
```
For each position i:
  dp[i] = longest increasing sequence ending at i
  
Check all previous positions j < i:
  If arr[j] < arr[i]:
    dp[i] = max(dp[i], dp[j] + 1)
```

**⏱️ COMPLEXITY:**
- **Basic DP**: O(n²) - easy to understand
- **Optimized**: O(n log n) - uses binary search

**✍️ FOR THEORY EXAM WRITE:**
- "dp[i] = LIS ending at index i"
- "Check all previous smaller elements"
- "Time: O(n²) or O(n log n) optimized"
- "Different from LCS!"

---

### 5. LPS (Longest Palindromic Subsequence)

**📖 WHAT IS IT?**
Find longest palindrome that can be formed using subsequence (not necessarily consecutive).

**📝 DEFINITION FOR EXAM:**
"LPS finds the longest palindromic subsequence in a string, where characters can be non-consecutive but must maintain relative order."

**👉 SIMPLE EXAMPLE:**
```
String: "BBABCBCAB"

Palindromic subsequences:
"BB" - length 2
"BAB" - length 3
"BABAB" - length 5
"BABCBAB" - length 7 ✓ LONGEST!

Answer: 7
```

**🔢 HOW IT WORKS:**
```
If first and last characters match:
  LPS = 2 + LPS(middle part)
  
If they don't match:
  LPS = max(
    LPS(skip first),
    LPS(skip last)
  )
```

**💡 TRICK:**
Can also solve by:
**LPS(s) = LCS(s, reverse(s))**

```
Example:
s = "BBABCBCAB"
reverse = "BACBCBABB"

Find LCS → gives LPS!
```

**⏱️ COMPLEXITY:**
- **Time**: O(n²)
- **Space**: O(n²)

**✍️ FOR THEORY EXAM WRITE:**
- "**Interval DP** problem"
- "Can use LCS with reversed string"
- "Match ends → include both + recurse middle"
- "No match → max of (skip left, skip right)"

---

### 6. MINIMUM PATH SUM (Grid/Matrix)

**📖 WHAT IS IT?**
Find path from top-left to bottom-right in grid with minimum sum. Can only move right or down.

**📝 DEFINITION FOR EXAM:**
"Minimum Path Sum finds the path from top-left to bottom-right corner with minimum sum, allowing only right and down movements, using DP."

**👉 SIMPLE EXAMPLE:**
```
Grid:
1  3  1
1  5  1
4  2  1

Possible paths:
Path 1: 1→3→1→1→1 = 7 ✓ MINIMUM
Path 2: 1→1→5→1→1 = 9
Path 3: 1→1→4→2→1 = 9

Answer: 7
```

**🔢 HOW IT WORKS:**
```
Build DP table:

For each cell:
  dp[i][j] = grid[i][j] + min(
    dp[i-1][j],  ← from top
    dp[i][j-1]   ← from left
  )

First row: can only come from left
First column: can only come from top
```

**DP TABLE:**
```
Grid:          DP Table:
1  3  1        1  4  5
1  5  1   →    2  7  6
4  2  1        6  8  7

Answer: dp[2][2] = 7
```

**⏱️ COMPLEXITY:**
- **Time**: O(m × n)
- **Space**: O(m × n), can optimize to O(n)

**✍️ FOR THEORY EXAM WRITE:**
- "Only **right** and **down** moves allowed"
- "dp[i][j] = grid[i][j] + min(top, left)"
- "Initialize first row and column separately"
- "Similar to: Unique Paths problem"

---

---

## 🚨 EXAM STRATEGY FOR THESE TOPICS

### For GRAPH:
✅ **Kruskal**: Always mention Union-Find and "sort edges by weight"
✅ **Dijkstra**: State "non-negative weights only" requirement  
✅ Draw small graph example to show understanding

### For RECURSION:
✅ **Always write base case FIRST** - this is critical!
✅ Show recurrence relation: T(n) = ...  
✅ State time complexity with reason (exponential/linear)

### For GREEDY:
✅ **Prove greedy choice works** (or state when it fails!)
✅ Show sorting step clearly  
✅ Know examples where greedy fails (coin change!)

### For STRINGS:
✅ **KMP**: Explain LPS array purpose
✅ **Rabin-Karp**: Mention rolling hash benefit
✅ **Manacher's**: State O(n) achievement

### For BACKTRACKING:
✅ **Three steps**: Try → Recurse → Backtrack  
✅ Explain what "backtrack" means (undo and try next)
✅ State exponential time complexity

### For DP:
✅ **Define state clearly**: "dp[i][j] represents..."  
✅ Write recurrence relation  
✅ Explain base cases
✅ State overlapping subproblems + optimal substructure

---

## 📋 QUICK REFERENCE CHEAT SHEET

### TIME COMPLEXITIES (Know These!)
| Algorithm | Time Complexity | Space |
|-----------|----------------|--------|
| **Kruskal** | O(E log E) | O(V) |
| **Dijkstra** | O((V+E) log V) | O(V) |
| **Fibonacci (naive)** | O(2^n) | O(n) |
| **Fibonacci (DP)** | O(n) | O(n) |
| **Tower of Hanoi** | O(2^n) | O(n) |
| **Activity Selection** | O(n log n) | O(1) |
| **Fractional Knapsack** | O(n log n) | O(1) |
| **Rabin-Karp** | O(n+m) avg | O(1) |
| **KMP** | O(n+m) | O(m) |
| **Manacher's** | O(n) | O(n) |
| **N-Queens** | O(N!) | O(N) |
| **0/1 Knapsack** | O(n×W) | O(n×W) |
| **Coin Change** | O(amount×coins) | O(amount) |
| **LCS** | O(m×n) | O(m×n) |
| **LIS** | O(n²) or O(n log n) | O(n) |
| **Min Path Sum** | O(m×n) | O(m×n) |

### KEY CONCEPTS TO REMEMBER

**GREEDY vs DP:**
- Greedy: Local best choice → May not be globally optimal
- DP: Try all possibilities → Guaranteed optimal

**When Greedy WORKS:**
- Activity Selection ✓
- Fractional Knapsack ✓
- Kruskal's MST ✓
- Dijkstra (non-negative weights) ✓

**When Greedy FAILS (use DP):**
- 0/1 Knapsack ✗
- Arbitrary Coin Change ✗

**RECURSION KEY:**
- **Base case** = stopping condition
- **Recurrence** = problem in terms of smaller problem
- Always write base case first!

**DP PATTERNS:**
- **Linear DP**: Fibonacci, Coin Change, LIS
- **2D DP**: Knapsack, LCS, Grid problems
- **Interval DP**: LPS

**BACKTRACKING PATTERN:**
```
1. Make a choice
2. Recurse
3. If solution found → return
4. Else → Undo choice (backtrack)
5. Try next choice
```

---

## ⏰ TONIGHT'S STUDY PLAN (7 HOURS)

### 🕐 Hour 1-2: DYNAMIC PROGRAMMING (Most Important!)
**Why:** Maximum problems in exam from here!
- Focus: 0/1 Knapsack, Coin Change, LCS
- Practice: Draw DP table for small example
- Understand: Include/exclude pattern

### 🕒 Hour 3: GRAPH ALGORITHMS
**Why:** Teacher specifically emphasized!
- Kruskal: Remember Union-Find
- Dijkstra: Remember non-negative weights limitation
- Draw one example of each

### 🕓 Hour 4: RECURSION (All 6 problems)
**Why:** Foundation for many algorithms!
- Fibonacci, Factorial (easy ones first)
- Tower of Hanoi, GCD (medium)
- Practice writing base cases

### 🕔 Hour 5: GREEDY + STRINGS
- Greedy: Know when it fails!
- Activity Selection pattern
- KMP: Understand LPS concept
- Rabin-Karp: Rolling hash idea

### 🕕 Hour 6: BACKTRACKING
- N-Queens: Understand row-by-row approach
- Graph Coloring: Safety check concept
- General backtracking pattern

### 🕖 Hour 7: REVISION & PRACTICE
- Read all definitions again
- Practice explaining 2-3 algorithms aloud
- Review this cheat sheet
- Check complexity table

### 🛌 SLEEP! (Critical!)
Get 6-7 hours sleep - your brain needs it to consolidate!

---

## 🎯 FINAL EXAM TIPS

### Writing Answers:
1. **Start with definition** - 2-3 lines
2. **Explain purpose** - Real-world use
3. **Show steps** - Numbered list
4. **Give example** - Small, clear example
5. **State complexity** - Time and space

### Time Management:
- **Graphs**: 15 minutes each
- **Recursion**: 8-10 minutes each
- **DP**: 15 minutes each
- **Others**: 10 minutes each

### Common Mistakes to Avoid:
❌ Forgetting to state base case in recursion
❌ Confusing subsequence with substring
❌ Not mentioning time complexity
❌ Using greedy when DP is needed
❌ Forgetting Dijkstra's limitation (non-negative weights)

### Magic Phrases for Extra Marks:
✨ "This uses **optimal substructure** property..."
✨ "The **greedy choice** property holds because..."
✨ "Time complexity is O(...) due to **nested loops/sorting/recursion depth**..."
✨ "This problem exhibits **overlapping subproblems**..."
✨ "We use **memoization/tabulation** to optimize..."

---

## 💪 YOU CAN DO THIS!

**Remember:**
- Teacher gave you the priority list - FOCUS on these!
- Understand concepts, don't memorize blindly
- Draw diagrams - they help A LOT
- Examples show you understand
- Sleep is not optional!

**You've got all the material you need. Trust your preparation!** 🚀

Good luck with your exam tomorrow! 📚✨

---

**END OF PRIORITY TOPICS GUIDE**
