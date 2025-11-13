# String Algorithms 1

## 📌 KEY DEFINITIONS

**Pattern Matching**: Find occurrences of pattern P in text T

**Prefix**: Substring starting from beginning (e.g., "abc" is prefix of "abcdef")

**Suffix**: Substring ending at end (e.g., "def" is suffix of "abcdef")

**Proper Prefix/Suffix**: Prefix/suffix that is not the entire string

**Lexicographic Order**: Dictionary order for strings

---

## 📝 NAIVE PATTERN MATCHING

### Algorithm
```
NAIVE_SEARCH(text, pattern):
    n = length of text
    m = length of pattern
    
    for i = 0 to n-m:
        j = 0
        while j < m AND text[i+j] == pattern[j]:
            j++
        
        if j == m:
            print "Pattern found at index", i

Time: O(n × m)
Space: O(1)
```

**Example**:
```
Text:    "AABAACAADAABAAABAA"
Pattern: "AABA"

Check at i=0: AABA ✅ Match!
Check at i=1: ABAA ❌
Check at i=9: AABA ✅ Match!
```

---

## 🎯 KMP ALGORITHM (Knuth-Morris-Pratt)

### Key Idea
Don't re-compare characters that we know will match. Use preprocessing to avoid backtracking.

### LPS Array (Longest Proper Prefix-Suffix)

**Definition**: LPS[i] = length of longest proper prefix of pattern[0...i] which is also suffix

```
COMPUTE_LPS(pattern, m):
    lps[0] = 0  // First character has no proper prefix
    length = 0  // Length of previous longest prefix suffix
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length++
            lps[i] = length
            i++
        else:
            if length != 0:
                length = lps[length - 1]  // Try previous
            else:
                lps[i] = 0
                i++
    
    return lps

Time: O(m)
Space: O(m)
```

**Example - LPS Array**:
```
Pattern: "AABAACAABAA"
Index:    0 1 2 3 4 5 6 7 8 9 10
LPS:      0 1 0 1 2 0 1 2 3 4 5

Explanation:
A        → LPS[0] = 0 (no proper prefix)
AA       → LPS[1] = 1 (A = A)
AAB      → LPS[2] = 0 (no match)
AABA     → LPS[3] = 1 (A = A)
AABAA    → LPS[4] = 2 (AA = AA)
AABAAC   → LPS[5] = 0 (no match)
...
```

### KMP Search Algorithm
```
KMP_SEARCH(text, pattern):
    n = length of text
    m = length of pattern
    
    // Preprocess pattern
    lps = COMPUTE_LPS(pattern, m)
    
    i = 0  // Index for text
    j = 0  // Index for pattern
    
    while i < n:
        if pattern[j] == text[i]:
            i++
            j++
        
        if j == m:
            print "Pattern found at index", i-j
            j = lps[j-1]  // Continue searching
        
        else if i < n AND pattern[j] != text[i]:
            if j != 0:
                j = lps[j-1]  // Don't match lps[0..lps[j-1]]
            else:
                i++

Time: O(n + m)
Space: O(m)
```

**Key Advantage**: Never moves backward in text, only in pattern

---

## 🔍 RABIN-KARP ALGORITHM

### Key Idea
Use hashing to find pattern. If hashes match, verify character by character.

### Rolling Hash Formula
```
For string s of length m:
hash(s) = (s[0] × d^(m-1) + s[1] × d^(m-2) + ... + s[m-1]) % q

Where:
- d = number of characters in alphabet (usually 256)
- q = large prime number (to avoid collisions)
```

### Pseudocode
```
RABIN_KARP(text, pattern):
    n = length of text
    m = length of pattern
    d = 256  // Number of characters
    q = 101  // A prime number
    
    h = d^(m-1) % q  // Hash value for highest position
    patternHash = 0
    textHash = 0
    
    // Calculate hash of pattern and first window
    for i = 0 to m-1:
        patternHash = (d × patternHash + pattern[i]) % q
        textHash = (d × textHash + text[i]) % q
    
    // Slide pattern over text
    for i = 0 to n-m:
        // Check if hashes match
        if patternHash == textHash:
            // Verify character by character
            if text[i...i+m-1] == pattern:
                print "Pattern found at index", i
        
        // Calculate hash for next window
        if i < n-m:
            textHash = (d × (textHash - text[i] × h) + text[i+m]) % q
            
            // Handle negative hash
            if textHash < 0:
                textHash += q

Time: O(n + m) average, O(nm) worst case
Space: O(1)
```

