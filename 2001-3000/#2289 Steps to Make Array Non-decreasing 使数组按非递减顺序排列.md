# 2289 Steps to Make Array Non-decreasing 使数组按非递减顺序排列

__Description:__

You are given a __0-indexed__ integer array `nums`. In one step, __remove__ all elements `nums[i]` where `nums[i - 1] > nums[i]` for all `0 < i < nums.length`.

Return _the number of steps performed until_ `nums` _becomes a __non-decreasing__ array_.

__Example:__

Example 1:

```text
Input: nums = [5,3,4,4,7,3,6,11,8,5,11]
Output: 3
Explanation: The following are the steps performed:
```

- Step 1: [5,3,4,4,7,3,6,11,8,5,11] becomes [5,4,4,7,6,11,11]
- Step 2: [5,4,4,7,6,11,11] becomes [5,4,7,11,11]
- Step 3: [5,4,7,11,11] becomes [5,7,11,11]

[5,7,11,11] is a non-decreasing array. Therefore, we return 3.

Example 2:

```text
Input: nums = [4,5,7,7,13]
Output: 0
Explanation: nums is already a non-decreasing array. Therefore, we return 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。在一步操作中，移除所有满足 `nums[i - 1] > nums[i]` 的 `nums[i]` ，其中 `0 < i < nums.length` 。

重复执行步骤，直到 `nums` 变为 __非递减__ 数组，返回所需执行的操作数。

__示例:__

示例 1：

```text
输入：nums = [5,3,4,4,7,3,6,11,8,5,11]
输出：3
解释：执行下述几个步骤：
```

- 步骤 1 ：[5,3,4,4,7,3,6,11,8,5,11] 变为 [5,4,4,7,6,11,11]
- 步骤 2 ：[5,4,4,7,6,11,11] 变为 [5,4,7,11,11]
- 步骤 3 ：[5,4,7,11,11] 变为 [5,7,11,11]

[5,7,11,11] 是一个非递减数组，因此，返回 3 。

示例 2：

```text
输入：nums = [4,5,7,7,13]
输出：0
解释：nums 已经是一个非递减数组，因此，返回 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
单调栈
注意到这题只需要返回操作数, 因此可以不用真正的去修改数组
维护一个单调递增栈, 用于存储每个元素的值和其对应的步数
维护每一段递增的序列, 最后一个元素的删除时间就是这段递增序列的步数
所以如果当前元素不比栈顶元素小, 则需要将栈顶元素出栈, 并更新当前元素的最大步数
否则将当前元素和最大步数加 1 入栈
如果栈为空, 则当前元素的步数为 0, 表示该元素不会被删除
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int totalSteps(vector<int>& nums) 
    {
        stack<pair<int, int>> st;
        int result = 0;
        for (const auto& num : nums) 
        {
            int cur = 0;
            while (!st.empty() and st.top().first <= num) 
            {
                cur = max(cur, st.top().second);
                st.pop();
            }
            result = max(result, cur += !st.empty());
            st.push({num, cur});
        } 
        return result;
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    int totalSteps(vector<int>& nums) 
    {
        stack<pair<int, int>> st;
        int result = 0;
        for (const auto& num : nums) 
        {
            int cur = 0;
            while (!st.empty() and st.top().first <= num) 
            {
                cur = max(cur, st.top().second);
                st.pop();
            }
            result = max(result, cur += !st.empty());
            st.push({num, cur});
        } 
        return result;
    }
};
```

__Python__:

```Python
class Solution 
{
public:
    int totalSteps(vector<int>& nums) 
    {
        stack<pair<int, int>> st;
        int result = 0;
        for (const auto& num : nums) 
        {
            int cur = 0;
            while (!st.empty() and st.top().first <= num) 
            {
                cur = max(cur, st.top().second);
                st.pop();
            }
            result = max(result, cur += !st.empty());
            st.push({num, cur});
        } 
        return result;
    }
};
```
