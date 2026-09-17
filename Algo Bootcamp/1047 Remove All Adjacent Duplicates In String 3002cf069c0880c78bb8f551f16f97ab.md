# 1047. Remove All Adjacent Duplicates In String

Date: Feb 6
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: Stack&Queue

<aside>
💡

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

</aside>

- Thought Process: if match, pop; else, push.
- Evaluation: simple

# Solution

```java
class Solution {
    public String removeDuplicates(String s) {
        Deque<Character> stack = new ArrayDeque<>();

        for (char c : s.toCharArray()) {
            if (!stack.isEmpty() && c == stack.peek()) {
                stack.pop();
            } else {
                stack.push(c);
            } 
        }

        StringBuilder sb = new StringBuilder();
        Iterator<Character> it = stack.descendingIterator();
        while (it.hasNext()) {
            sb.append(it.next());
        }

        return sb.toString();

        // int n = stack.size();
        // char[] res = new char[n];
        // for (int i = n - 1; i >= 0; i--) res[i] = stack.pop();
        // return new String(res)
    }
}
```