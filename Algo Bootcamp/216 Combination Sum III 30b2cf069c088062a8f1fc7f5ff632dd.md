# 216. Combination Sum III

Date: Feb 18
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: Backtracking, Math
Time Complexity: O(n * 2^n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/combination-sum-iii/
Carl's: https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html

<aside>
💡

Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return *a list of all possible valid combinations*. The list must not contain the same combination twice, and the combinations may be returned in any order.

</aside>

# Thought

- Based on [77. Combinations](77%20Combinations%2030b2cf069c088004891cfa7e97b22c70.md) ,
    - add one more conditional statement: `sum == n`
- Be careful of all operations in forward processing, need undo **all** in backtracking

# Solutions

## Raw (fast)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    Deque<Integer> path = new ArrayDeque<>();

    public List<List<Integer>> combinationSum3(int k, int n) {
        int min = 0;
        for (int i = 1; i <= k; i++) {
            min += i;
        }

        if (n < min || n > 9*k) {
            return res;
        }

        backtracking(k, n, 1, 0);
        return res;
    }

    private void backtracking(int k, int n, int index, int sum) {
        if (path.size() == k) {
            if (sum == n) {
                res.add(new ArrayList(path));
            }
            return;
        }

        for (int i = index; i <= 9 - (k - path.size()) + 1; i++) {
            path.push(i);
            sum += i;
            backtracking(k, n, i + 1, sum);
            path.pop();
            sum -= i;
        }
    }
}
```

## Carl’s (one more prune)

```java
class Solution {
	List<List<Integer>> result = new ArrayList<>();
	LinkedList<Integer> path = new LinkedList<>();

	public List<List<Integer>> combinationSum3(int k, int n) {
		backTracking(n, k, 1, 0);
		return result;
	}

	private void backTracking(int targetSum, int k, int startIndex, int sum) {
		// 减枝
		if (sum > targetSum) {
			return;
		}

		if (path.size() == k) {
			if (sum == targetSum) result.add(new ArrayList<>(path));
			return;
		}

		// 减枝 9 - (k - path.size()) + 1
		for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) {
			path.add(i);
			sum += i;
			backTracking(targetSum, k, i + 1, sum);
			//回溯
			path.removeLast();
			//回溯
			sum -= i;
		}
	}
}
```