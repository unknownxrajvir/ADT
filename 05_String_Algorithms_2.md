# String Algorithms 2

## 📌 KEY DEFINITIONS

**Z-Algorithm**: Computes Z-array where Z[i] = length of longest substring starting from i that matches prefix

**Suffix Array**: Sorted array of all suffixes of a string

**Suffix Tree**: Compressed trie of all suffixes of a string

**Aho-Corasick**: Multiple pattern matching using trie with failure links

**Manacher's Algorithm**: Finds longest palindromic substring in O(n)

---

## 🎯 Z-ALGORITHM

### Z-Array Definition

**Z[i]** = Length of longest substring starting from i that matches prefix of string

**Example**:
```
String: "aabcaabxaaz"
Index:   0 1 2 3 4 5 6 7 8 9 10

Z-array: [0, 1, 0, 0, 3, 1, 0, 0, 2, 1, 0]

Explanation:
Z[0] = 0 (by definition)
Z[1] = 1 (a matches)
Z[4] = 3 (aab matches)
Z[8] = 2 (aa matches)
```

### Pseudocode
```
Z_ALGORITHM(s, n):
    Z[0] = 0
    left = 0, right = 0
    
    for i = 1 to n-1:
        if i > right:
            // Calculate from scratch
            left = right = i
            while right < n AND s[right] == s[right - left]:
                right++
            Z[i] = right - left
            right--
        else:
            // Inside Z-box, use previously computed values
            k = i - left
            
            if Z[k] < right - i + 1:
                Z[i] = Z[k]
            else:
                left = i
                while right < n AND s[right] == s[right - left]:
                    right++
                Z[i] = right - left
                right--
    
    return Z

Time: O(n)
Space: O(n)
```

### Pattern Matching using Z-Algorithm
```
Z_PATTERN_MATCH(text, pattern):
    // Concatenate pattern + $ + text
    concat = pattern + "$" + text
    n = length of concat
    
    Z = Z_ALGORITHM(concat)
    m = length of pattern
    
    for i = m+1 to n-1:
        if Z[i] == m:
            print "Pattern found at index", i - m - 1

Time: O(n + m)
Space: O(n + m)
```

---

## 🔍 AHO-CORASICK ALGORITHM

### Key Idea
Build trie of all patterns, add failure links for efficient multiple pattern matching

### Components
1. **Trie**: Store all patterns
2. **Failure Links**: Where to go on mismatch
3. **Output Links**: Which patterns end at this node

### Pseudocode
```
CLASS AhoCorasickNode:
    children[ALPHABET_SIZE]
    failureLink
    output = []  // List of pattern indices ending here

BUILD_AUTOMATON(patterns):
    root = new AhoCorasickNode()
    
    // Step 1: Build Trie
    for each pattern in patterns:
        current = root
        for each char in pattern:
            if current.children[char] == NULL:
                current.children[char] = new AhoCorasickNode()
            current = current.children[char]
        current.output.append(pattern_index)
    
    // Step 2: Build Failure Links (BFS)
    queue = empty
    
    // All children of root fail to root
    for each child of root:
        if child != NULL:
            child.failureLink = root
            queue.enqueue(child)
    
    while queue is not empty:
        current = queue.dequeue()
        
        for each char in alphabet:
            child = current.children[char]
            if child != NULL:
                queue.enqueue(child)
                
                // Find failure link
                temp = current.failureLink
                while temp != root AND temp.children[char] == NULL:
                    temp = temp.failureLink
                
                if temp.children[char] != NULL AND temp.children[char] != child:
                    child.failureLink = temp.children[char]
                else:
                    child.failureLink = root
                
                // Merge outputs
                child.output.merge(child.failureLink.output)
    
    return root

SEARCH(text, automaton_root):
    current = automaton_root
    
    for i = 0 to length(text)-1:
        char = text[i]
        
        // Follow failure links until match found
        while current != root AND current.children[char] == NULL:
            current = current.failureLink
        
        if current.children[char] != NULL:
            current = current.children[char]
        
        // Report all patterns ending at this position
        for each pattern_index in current.output:
            print "Pattern", pattern_index, "found ending at", i

Time: O(n + m + z) where n=text, m=total pattern length, z=output size
Space: O(m × ALPHABET_SIZE)
```

---

## 📊 SUFFIX ARRAY

### Definition
Array of integers representing starting positions of suffixes in lexicographic order

### Example
```
String: "banana"
Index:   0 1 2 3 4 5

All Suffixes:
0: banana
1: anana
2: nana
3: ana
4: na
5: a

Sorted Suffixes:
5: a
3: ana
1: anana
0: banana
4: na
2: nana

Suffix Array: [5, 3, 1, 0, 4, 2]
```

### Construction (Simple O(n² log n))
```
BUILD_SUFFIX_ARRAY_SIMPLE(s, n):
    suffixes = array of (index, suffix_string)
    
    for i = 0 to n-1:
        suffixes[i] = (i, s[i...n-1])
    
    sort suffixes by suffix_string
    
    suffixArray = []
    for each (index, suffix) in suffixes:
        suffixArray.append(index)
    
    return suffixArray

Time: O(n² log n)
Space: O(n²)
```

