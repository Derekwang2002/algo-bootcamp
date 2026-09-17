# 541.Reverse String II

Date: Jan 30
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DoublePointer, String

<aside>
💡

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

</aside>

# Thought:

- Basic is to do a reverse string in [**344. Reverse String**](344%20Reverse%20String%202f92cf069c0880c5ba74d28a323775d9.md) in specific range, looply
    - Use [**344. Reverse String**](344%20Reverse%20String%202f92cf069c0880c5ba74d28a323775d9.md) as private helper function
- Loop from i=0, step is 2k
    - In each loop, use ***min(k, distance to end)*** as right bound, and i as left bound

# Solution

```java
class Solution {
    public String reverseStr(String s, int k) {
        int n = s.length();
        char[] chars = s.toCharArray();

        for (int i = 0; i < n; i += 2*k) {
            int dist = n - i;
            int dest = Math.min(dist, k);
            reversePart(chars, i, i + dest - 1);
        }

        return new String(chars);
    }

    private char[] reversePart(char[] s, int i, int j) {
        int left = i, right = j;

        while (left < right) {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;

            left++;
            right--;
        }

        return s;
    }
}
```

```java
class Solution {
    public String reverseWords(String s) {
        String[] res = s.trim().split("\\s+");

        for (int i = 0, j = res.length - 1; i < j; i++, j--) {
            String temp = res[i];
            res[i] = res[j];
            res[j] = temp;
        }

        return String.join(" ", res);
    }
}
```