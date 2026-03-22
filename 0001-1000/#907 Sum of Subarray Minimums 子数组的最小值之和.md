# 907 Sum of Subarray Minimums 子数组的最小值之和

__Description__:
Given an array of integers arr, find the sum of min(b), where b ranges over every (contiguous) subarray of arr. Since the answer may be large, return the answer modulo 10^9 + 7.

__Example:__

Example 1:

Input: arr = [3,1,2,4]
Output: 17
Explanation:
Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4].
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

Example 2:

Input: arr = [11,81,94,43,3]
Output: 444

__Constraints:__

1 <= arr.length <= 3 \* 10^4
1 <= arr[i] <= 3 \* 10^4

__题目描述__:
给定一个整数数组 arr，找到 min(b) 的总和，其中 b 的范围为 arr 的每个（连续）子数组。

由于答案可能很大，因此 返回答案模 10^9 + 7 。

__示例 :__

示例 1：

输入：arr = [3,1,2,4]
输出：17
解释：
子数组为 [3]，[1]，[2]，[4]，[3,1]，[1,2]，[2,4]，[3,1,2]，[1,2,4]，[3,1,2,4]。
最小值为 3，1，2，4，1，1，2，1，1，1，和为 17。

示例 2：

输入：arr = [11,81,94,43,3]
输出：444

__提示:__

1 <= arr.length <= 3 \* 10^4
1 <= arr[i] <= 3 \* 10^4

__思路__:

单调栈
核心是计算出每个最小值对应的子数组的长度及个数
result = ∑arr[i] \* (i - left_index) \* (right_index - i), left_index, right_index 分别为 arr[i] 作为最小值数组左右两边的下标
单调栈中记录依次递增的值的下标
每次栈顶对应元素不小于当前元素时, 弹出栈顶
这时候, 栈顶对应元素对子数组的最小值的贡献可以通过 (arr[i] - arr[top]) * (top - stack.top()(default = -1)) 得到
用一个 pre 记录当前的和 pre 需要加上贡献及当前元素
result 每次加上 pre 就能得到子数组的最小值之和
时间复杂度为 O(n), 空间复杂度为 O(n), 每个元素只需要访问最多 2 次

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int sumSubarrayMins(vector<int>& arr) 
    {
        long MOD = 1e9 + 7, pre = 0, result = 0;
        stack<int> s;
        for (int i = 0, n = arr.size(); i < n; i++)
        {
            while (!s.empty() and arr[s.top()] >= arr[i])
            {
                int top = s.top();
                s.pop();
                pre += (arr[i] - arr[top]) * (top - (s.empty() ? -1 : s.top()));
            }
            pre += arr[i];
            s.push(i);
            result += pre;
        }
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumSubarrayMins(int[] arr) {
        long MOD = 1_000_000_007, pre = 0, result = 0;
        Stack<Integer> stack = new Stack<>();
        for (int i = 0, n = arr.length; i < n; i++) {
            while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) {
                int top = stack.pop();
                pre += (arr[i] - arr[top]) * (top - (stack.isEmpty() ? -1 : stack.peek()));
            }
            pre += arr[i];
            stack.push(i);
            result += pre;
        }
        return (int)(result % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def sumSubarrayMins(self, arr: List[int]) -> int:
        stack, mod, pre, result, n = [], 10 ** 9 + 7, 0, 0, len(arr)
        for i in range(n):
            while stack and arr[stack[-1]] >= arr[i]:
                pre += (arr[i] - arr[(top := stack.pop(-1))]) * (top - (stack[-1] if stack else -1))
            pre += arr[i]
            stack.append(i)
            result += pre
        return result % mod
```
