# 2866 Beautiful Towers II 美丽塔 II

__Description:__

You are given a __0-indexed__ array `maxHeights` of `n` integers.

You are tasked with building `n` towers in the coordinate line. The `i ^ th` tower is built at coordinate `i` and has a height of `heights[i]`.

A configuration of towers is beautiful if the following conditions hold:

Array `heights` is a __mountain__ if there exists an index `i` such that:

- For all `0 < j <= i`, `heights[j - 1] <= heights[j]`
- For all `i <= k < n - 1`, `heights[k + 1] <= heights[k]`

Return the maximum possible sum of heights of a beautiful configuration of towers.

__Example:__

Example 1:

```text
Input: maxHeights = [5,3,4,1,1]
Output: 13
Explanation: One beautiful configuration with a maximum sum is heights = [5,3,3,1,1]. This configuration is beautiful since:
```

- 1 <= heights[i] <= maxHeights[i]  
- heights is a mountain of peak i = 0.

It can be shown that there exists no other beautiful configuration with a sum of heights greater than 13.

Example 2:

```text
Input: maxHeights = [6,5,3,9,2,7]
Output: 22
Explanation: One beautiful configuration with a maximum sum is heights = [3,3,3,9,2,2]. This configuration is beautiful since:
```

- 1 <= heights[i] <= maxHeights[i]
- heights is a mountain of peak i = 3.

It can be shown that there exists no other beautiful configuration with a sum of heights greater than 22.

Example 3:

```text
Input: maxHeights = [3,2,5,5,2,3]
Output: 18
Explanation: One beautiful configuration with a maximum sum is heights = [2,2,5,5,2,2]. This configuration is beautiful since:
```

- 1 <= heights[i] <= maxHeights[i]
- heights is a mountain of peak i = 2.

Note that, for this configuration, i = 3 can also be considered a peak.
It can be shown that there exists no other beautiful configuration with a sum of heights greater than 18.

__Constraints:__

- `1 <= n == maxHeights.length <= 10 ^ 5`
- `1 <= maxHeights[i] <= 10 ^ 9`

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `maxHeights` 。

你的任务是在坐标轴上建 `n` 座塔。第 `i` 座塔的下标为 `i` ，高度为 `heights[i]` 。

如果以下条件满足，我们称这些塔是 美丽 的：

如果存在下标 `i` 满足以下条件，那么我们称数组 `heights` 是一个 __山脉__ 数组:

- 对于所有 `0 < j <= i` ，都有 `heights[j - 1] <= heights[j]`
- 对于所有 `i <= k < n - 1` ，都有 `heights[k + 1] <= heights[k]`

请你返回满足 美丽塔 要求的方案中，高度和的最大值 。

__示例:__

示例 1：

```text
输入：maxHeights = [5,3,4,1,1]
输出：13
解释：和最大的美丽塔方案为 heights = [5,3,3,1,1] ，这是一个美丽塔方案，因为：
```

- 1 <= heights[i] <= maxHeights[i]  
- heights 是个山脉数组，峰值在 i = 0 处。

13 是所有美丽塔方案中的最大高度和。

示例 2：

```text
输入：maxHeights = [6,5,3,9,2,7]
输出：22
解释： 和最大的美丽塔方案为 heights = [3,3,3,9,2,2] ，这是一个美丽塔方案，因为：
```

- 1 <= heights[i] <= maxHeights[i]
- heights 是个山脉数组，峰值在 i = 3 处。

22 是所有美丽塔方案中的最大高度和。

示例 3：

```text
输入：maxHeights = [3,2,5,5,2,3]
输出：18
解释：和最大的美丽塔方案为 heights = [2,2,5,5,2,2] ，这是一个美丽塔方案，因为：
```

- 1 <= heights[i] <= maxHeights[i]
- heights 是个山脉数组，最大值在 i = 2 处。

注意，在这个方案中，i = 3 也是一个峰值。
18 是所有美丽塔方案中的最大高度和。

__提示：__

- `1 <= n == maxHeights <= 10 ^ 5`
- `1 <= maxHeights[i] <= 10 ^ 9`

__思路:__

```text
前后缀分解 ➕ 单调栈
本题与 2865 完全相同, 数据量不同
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumSumOfHeights(vector<int>& heights) 
    {
        int n = heights.size();
        long long pre = 0LL, cur = 0LL, result = 0LL;
        vector<long long> suf(n + 1);
        stack<int> s;
        s.push(n);
        for (int i = n - 1, j = 0; i > -1; i--) 
        {
            while (s.size() > 1 and heights[s.top()] >= heights[i]) 
            {
                j = s.top();
                s.pop();
                cur -= (long long)heights[j] * (s.top() - j);
            }
            cur += (long long)heights[i] * (s.top() - i);
            s.push(i);
            suf[i] = cur;
        }
        while (!s.empty()) s.pop();
        s.push(-1);
        for (int i = 0, j = 0; i < n; i++) 
        {
            while (s.size() > 1 and heights[s.top()] >= heights[i]) 
            {
                j = s.top();
                s.pop();
                pre -= (long long)heights[j] * (j - s.top());
            }
            pre += (long long)heights[i] * (i - s.top());
            s.push(i);
            result = max(result, pre + suf[i + 1]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumSumOfHeights(List<Integer> maxHeights) {
        int n = maxHeights.size(), heights[] = new int[n];
        for (int i = 0; i < n; i++) heights[i] = maxHeights.get(i);
        long pre = 0L, s = 0L, result = 0L, suf[] = new long[n + 1];
        var stack = new Stack<Integer>();
        stack.push(n);
        for (int i = n - 1, j = 0; i > -1; i--) {
            while (stack.size() > 1 && heights[stack.peek()] >= heights[i]) s -= (long)heights[j = stack.pop()] * (stack.peek() - j);
            s += (long)heights[i] * (stack.peek() - i);
            stack.push(i);
            suf[i] = s;
        }
        stack.clear();
        stack.push(-1);
        for (int i = 0, j = 0; i < n; i++) {
            while (stack.size() > 1 && heights[stack.peek()] >= heights[i]) pre -= (long)heights[j = stack.pop()] * (j - stack.peek());
            pre += (long)heights[i] * (i - stack.peek());
            stack.push(i);
            result = Math.max(result, pre + suf[i + 1]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSumOfHeights(self, maxHeights: List[int]) -> int:
        result, s, stack, suf, pre = 0, 0, [n := len(maxHeights)], [0] * (n + 1), 0
        for i in range(n - 1, -1, -1):
            while len(stack) > 1 and maxHeights[stack[-1]] >= maxHeights[i]:
                s -= maxHeights[j := stack.pop()] * (stack[-1] - j)
            suf[i] = (s := s + maxHeights[i] * (stack[-1] - i))
            stack.append(i)
        stack = [-1]
        for i, height in enumerate(maxHeights):
            while len(stack) > 1 and maxHeights[stack[-1]] >= height:
                pre -= maxHeights[j := stack.pop()] * (j - stack[-1])
            result = max(result, suf[i + 1] + (pre := pre + height * (i - stack[-1])))
            stack.append(i)
        return result
```
