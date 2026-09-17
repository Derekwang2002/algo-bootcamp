# 738. Monotone Increasing Digits

Date: Mar 3
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: Greedy
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/monotone-increasing-digits/description/
Carl's: https://programmercarl.com/0738.%E5%8D%95%E8%B0%83%E9%80%92%E5%A2%9E%E7%9A%84%E6%95%B0%E5%AD%97.html

<aside>
💡

An integer has **monotone increasing digits** if and only if each pair of adjacent digits `x` and `y` satisfy `x <= y`.

Given an integer `n`, return *the largest number that is less than or equal to* `n` *with **monotone increasing digits***.

</aside>

# Thought

- Greedy strategy:
    - First split the integer by digit:
    
    ```java
    while (n > 0) {
        digits.add(n % 10);
        n /= 10;
    }
    ```
    
    - Traverse backward, detect validation of each two digits
        - if-not operation: former digit-1, latter digit set to 9.
        - missing condition: the digit behind latter digit
    - Deal with missing conditions: traverse forward, detect validation again.
        - if-not operation: set latter digit to 9
    - Restore from digits to integer:
    
    ```java
    for (int i = len - 1; i >= 0; i--) {
        n *= 10;
        n += digits.get(i);
    }
    ```
    

# Solution

```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        List<Integer> digits = new ArrayList<>();

				// Break donw
        while (n > 0) {
            digits.add(n % 10);
            n /= 10;
        }

        int len = digits.size();

				// greedy algorithm
        for (int i = 1; i < len; i++) {
            if (digits.get(i) > digits.get(i - 1)) {
                digits.set(i - 1, 9);
                digits.set(i, digits.get(i) - 1);
            }
        }

        for (int i = len - 1; i > 0; i--) {
            if (digits.get(i) > digits.get(i - 1)) {
                digits.set(i - 1, 9);
            }
        }

				// restore number
        for (int i = len - 1; i >= 0; i--) {
            n *= 10;
            n += digits.get(i);
        }

        return n;
    }
}
```