### Efficient Construction (O(n log n))
```
BUILD_SUFFIX_ARRAY_EFFICIENT(s, n):
    // Use radix sort with doubling technique
    // Rank suffixes by first 2^k characters
    
    suffixArray = [0...n-1]
    rank = [s[i] for i in 0...n-1]
    tempRank = array of size n
    k = 1
    
    while k < n:
        // Sort by (rank[i], rank[i+k])
        sort suffixArray using comparison:
            compare (rank[sa[i]], rank[sa[i]+k]) with (rank[sa[j]], rank[sa[j]+k])
        
        // Update ranks
        tempRank[suffixArray[0]] = 0
        for i = 1 to n-1:
            tempRank[suffixArray[i]] = tempRank[suffixArray[i-1]]
            if rank differs:
                tempRank[suffixArray[i]]++
        
        rank = tempRank
        k *= 2
    
    return suffixArray

Time: O(n log n)
Space: O(n)
```

### Applications
1. **Pattern Matching**: Binary search on suffix array - O(m log n)
2. **Longest Common Substring**: Find consecutive suffixes in array
3. **Longest Repeated Substring**: Adjacent suffixes with longest common prefix

---

## 🌳 SUFFIX TREE

### Definition
Compressed trie of all suffixes. Each edge labeled with substring.

### Key Properties
- O(n) construction time (Ukkonen's algorithm)
- O(n) space
- Pattern matching in O(m) time

### Visual Example
```
String: "banana$"

Suffix Tree (simplified):
              root
           /   |    \
          $   a      banana$
             /|\
            $ na$  na$
               |
              na$
```

### Applications
1. **Pattern Matching**: O(m) - traverse tree
2. **Longest Repeated Substring**: Find deepest internal node
3. **Longest Common Substring**: Build generalized suffix tree
4. **Longest Palindrome**: Combine string with reverse

### Pseudocode (Conceptual - Ukkonen's is complex)
```
BUILD_SUFFIX_TREE(s):
    // Ukkonen's algorithm builds in O(n)
    // Uses active point, remainder, and rules
    // Too complex for exam - know applications!

Time: O(n)
Space: O(n)
```

---

## 🎨 MANACHER'S ALGORITHM

### Problem
Find longest palindromic substring in O(n)

### Key Idea
Transform string to handle even/odd length palindromes uniformly

### Transformation
```
"aba" → "#a#b#a#"
"abba" → "#a#b#b#a#"
```

### Pseudocode
```
MANACHER(s):
    // Transform string
    T = "#"
    for each char in s:
        T += char + "#"
    
    n = length of T
    P = array of size n  // P[i] = radius of palindrome centered at i
    center = 0
    right = 0
    
    for i = 0 to n-1:
        mirror = 2 × center - i
        
        if i < right:
            P[i] = min(right - i, P[mirror])
        
        // Expand around center
        while i + P[i] + 1 < n AND i - P[i] - 1 >= 0:
            if T[i + P[i] + 1] == T[i - P[i] - 1]:
                P[i]++
            else:
                break
        
        // Update center and right
        if i + P[i] > right:
            center = i
            right = i + P[i]
    
    // Find max palindrome
    maxLen = 0
    centerIndex = 0
    for i = 0 to n-1:
        if P[i] > maxLen:
            maxLen = P[i]
            centerIndex = i
    
    start = (centerIndex - maxLen) / 2
    return s[start...start+maxLen-1]

Time: O(n)
Space: O(n)
```

### Example
```
String: "babad"
Transformed: "#b#a#b#a#d#"

P array: [0,1,0,3,0,3,0,1,0,1,0]
         # b # a # b # a # d #

Max at index 3 (or 5): "aba" or "bab"
```

---

## 📊 ALGORITHM COMPARISON

| Algorithm | Preprocessing | Search | Space | Best For |
|-----------|--------------|--------|-------|----------|
| **Z-Algorithm** | O(n) | O(n) | O(n) | Single pattern, linear |
| **Aho-Corasick** | O(m) | O(n+z) | O(m×k) | Multiple patterns |
| **Suffix Array** | O(n log n) | O(m log n) | O(n) | Space-efficient, multiple queries |
| **Suffix Tree** | O(n) | O(m) | O(n) | Fastest search, many applications |
| **Manacher** | O(n) | - | O(n) | Longest palindrome only |

m = pattern length, n = text length, k = alphabet size, z = output size

---

## 💡 WHEN TO USE WHICH

### Single Pattern Matching
- **KMP** or **Z-Algorithm**: O(n+m), simple

### Multiple Pattern Matching
- **Aho-Corasick**: Best for many patterns

### Many Queries on Same Text
- **Suffix Array**: Less space
- **Suffix Tree**: Faster queries

### Palindrome Problems
- **Manacher's**: O(n) for longest palindrome

---

## 🚨 COMMON EXAM QUESTIONS

1. **Build Z-array** for given string
2. **Build Suffix Array** for given string
3. **Compare** suffix array vs suffix tree
4. **Pattern matching** using suffix structures
5. **Find longest palindrome** using Manacher's

---

## 📖 MUST REMEMBER

1. **Z[i]**: Longest substring from i matching prefix
2. **Z-Algorithm**: O(n) using Z-box optimization
3. **Aho-Corasick**: Trie + failure links for multiple patterns
4. **Suffix Array**: Sorted indices of suffixes, O(n log n) construction
5. **Suffix Tree**: Compressed trie of suffixes, O(n) construction
6. **Manacher's**: Transform string with #, expand around centers
7. **Pattern match in suffix array**: Binary search O(m log n)
8. **Pattern match in suffix tree**: Tree traversal O(m)
9. **Longest repeated substring**: Use suffix array/tree
10. **Multiple patterns**: Use Aho-Corasick
