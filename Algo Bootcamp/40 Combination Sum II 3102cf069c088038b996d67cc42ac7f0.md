# 40. Combination Sum II

Date: Feb 23
Level: Medium
Minutes: 35
Review Recommendation: ✅ Good job
Status: Done
Tags: Backtracking, Deduplicate
Time Complexity: O(n * 2^n)
Space Complexity: O(T / m)
URL: https://leetcode.com/problems/combination-sum-ii/description/
Carl's: https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

</aside>

# Thought

- Similar but different from [**39. Combination Sum**](39%20Combination%20Sum%203102cf069c088097b68cf793995ac167.md) :
    - Each candidate can used only once
    - Candidates are not unique
- Requirement: **Deduplicate** (since candidate not unique)
    - Duplication(**results**): sort before add and add to a set.
        - can be reduced in improvements
    - Duplication(**horizontal**): for each loop, use a HashSet to store used candidates, if meet a duplicate one, skip it.
        - Why? All the possible combination is checked at the former same candidate.
- **Improvements**:
    - trim in for-loop condition:
    
    ```java
    for (int i = start; i < candidates.length && sum + candidates[i] <= target; i++)
    ```
    
    - **presort** → huge efficient improve
        - no need for use hashset res and sort before add path
            - automatically reduced at trimming ⬇️
        - no need for create new hashset at each level, use condition
        
        ```java
        if (i > start && candidates[i] == candidates[i - 1]) continue;
        ```
        
    - **Deduplication: result-level  → search-process**

# Solution

## Raw

```java
class Solution {
    Set<List<Integer>> res = new HashSet<>();
    Deque<Integer> path = new ArrayDeque<>();
    int sum = 0;
    
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        backtracking(candidates, target, 0);
        return new ArrayList<>(res);
    }

    private void backtracking(int[] candidates, int target, int index) {
        if (sum > target) { 
            return;
        }

        if (sum == target) {
            List<Integer> temp = new ArrayList(path);
            Collections.sort(temp); // costly
            res.add(temp);        
            return;
        }

        Set<Integer> used = new HashSet<>(); // costly
        for (int i = index; i < candidates.length; i++) { 
            int num = candidates[i];
            if (!used.add(num)) {
                continue;
            }
        
            sum += num;
            path.push(num);

            backtracking(candidates, target, i + 1);

            sum -= num;
            path.pop();
        }
    }
}
```

## Carl’s

```java
class Solution {
  List<List<Integer>> res = new ArrayList<>();
  LinkedList<Integer> path = new LinkedList<>();
  int sum = 0;

  public List<List<Integer>> combinationSum2( int[] candidates, int target ) {
    //为了将重复的数字都放到一起，所以先进行排序
    Arrays.sort( candidates );
    backTracking( candidates, target, 0 );
    return res;
  }

  private void backTracking( int[] candidates, int target, int start ) {
    if ( sum == target ) {
      res.add( new ArrayList<>( path ) );
      return;
    }
    for ( int i = start; i < candidates.length && sum + candidates[i] <= target; i++ ) {
      //正确剔除重复解的办法
      //跳过同一树层使用过的元素
      if ( i > start && candidates[i] == candidates[i - 1] ) {
        continue;
      }

      sum += candidates[i];
      path.add( candidates[i] );
      // i+1 代表当前组内元素只选取一次
      backTracking( candidates, target, i + 1 );

      int temp = path.getLast();
      sum -= temp;
      path.removeLast();
    }
  }
}
```

## Cleanest(sum→remain)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    Deque<Integer> path = new ArrayDeque<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(candidates, target, 0);
        return res;
    }

    // remain = 还差多少凑到 target
    private void backtracking(int[] candidates, int remain, int start) {
        if (remain == 0) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            int x = candidates[i];

            // 剪枝：后面只会更大
            if (x > remain) break;

            // 同一层去重：i > start 且和前一个相同就跳过
            if (i > start && candidates[i] == candidates[i - 1]) continue;

            path.push(x);
            backtracking(candidates, remain - x, i + 1);
            path.pop();
        }
    }
}
```

no need for further improvement