# 739 Daily Temperatures 每日温度

__Description__:
Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead.

__Example:__

Example 1:

Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]

Example 2:

Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]

Example 3:

Input: temperatures = [30,60,90]
Output: [1,1,0]

__Constraints:__

1 <= temperatures.length <= 10^5
30 <= temperatures[i] <= 100

__题目描述__:
请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

__示例 :__

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

__提示:__
气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。

__思路__:

单调栈
遍历的时候维护一个单调递减的元素下标的单调栈
每次取到较大元素就弹出栈中元素并记录距离
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) 
    {
        stack<int> s;
        int n = temperatures.size();
        vector<int> result(n);
        for (int i = 0; i < n; i++) 
        {
            while (!s.empty() and temperatures[s.top()] < temperatures[i]) 
            {
                result[s.top()] = i - s.top();
                s.pop();
            }
            s.push(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack = new Stack<>();
        int n = temperatures.length;
        int result[] = new int[n];
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) result[stack.peek()] = i - stack.pop();
            stack.push(i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stack, result = [], [0] * (n := len(temperatures))
        for i, t in enumerate(temperatures):     
            while stack and temperatures[stack[-1]] < t:
                result[stack.pop()] = i - stack[-1]
            stack.append(i)
        return result
```