**Rolling Hash Example**:
```
Pattern: "ABC"
Text: "ABCABC"
d = 256, q = 101

patternHash = (65×256² + 66×256 + 67) % 101 = ...
textHash[0] = same ✅

Slide window:
Remove 'A', Add 'A': 
textHash[1] = (textHash[0] - 65×256²)×256 + 65 % 101
```

---

## 🌲 TRIE (PREFIX TREE)

### Key Definitions

**Trie**: Tree structure for storing strings efficiently

**Trie Node**: Contains:
- Children array (26 for lowercase, 256 for all ASCII)
- Boolean flag for end of word

### Structure
```
CLASS TrieNode:
    children[26]  // For 'a' to 'z'
    isEndOfWord = false
```

### Operations

#### 1. INSERT
```
INSERT(root, word):
    current = root
    
    for each char in word:
        index = char - 'a'
        
        if current.children[index] == NULL:
            current.children[index] = new TrieNode()
        
        current = current.children[index]
    
    current.isEndOfWord = true

Time: O(L) where L = length of word
Space: O(L)
```

#### 2. SEARCH
```
SEARCH(root, word):
    current = root
    
    for each char in word:
        index = char - 'a'
        
        if current.children[index] == NULL:
            return false
        
        current = current.children[index]
    
    return current.isEndOfWord

Time: O(L)
Space: O(1)
```

#### 3. DELETE
```
DELETE(root, word, depth):
    if root == NULL:
        return NULL
    
    // Base case: reached end of word
    if depth == length of word:
        if root.isEndOfWord:
            root.isEndOfWord = false
        
        // If node has no children, delete it
        if isEmpty(root):
            return NULL
        
        return root
    
    // Recursive case
    index = word[depth] - 'a'
    root.children[index] = DELETE(root.children[index], word, depth+1)
    
    // If root has no children and is not end of another word
    if isEmpty(root) AND root.isEndOfWord == false:
        return NULL
    
    return root

Time: O(L)
Space: O(L) for recursion
```

#### 4. PREFIX SEARCH (Autocomplete)
```
STARTS_WITH(root, prefix):
    current = root
    
    for each char in prefix:
        index = char - 'a'
        
        if current.children[index] == NULL:
            return false
        
        current = current.children[index]
    
    return true

Time: O(L)
```

### Visual Example
```
Words: "cat", "car", "dog", "dot"

         root
        /    \
       c      d
       |      |
       a      o
      / \    / \
     t   r  g   t
     *   *  *   *

* = end of word
```

---

## 💡 APPLICATIONS

### 1. Pattern Matching
- **Naive**: Simple, no preprocessing
- **KMP**: Best for single pattern, linear time
- **Rabin-Karp**: Good for multiple patterns

### 2. Trie Applications
- **Autocomplete**: Find all words with prefix
- **Spell Checker**: Check if word exists
- **IP Routing**: Longest prefix matching
- **Dictionary**: Fast lookup
- **T9 Predictive Text**

---

## 📊 ALGORITHM COMPARISON

| Algorithm | Preprocessing | Search | Space | Best For |
|-----------|--------------|--------|-------|----------|
| **Naive** | O(1) | O(nm) | O(1) | Small inputs |
| **KMP** | O(m) | O(n) | O(m) | Single pattern |
| **Rabin-Karp** | O(m) | O(n+m) avg | O(1) | Multiple patterns |
| **Trie** | O(NL) | O(L) | O(NL) | Many searches |

Where: n = text length, m = pattern length, N = number of words, L = average word length

---

## 🚨 COMMON EXAM MISTAKES

❌ KMP: Wrong LPS calculation

❌ Rabin-Karp: Forgetting modulo operation or handling negative hash

❌ Trie: Wrong index calculation (char - 'a')

❌ Confusing time complexities

---

## 📖 MUST REMEMBER

1. **Naive**: O(nm) - check every position
2. **KMP**: O(n+m) - uses LPS array, never backtracks in text
3. **LPS[i]**: Length of longest proper prefix-suffix for pattern[0...i]
4. **Rabin-Karp**: Hashing with rolling hash, O(n+m) average
5. **Trie**: Tree structure, O(L) operations where L = word length
6. **Trie Space**: O(ALPHABET_SIZE × N × L) in worst case
7. **KMP best for**: Single pattern matching
8. **Trie best for**: Multiple lookups with same dictionary
