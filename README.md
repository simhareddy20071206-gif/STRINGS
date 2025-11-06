

# 🧠 STRINGS IN DSA — COMPLETE CONCEPT + PROBLEMS GUIDE

---

## 🧱 1. BASICS OF STRINGS

### 🔹 What is a String?

A **string** is a **sequence of characters**, stored as an array of characters ending with a **null character ('\0')** in C/C++, or a dynamic object in C++ STL (`std::string`).

```cpp
string s = "hello";
cout << s.length(); // 5
```

In C++ STL:

* Strings are **mutable**, i.e., you can modify them.
* Internally stored as a dynamic char array.

---

### 🔹 Common Operations

| Operation          | Example                 | Time Complexity |
| ------------------ | ----------------------- | --------------- |
| `s.length()`       | Get size                | O(1)            |
| `s[i]`             | Access character        | O(1)            |
| `s.substr(i, len)` | Extract substring       | O(len)          |
| `s.find("abc")`    | Find pattern            | O(n*m)          |
| `s + t`            | Concatenate             | O(n+m)          |
| `s.compare(t)`     | Lexicographical compare | O(min(n, m))    |

---

### 🧩 Problems:

1. Reverse a string
2. Count vowels/consonants
3. Check if a string is palindrome

---

### ✅ Example — Palindrome Check

**Concept:** Two pointers moving inward.

```cpp
bool isPalindrome(string s) {
    int l = 0, r = s.size() - 1;
    while (l < r) {
        if (s[l++] != s[r--])
            return false;
    }
    return true;
}
```

---

## ⚙️ 2. STRING TRAVERSAL & COMPARISON

### 🔹 Lexicographical Order

Just like dictionary order — `'a' < 'b'`.

You can directly use:

```cpp
if(s1 < s2) cout << "s1 comes first";
```

---

### 🔹 Reverse a String

```cpp
reverse(s.begin(), s.end());
```

or manually:

```cpp
for(int i=0, j=s.size()-1; i<j; i++, j--)
    swap(s[i], s[j]);
```

---

### 🧩 Problems:

1. Reverse words in a sentence
2. Find smallest and largest lexicographical substring
3. Rotate string (left/right)

---

## 🔢 3. CHARACTER FREQUENCY & HASHING

### 🔹 Concept:

Every string problem eventually breaks down to **frequency of characters**.

We use:

```cpp
int freq[26] = {0};
for(char c : s) freq[c - 'a']++;
```

or:

```cpp
unordered_map<char, int> freq;
for(char c : s) freq[c]++;
```

---

### 🧩 Problems:

1. **Check if two strings are anagrams**

```cpp
bool isAnagram(string s, string t) {
    if(s.size() != t.size()) return false;
    vector<int> count(26, 0);
    for(int i=0; i<s.size(); i++){
        count[s[i]-'a']++;
        count[t[i]-'a']--;
    }
    for(int c: count) if(c!=0) return false;
    return true;
}
```

2. **First non-repeating character**
3. **Group anagrams** (Leetcode 49)
4. **Rearrange string by frequency**

---

## 🪟 4. SLIDING WINDOW (VERY IMPORTANT)

### 🔹 Concept:

When you need to analyze **substrings** (continuous part of string), you use the **sliding window** technique.

Maintain a window [l..r] → move right pointer to expand, left to shrink.

---

### 🧠 Core Intuition:

Instead of checking all substrings (`O(n²)`), maintain information dynamically in `O(n)`.

---

### ✅ Example — Longest Substring Without Repeating Characters

```cpp
int lengthOfLongestSubstring(string s) {
    vector<int> last(256, -1);
    int start = 0, ans = 0;
    for(int i = 0; i < s.size(); i++) {
        if(last[s[i]] >= start)
            start = last[s[i]] + 1;
        last[s[i]] = i;
        ans = max(ans, i - start + 1);
    }
    return ans;
}
```

---

### 🧩 Problems:

1. Longest substring without repeat
2. Longest substring with at most K distinct chars
3. Minimum window substring
4. Count substring with given frequency pattern

---

## 🔍 5. PATTERN MATCHING ALGORITHMS

When you need to **search a substring (pattern) inside a larger string (text)** efficiently.

---

### 🔹 1. Brute Force

Check all starting positions
→ Time: `O(n*m)`

---

### 🔹 2. KMP Algorithm (Knuth-Morris-Pratt)

Avoids rechecking characters using **LPS (Longest Prefix Suffix)** array.
Time: `O(n+m)`

#### Code:

```cpp
vector<int> computeLPS(string pat) {
    int m = pat.size(), len = 0;
    vector<int> lps(m, 0);
    for(int i=1; i<m; ){
        if(pat[i] == pat[len])
            lps[i++] = ++len;
        else if(len != 0)
            len = lps[len-1];
        else
            lps[i++] = 0;
    }
    return lps;
}
```

---

### 🔹 3. Rabin-Karp

Use **rolling hash** for substring matching
Time: `O(n+m)` average

