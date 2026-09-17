# KamaCoder 58. 区间和

Date: Jan 23
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: Array, PrefixSum

## Decription

<aside>
💡

给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间下标：a，b （b > = a），直至文件结束。

</aside>

# Thought

- 直接计算时间复杂度为 $O(n^2)$，超时
- “**前缀和**”思路：
    - 记录每个位置的**累加和 (prefix sum)**，区间 $[left,right]$ 内的和可以表示为:
    
    $$
     left\ 处的累加和 -\ (right-1)\ 处的累加和
    $$
    
    - 由于右侧`index`前移了一位，所以需要在`prefix_sum`数组前加一位`0`保护`right = 0`的情况
- 考察了 **输入的读取**

# Solution

## Mine

### 初始版本：直接思路，超时

```python
import sys

data = sys.stdin.read()
input_nums = list(map(int, data.split()))

n = input_nums[0]
input_array = [0] * n
for i in range(n):
    input_array[i] = input_nums[i+1]

ranges = []
for i in range(n+1, len(input_nums), 2):
    ranges.append([input_nums[i], input_nums[i+1]])

res = 0
for r in ranges:
    for k in range(r[0], r[1]+1):
        res += input_array[k]

    print(res)
    res = 0
```

### 通过版：由评论区提到的“前缀和”思想改进

- 简化了变量命名

```python
import sys

data = sys.stdin.read()
inputs = list(map(int, data.split()))

n = inputs[0]
nums = [0] * n
prefix_sum = [0] * (n+1)
s = 0

for i in range(n):
    nums[i] = inputs[i+1]
    s += nums[i]
    prefix_sum[i+1] = s
    

for i in range(n+1, len(inputs), 2):
    r, l = inputs[i] + 1, inputs[i+1] + 1 # + 1 is to track prefix_sum's index
    print(prefix_sum[l] - prefix_sum[r-1]) # key: rearer than r, means we need prefix_sum for [-1]
```

## Carl’s

```python
import sys
input = sys.stdin.read

def main():
    data = input().split()
    index = 0
    n = int(data[index])
    index += 1
    vec = []
    for i in range(n):
        vec.append(int(data[index + i]))
    index += n

    p = [0] * n
    presum = 0
    for i in range(n):
        presum += vec[i]
        p[i] = presum

    results = []
    while index < len(data):
        a = int(data[index])
        b = int(data[index + 1])
        index += 2

        if a == 0:
            sum_value = p[b]
        else:
            sum_value = p[b] - p[a - 1]

        results.append(sum_value)

    for result in results:
        print(result)

if __name__ == "__main__":
    main()
```