# 1124 Longest Well-Performing Interval 表现良好的最长时间段

__Description__:
We are given hours, a list of the number of hours worked per day for a given employee.

A day is considered to be a tiring day if and only if the number of hours worked is (strictly) greater than 8.

A well-performing interval is an interval of days for which the number of tiring days is strictly larger than the number of non-tiring days.

Return the length of the longest well-performing interval.

__Example:__

Example 1:

Input: hours = [9,9,6,0,6,6,9]
Output: 3
Explanation: The longest well-performing interval is [9,9,6].

Example 2:

Input: hours = [6,6,6]
Output: 0

__Constraints:__

1 <= hours.length <= 10^4
0 <= hours[i] <= 16

__题目描述__:
给你一份工作时间表 hours，上面记录着某一位员工每天的工作小时数。

我们认为当员工一天中的工作小时数大于 8 小时的时候，那么这一天就是「劳累的一天」。

所谓「表现良好的时间段」，意味在这段时间内，「劳累的天数」是严格 大于「不劳累的天数」。

请你返回「表现良好时间段」的最大长度。

__示例 :__

示例 1：

输入：hours = [9,9,6,0,6,6,9]
输出：3
解释：最长的表现良好时间段是 [9,9,6]。

示例 2：

输入：hours = [6,6,6]
输出：0

__提示:__

1 <= hours.length <= 10^4
0 <= hours[i] <= 16

__思路__:

单调栈 ➕ 前缀和
先将 hours 数组按照是否大于 8 赋值 1/-1
求取新数组的前缀和
转化为在前缀和数组中找到一个子数组, 使得子数组的和大于 0, 且长度最长
即 max(j - i), s.t. pre[j] - pre[i] > 0, 0 < i < j < n
先将前缀和中的数按照严格递减顺序加入单调栈
然后从后往前遍历前缀和数组, 将当前下标之后的单调栈元素进行比较得到最长的子数组
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestWPI(vector<int>& hours) 
    {
        int n = hours.size(), result = 0, i = 0;
        vector<int> score(n), pre(n + 1);
        stack<int> s;
        for (i = 0; i < n; i++) score[i] = hours[i] > 8 ? 1 : -1;
        for (i = 1; i <= n; i++) pre[i] = pre[i - 1] + score[i - 1];
        for (i = 0; i <= n; i++) if (s.empty() or pre[s.top()] > pre[i]) s.push(i);
        for (i = n; i > result; i--)
        {
            while (!s.empty() and pre[s.top()] < pre[i])
            {
                result = max(result, i - s.top());
                s.pop();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestWPI(int[] hours) {
        int n = hours.length, result = 0, score[] = new int[n], pre[] = new int[n + 1], i = 0;
        Stack<Integer> stack = new Stack<>();
        for (i = 0; i < n; i++) score[i] = hours[i] > 8 ? 1 : -1;
        for (i = 1; i <= n; i++) pre[i] = pre[i - 1] + score[i - 1];
        for (i = 0; i <= n; i++) if (stack.isEmpty() || pre[stack.peek()] > pre[i]) stack.push(i);
        for (i = n; i > result; i--) while (!stack.isEmpty() && pre[stack.peek()] < pre[i]) result = Math.max(result, i - stack.pop());
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestWPI(self, hours: List[int]) -> int:
        n, pre, result, stack = len(hours), [0] + list(accumulate([1 if h > 8 else -1 for h in hours])), 0, []
        for i in range(n + 1):
            if not stack or pre[stack[-1]] > pre[i]:
                stack.append(i)
        i = n
        while i > result:
            while stack and pre[stack[-1]] < pre[i]:
                result = max(result, i - stack.pop())
            i -= 1
        return result
```
