# 28.Find the Index of the First Occurrence in a String

Date: Feb 4
Level: Hard
Minutes: 70
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Done
Tags: KMP, String

<aside>
💡

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

</aside>

- Timer:
    - ***10min*** solution
    - ***1h*** KMP

# Tought:

- Loop haystack and check (by while)
- When the first letter appear in haystack, loop to check if full needle exist. Return when reach the end of any string.
- Evaluation:
    - Forceful solution is trivial
    - [**KMP - crucial algorithm](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E6%80%9D%E8%B7%AF)(link)**
        - **next** array: store *max equal pre/post-fix length* (copmutation!)
        - Most important step in get next array (understand):
        
        ```java
        while (j > 0 && s.charAt(j) != s.charAt(i)) { j = next[j - 1]; }
        ```
        
        - if **`needle[j]`** don’t match in **`haystack[i]`**, back to **`needle[k]`** and continue, where **k** is the max equal pre/post-fix length before **i**

# Solution

## Mine

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int nHay = haystack.length();
        int nNee = needle.length();

        for (int i = 0; i < nHay; i++) {
            int k = 0;
            while (haystack.charAt(i + k) == needle.charAt(k)) {
                k++;
                if (i + k == nHay && k < nNee) { return -1; }
                if (k == nNee) { return i; }
            }
        }

        return -1;
    }
}
```

## KMP

```java
class Solution {
    //前缀表（不减一）Java实现
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) return 0;
        int[] next = new int[needle.length()];
        getNext(next, needle);

        int j = 0;
        for (int i = 0; i < haystack.length(); i++) {
            while (j > 0 && needle.charAt(j) != haystack.charAt(i)) 
                j = next[j - 1];
            if (needle.charAt(j) == haystack.charAt(i)) 
                j++;
            if (j == needle.length()) 
                return i - needle.length() + 1;
        }
        return -1;

    }
    
    private void getNext(int[] next, String s) { 
        int j = 0; // last max euqal fix length, position after prefix
        next[0] = 0; // store j
        for (int i = 1; i < s.length(); i++) {
            while (j > 0 && s.charAt(j) != s.charAt(i)) 
                j = next[j - 1]; // !!!previous max equal fix length!!!
            if (s.charAt(j) == s.charAt(i)) 
                j++; // increase continuously
            next[i] = j; 
        }
    }
}
```