# 1944 Number of Visible People in a Queue 队列中可以看到的人数

__Description:__

There are `n` people standing in a queue, and they numbered from `0` to `n - 1` in __left to right__ order. You are given an array `heights` of __distinct__ integers where `heights[i]` represents the height of the `i ^ th` person.

A person can __see__ another person to their right in the queue if everybody in between is __shorter__ than both of them. More formally, the `i ^ th` person can see the `j ^ th` person if `i < j` and `min(heights[i], heights[j]) > max(heights[i+1], heights[i+2], ..., heights[j-1])`.

Return _an array_ `answer` _of length_ `n` _where_ `answer[i]` _is the __number of people__ the_ `i ^ th` _person can __see__ to their right in the queue_.

__Example:__

Example 1:

![1944-1](https://assets.leetcode.com/uploads/2021/05/29/queue-plane.jpg)

```text
Input: heights = [10,6,8,5,11,9]
Output: [3,1,2,1,1,0]
Explanation:
Person 0 can see person 1, 2, and 4.
Person 1 can see person 2.
Person 2 can see person 3 and 4.
Person 3 can see person 4.
Person 4 can see person 5.
Person 5 can see no one since nobody is to the right of them.
```

Example 2:

```text
Input: heights = [5,1,2,3,10]
Output: [4,1,1,1,0]
```

__Constraints:__

- `n == heights.length`
- `1 <= n <= 10 ^ 5`
- `1 <= heights[i] <= 10 ^ 5`
- All the values of `heights` are __unique__.

__题目描述:__

有 `n` 个人排成一个队列，__从左到右__ 编号为 `0` 到 `n - 1` 。给你以一个整数数组 `heights` ，每个整数 __互不相同__， `heights[i]` 表示第 `i` 个人的高度。

一个人能 __看到__ 他右边另一个人的条件是这两人之间的所有人都比他们两人 __矮__ 。更正式的，第 `i` 个人能看到第 `j` 个人的条件是 `i < j` 且 `min(heights[i], heights[j]) > max(heights[i+1], heights[i+2], ..., heights[j-1])` 。

请你返回一个长度为 `n` 的数组 `answer` ，其中 `answer[i]` 是第 `i` 个人在他右侧队列中能 __看到__ 的 __人数__ 。

__示例:__

示例 1：

![1944-2](https://assets.leetcode.com/uploads/2021/05/29/queue-plane.jpg)

```text
输入：heights = [10,6,8,5,11,9]
输出：[3,1,2,1,1,0]
解释：
第 0 个人能看到编号为 1 ，2 和 4 的人。
第 1 个人能看到编号为 2 的人。
第 2 个人能看到编号为 3 和 4 的人。
第 3 个人能看到编号为 4 的人。
第 4 个人能看到编号为 5 的人。
第 5 个人谁也看不到因为他右边没人。
```

示例 2：

```text
输入：heights = [5,1,2,3,10]
输出：[4,1,1,1,0]
```

__提示：__

- `n == heights.length`
- `1 <= n <= 10 ^ 5`
- `1 <= heights[i] <= 10 ^ 5`
- `heights` 中所有数 __互不相同__ 。

__思路:__

```text
单调栈
从后往前遍历
每次碰到比自己高的就弹出
同时结果加 1 表示能够看到的人数多 1
如果能看到区间不为空, 加 1 表示能够看到当前最高的
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> canSeePersonsCount(vector<int>& heights) 
    {
        stack<int> s;
        int n = heights.size();
        vector<int> result(n);
        for (int i = n - 1; i > -1; i--) 
        {
            while (!s.empty() and s.top() < heights[i]) 
            {
                s.pop();
                ++result[i];
            }
            result[i] += !s.empty();
            s.push(heights[i]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] canSeePersonsCount(int[] heights) {
        Stack<Integer> stack = new Stack<>();
        int n = heights.length, result[] = new int[n];
        for (int i = n - 1; i > -1; i--) {
            while (!stack.isEmpty() && stack.peek() < heights[i]) {
                stack.pop();
                ++result[i];
            }
            if (!stack.isEmpty()) ++result[i];
            stack.push(heights[i]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def canSeePersonsCount(self, heights: List[int]) -> List[int]:
        stack, result = deque(), [0] * (n := len(heights))
        for i in range(n - 1, -1, -1):
            while stack and stack[-1] < heights[i]:
                stack.pop()
                result[i] += 1
            result[i] += not not stack
            stack.append(heights[i])
        return result
```
