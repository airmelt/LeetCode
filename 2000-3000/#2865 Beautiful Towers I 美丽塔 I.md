# 2865 Beautiful Towers I 美丽塔 I

__Description:__

You are given an array `heights` of `n` integers representing the number of bricks in `n` consecutive towers. Your task is to remove some bricks to form a __mountain-shaped__ tower arrangement. In this arrangement, the tower heights are non-decreasing, reaching a maximum peak value with one or multiple consecutive towers and then non-increasing.

Return the maximum possible sum of heights of a mountain-shaped tower arrangement.

__Example:__

Example 1:

```text
Input: heights = [5,3,4,1,1]

Output: 13

Explanation:

We remove some bricks to make heights = [5,3,3,1,1], the peak is at index 0.
```

Example 2:

```text
Input: heights = [6,5,3,9,2,7]

Output: 22

Explanation:

We remove some bricks to make heights = [3,3,3,9,2,2], the peak is at index 3.
```

Example 3:

```text
Input: heights = [3,2,5,5,2,3]

Output: 18

Explanation:

We remove some bricks to make heights = [2,2,5,5,2,2], the peak is at index 2 or 3.
```

__Constraints:__

- `1 <= n == heights.length <= 10 ^ 3`
- `1 <= heights[i] <= 10 ^ 9`

__题目描述:__

给定一个包含 `n` 个整数的数组 `heights` 表示 `n` 座连续的塔中砖块的数量。你的任务是移除一些砖块来形成一个 __山脉状__ 的塔排列。在这种布置中，塔高度先是非递减，有一个或多个连续塔达到最大峰值，然后非递增排列。

返回满足山脉状塔排列的方案中，高度和的最大值 。

__示例:__

示例 1：

```text
输入：maxHeights = [5,3,4,1,1]
输出：13
解释：我们移除一些砖块来形成 heights = [5,3,3,1,1]，峰值位于下标 0。
```

示例 2：

```text
输入：maxHeights = [6,5,3,9,2,7]
输出：22
解释：我们移除一些砖块来形成 heights = [3,3,3,9,2,2]，峰值位于下标 3。
```

示例 3：

```text
输入：maxHeights = [3,2,5,5,2,3]
输出：18
解释：我们移除一些砖块来形成 heights = [2,2,5,5,2,2]，峰值位于下标 2 或 3。
```

__提示：__

- `1 <= n == heights.length <= 10 ^ 3`
- `1 <= heights[i] <= 10 ^ 9`

__思路:__

```text
前后缀分解 ➕ 单调栈
设以 i 为最大值, 包括 i 的前缀和为 pre, 后缀和为 suf[i]
那么最大值为 pre + suf[i + 1]
首先计算出后缀和
单调栈中记录比 i 小的下标
使用数组长度作为哨兵
如果当前值不大于单调栈栈顶
弹出元素, 令 j = stack.pop(), 并减去 j 到单调栈栈顶的个数的 heights[j], 因为这一段都会被减少到 heights[j]
然后加上 i 到栈顶个 heights[i], 这一段都会是 heights[i]
最后记录后缀和并将 i 加入栈顶
这样就能保证栈中元素是递减的
计算前缀和类似
更新结果为前后缀和的最大值即可
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
    public long maximumSumOfHeights(int[] heights) {
        int n = heights.length;
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
    def maximumSumOfHeights(self, heights: List[int]) -> int:
        suf, stack, s = [0] * ((n := len(heights)) + 1), [n], 0
        for i in range(n - 1, -1, -1):
            while len(stack) > 1 and heights[i] <= heights[stack[-1]]:
                s -= heights[j := stack.pop()] * (stack[-1] - j)
            s += heights[i] * (stack[-1] - i)
            suf[i] = s
            stack.append(i)
        result, stack, pre = s, [-1], 0
        for i, height in enumerate(heights):
            while len(stack) > 1 and height <= heights[stack[-1]]:
                pre -= heights[j := stack.pop()] * (j - stack[-1])
            pre += height * (i - stack[-1])
            result = max(result, pre + suf[i + 1])
            stack.append(i)
        return result
```
