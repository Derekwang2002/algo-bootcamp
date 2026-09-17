# 459. Repeated Substring Pattern

Date: Feb 5
Level: Hard
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: MoreSolution
Tags: KMP, String

<aside>
💡

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

</aside>

# Tought process:

- Keep finding possible substring when traversing string(***n*** length)
    - Criteria: **`cur == head && prev == end`**
    - Boundary: `n / 2 + 1`
- Test when find a possible one(***k*** length), by traverse checking.
    - Criteria: **`cur != sub.charAt(i % k)` && `n % k ≠ 0`**
    - A helper function
- Evaluation:
    - “Check all possible sub.” How to ***check all***? What is ***possible***?

# Solution

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        char head = s.charAt(0);
        char end = s.charAt(n - 1);

        StringBuilder sub = new StringBuilder();
        sub.append(head);

        int pos = 1;
        boolean res = false;
        
        while (pos < n / 2 + 1 && !res) {
            char cur = s.charAt(pos);
            char prev = s.charAt(pos - 1);
            if (
                cur == head 
                && prev == end 
            ) { res = testSub(sub, s); }

            sub.append(cur);
            pos++;
        }

        return res;
    }

    private boolean testSub(StringBuilder sub, String s) {
        int k = sub.length();
        int n = s.length();
        if (n % k != 0) { return false; }

        for (int i = 0; i < n; i++) { // O(n)
            char cur = s.charAt(i);
            if (cur != sub.charAt(i % k)) { return false; }
        }

        return true;
    }
}
```

![image.png](459%20Repeated%20Substring%20Pattern/image.png)