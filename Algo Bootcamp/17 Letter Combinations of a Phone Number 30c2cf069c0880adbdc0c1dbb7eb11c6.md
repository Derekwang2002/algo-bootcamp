# 17. Letter Combinations of a Phone Number

Date: Feb 18
Level: Medium
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: Backtracking
Time Complexity: O(3^m * 4^n)
Space Complexity: O(3^m * 4^n)
URL: https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/
Carl's: https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html

<aside>
💡

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![image.png](17%20Letter%20Combinations%20of%20a%20Phone%20Number/image.png)

</aside>

# Thought

- Build a `Map<Integer, List<Character>>` to store button-letter info.
- Recursion tree:
    - Width: unused digits’ letters * (#parents nodes)
    - Depth: length of given digits, or #digits of a letter combination
    
    ```mermaid
    graph TD
        root["23"] --> a["a"]
        root --> b["b"]
        root --> c["c"]
    
        a --> ad["ad"]
        a --> ae["ae"]
        a --> af["af"]
    
        b --> bd["bd"]
        b --> be["be"]
        b --> bf["bf"]
        
        c --> cd["cd"]
        c --> ce["ce"]
        c --> cf["cf"]
    
    ```
    
- Backtracking recursion Design:
    - Return void, take parameters `String s`, `index` for current digit, `StringBuilder` to store result of current path.
    - Base case: length reach end
    - Steps: (add) go down for each possible letter (out)
- Evaluation:
    - Backtracking solved the problem that *for-loop brute-force can’t be written out*.
    - Simple array could replace map

# Solution

## Raw

```java
class Solution {
    List<String> res = new ArrayList<>();
    Map<Integer, List<Character>> map = new HashMap<>();

    public List<String> letterCombinations(String digits) {
        int k = 0;

        for (int i = 2; i <= 9; i++) {
            List<Character> button = new ArrayList<>();
            for (int c = 0; c < 3; c++) {
                button.add((char)('a' + k++));
            }
            if (i == 7 || i == 9) {
                button.add((char)('a' + k++));
            }
            map.put(i, button);
        }
        
        backtracking(digits, 0, new StringBuilder());

        return res;
    }

    private void backtracking(String s, int index, StringBuilder sb) {
        if (s.length() == sb.length()) {
            res.add(sb.toString());
            return;
        }

        List<Character> button = map.get(s.charAt(index) - '0');

        for (char c : button) { // int i = 0; i < button.size(); i++
            sb.append(c);
            backtracking(s, index + 1, sb);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

## Carl’s

```java
class Solution {

    //设置全局列表存储最后的结果
    List<String> list = new ArrayList<>();

    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return list;
        }
        //初始对应所有的数字，为了直接对应2-9，新增了两个无效的字符串""
        String[] numString = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        //迭代处理
        backTracking(digits, numString, 0);
        return list;

    }

    //每次迭代获取一个字符串，所以会涉及大量的字符串拼接，所以这里选择更为高效的 StringBuilder
    StringBuilder temp = new StringBuilder();

    //比如digits如果为"23",num 为0，则str表示2对应的 abc
    public void backTracking(String digits, String[] numString, int num) {
        //遍历全部一次记录一次得到的字符串
        if (num == digits.length()) {
            list.add(temp.toString());
            return;
        }
        //str 表示当前num对应的字符串
        String str = numString[digits.charAt(num) - '0'];
        for (int i = 0; i < str.length(); i++) {
            temp.append(str.charAt(i));
            //递归，处理下一层
            backTracking(digits, numString, num + 1);
            //剔除末尾的继续尝试
            temp.deleteCharAt(temp.length() - 1);
        }
    }
}
```