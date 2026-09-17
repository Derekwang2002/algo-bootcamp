# 131. Palindrome Partitioning

Date: Feb 23
Level: Hard
Minutes: 55
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Done
Tags: Backtracking
Time Complexity: O(n * 2^n)
Space Complexity: O(n^2)
URL: https://leetcode.com/problems/palindrome-partitioning/
Carl's: https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html

<aside>
💡

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return *all possible palindrome partitioning of* `s`.

</aside>

# Thought

- 2 helper function
    - one for backtracking
    - onr for judge isPalindrome
- Backtracking recursion design
    - Divide: virtually cut, use idx as start index
    - Title: `void backtrack(String s, int idx)`
    - Base case: idx ≥ length, add path and return
        - only valid path can reach here
    - Steps(conquer): loop all combines starts with idx
        - check palindrome first, continue if not valid
        - do backtracking with path(add/remove substring)
- Tree:
    
    ```
    aab
    ├── a | ab
    │   ├── a | b
    │   │   └── b ✓
    │   └── ab ✗
    ├── aa | b
    │   └── b ✓
    └── aab ✗
    ```
    
- Evaluation:
    - **Difficulies**
        - The partitioning problem can be abstracted as a combination problem.
        - How to simulate those cut lines.
        - How to stop the recursion in a partitioning problem.
        - How to extract substrings inside a recursive loop.
        - How to check whether a string is a palindrome.
    - Improvements: Use *dynamic programming* to determine palindrome
        - link

# Solution

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> path = new ArrayList<>();

    public List<List<String>> partition(String s) {
        backtrack(s, 0);
        return res;
    }

    private void backtrack(String s, int idx) {
        if (idx >= s.length()) {
            res.add(new ArrayList<>(path)); // must new
            return;
        }

        for (int i = idx; i < s.length(); i++) {
            if (!isPalindrome(s, idx, i)) continue;

            path.add(s.substring(idx, i + 1));
            backtrack(s, i + 1);
            path.remove(path.size() - 1);
        }
    }

    private boolean isPalindrome(String s, int i, int j) {
        int front = i;
        int rear = j;

        while (front <= rear) {
            if (s.charAt(front) != s.charAt(rear)) {
                return false;
            }

            front++;
            rear--;
        }

        return true;
    }
}
```