### 🔹 4. Z Algorithm

Computes the **Z-array** (length of substring starting at i which matches prefix).

---

### 🧩 Problems:

* Search pattern in text (Leetcode #28)
* Find number of pattern occurrences
* Find repeated substring pattern

---

## 🧮 6. STRING MANIPULATION

### 🔹 Remove adjacent duplicates

```cpp
string removeDuplicates(string s) {
    string res = "";
    for(char c : s) {
        if(!res.empty() && res.back() == c)
            res.pop_back();
        else res.push_back(c);
    }
    return res;
}
```

### 🔹 Compress string (Run-Length Encoding)

```cpp
string compress(string s) {
    string res = "";
    for(int i=0; i<s.size(); i++) {
        int cnt = 1;
        while(i+1<s.size() && s[i]==s[i+1]) { cnt++; i++; }
        res += s[i] + to_string(cnt);
    }
    return res;
}
```

---

### 🧩 Problems:

* Remove k adjacent duplicates
* Decode string
* Expand run-length encoding
* Remove specific pattern

---

## 🌲 7. TRIE (PREFIX TREE)

### 🔹 Concept:

A tree where each node represents a **character**, and every path from root to a node represents a **prefix**.

Used in:

* Word autocomplete
* Dictionary search
* Longest prefix match

---

### ✅ Example:

```cpp
struct TrieNode {
    TrieNode* child[26];
    bool isEnd;
    TrieNode() { isEnd = false; memset(child, 0, sizeof(child)); }
};
```

---

### 🧩 Problems:

1. Implement Trie (insert/search)
2. Longest common prefix
3. Word break
4. Search suggestions

---

## 🧩 8. DYNAMIC PROGRAMMING ON STRINGS

These problems deal with **comparing or transforming strings**.

| Problem                               | Core Idea                            | Time   |
| ------------------------------------- | ------------------------------------ | ------ |
| Longest Common Subsequence (LCS)      | DP[i][j] = 1 + DP[i-1][j-1] if match | O(n*m) |
| Edit Distance                         | Insert / Delete / Replace            | O(n*m) |
| Longest Palindromic Subsequence       | DP[i][j] = DP[i+1][j-1]+2 if match   | O(n²)  |
| Minimum Insertions to make Palindrome | Related to LCS                       | O(n²)  |

---

### ✅ Example — Edit Distance

```cpp
int editDistance(string a, string b) {
    int n=a.size(), m=b.size();
    vector<vector<int>> dp(n+1, vector<int>(m+1));
    for(int i=0;i<=n;i++) dp[i][0]=i;
    for(int j=0;j<=m;j++) dp[0][j]=j;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            if(a[i-1]==b[j-1]) dp[i][j]=dp[i-1][j-1];
            else dp[i][j]=1+min({dp[i-1][j],dp[i][j-1],dp[i-1][j-1]});
    return dp[n][m];
}
```

---

## 🔮 9. ADVANCED TOPICS

### 🔹 Manacher’s Algorithm

Find **Longest Palindromic Substring in O(n)** using mirrored center expansion.

### 🔹 Suffix Array

Sort all suffixes of a string — used in lexicographic comparison, substring search.

### 🔹 Rolling Hash

Efficiently compute substring hashes (used in Rabin-Karp).

---

## 🧩 10. MOST ASKED INTERVIEW PROBLEMS

| Problem                          | Concept              | Level |
| -------------------------------- | -------------------- | ----- |
| Reverse words in a string        | Two pointers         | 🟢    |
| Longest substring without repeat | Sliding Window       | 🟠    |
| Check anagram                    | Frequency            | 🟢    |
| Longest common prefix            | Trie / Sorting       | 🟡    |
| Group Anagrams                   | Hashing              | 🟠    |
| Minimum window substring         | Sliding Window       | 🔴    |
| Edit distance                    | DP                   | 🔴    |
| Longest Palindromic Substring    | Expand Around Center | 🟠    |

---

## ⚡ 11. PRACTICE ORDER (RECOMMENDED)

| Stage | Focus                                 | Days              |
| ----- | ------------------------------------- | ----------------- |
| 1     | Basics + palindrome + reverse         | 2                 |
| 2     | Frequency & anagram problems          | 2                 |
| 3     | Sliding Window (substrings)           | 3                 |
| 4     | Pattern Matching (KMP, Z, Rabin–Karp) | 3                 |
| 5     | Trie problems                         | 3                 |
| 6     | DP on strings                         | 5                 |
| 7     | Manacher’s + Suffix Array             | Optional Advanced |

---

## 🚀 12. RESOURCES

* 🔹 **YouTube:** Striver, CodeHelp by Love Babbar, TUF
* 🔹 **Leetcode Set:** #3, #5, #14, #49, #76, #242, #647
* 🔹 **Book:** “Cracking the Coding Interview” (Chapter: Strings)
* 🔹 **Practice Sites:** Leetcode, InterviewBit, Codeforces (div 2B)
