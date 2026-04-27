# 2905 Find Indices With Index and Value Difference II 找出满足差值条件的下标 II

__Description:__

You are given a __0-indexed__ integer array `nums` having length `n`, an integer `indexDifference`, and an integer `valueDifference`.

Your task is to find __two__ indices `i` and `j`, both in the range `[0, n - 1]`, that satisfy the following conditions:

- `abs(i - j) >= indexDifference`, and
- `abs(nums[i] - nums[j]) >= valueDifference`

Return _an integer array_ `answer`, _where_ `answer = [i, j]` _if there are two such indices_, _and_ `answer = [-1, -1]` _otherwise_. If there are multiple choices for the two indices, return _any of them_.

__Note:__ `i` and `j` may be __equal__.

__Example:__

Example 1:

```text
Input: nums = [5,1,4,1], indexDifference = 2, valueDifference = 4
Output: [0,3]
Explanation: In this example, i = 0 and j = 3 can be selected.
abs(0 - 3) >= 2 and abs(nums[0] - nums[3]) >= 4.
Hence, a valid answer is [0,3].
[3,0] is also a valid answer.
```

Example 2:

```text
Input: nums = [2,1], indexDifference = 0, valueDifference = 0
Output: [0,0]
Explanation: In this example, i = 0 and j = 0 can be selected.
abs(0 - 0) >= 0 and abs(nums[0] - nums[0]) >= 0.
Hence, a valid answer is [0,0].
Other valid answers are [0,1], [1,0], and [1,1].
```

Example 3:

```text
Input: nums = [1,2,3], indexDifference = 2, valueDifference = 4
Output: [-1,-1]
Explanation: In this example, it can be shown that it is impossible to find two indices that satisfy both conditions.
Hence, [-1,-1] is returned.
```

__Constraints:__

- `1 <= n == nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= indexDifference <= 10 ^ 5`
- `0 <= valueDifference <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的整数数组 `nums` ，以及整数 `indexDifference` 和整数 `valueDifference` 。

你的任务是从范围 `[0, n - 1]` 内找出 __2__ 个满足下述所有条件的下标 `i` 和 `j` :

- `abs(i - j) >= indexDifference` 且
- `abs(nums[i] - nums[j]) >= valueDifference`

返回整数数组 `answer`。如果存在满足题目要求的两个下标，则 `answer = [i, j]` ；否则， `answer = [-1, -1]` 。如果存在多组可供选择的下标对，只需要返回其中任意一组即可。

__注意:__`i` 和 `j` 可能 __相等__ 。

__示例:__

示例 1：

```text
输入：nums = [5,1,4,1], indexDifference = 2, valueDifference = 4
输出：[0,3]
解释：在示例中，可以选择 i = 0 和 j = 3 。
abs(0 - 3) >= 2 且 abs(nums[0] - nums[3]) >= 4 。
因此，[0,3] 是一个符合题目要求的答案。
[3,0] 也是符合题目要求的答案。
```

示例 2：

```text
输入：nums = [2,1], indexDifference = 0, valueDifference = 0
输出：[0,0]
解释：
在示例中，可以选择 i = 0 和 j = 0 。 
abs(0 - 0) >= 0 且 abs(nums[0] - nums[0]) >= 0 。 
因此，[0,0] 是一个符合题目要求的答案。 
[0,1]、[1,0] 和 [1,1] 也是符合题目要求的答案。
```

示例 3：

```text
输入：nums = [1,2,3], indexDifference = 2, valueDifference = 4
输出：[-1,-1]
解释：在示例中，可以证明无法找出 2 个满足所有条件的下标。
因此，返回 [-1,-1] 。
```

__提示：__

- `1 <= n == nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= indexDifference <= 10 ^ 5`
- `0 <= valueDifference <= 10 ^ 9`

__思路:__

```text
遍历
因为最大值和最小值最容易满足 abs(nums[i] - nums[j]) >= valueDifference
从 indexDifference 开始
记录 [indexDifference - j, indexDifference] 的最大值 mx 和最小值 mn
如果 abs(nums[mx or mn] - nums[j]) >= valueDifference
立即返回 mx/mn 对应的下标以及 j
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findIndices(vector<int>& nums, int indexDifference, int valueDifference) 
    {
        int mx = 0, mn = 0, n = nums.size();
        for (int j = indexDifference, i = 0; j < n; j++) 
        {
            if (nums[i = j - indexDifference] > nums[mx]) mx = i;
            else if (nums[i] < nums[mn]) mn = i;
            if (nums[mx] - nums[j] >= valueDifference) return { mx, j };
            if (nums[j] - nums[mn] >= valueDifference) return { mn, j };
        }
        return { -1, -1 };
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findIndices(int[] nums, int indexDifference, int valueDifference) {
        int mx = 0, mn = 0, n = nums.length;
        for (int j = indexDifference, i = 0; j < n; j++) {
            if (nums[i = j - indexDifference] > nums[mx]) mx = i;
            else if (nums[i] < nums[mn]) mn = i;
            if (nums[mx] - nums[j] >= valueDifference) return new int[]{ mx, j };
            if (nums[j] - nums[mn] >= valueDifference) return new int[]{ mn, j };
        }
        return new int[]{ -1, -1 };
    }
}
```

__Python__:

```Python
class Solution:
    def findIndices(self, nums: List[int], indexDifference: int, valueDifference: int) -> List[int]:
        mx, mn, n = 0, 0, len(nums)
        for j in range(indexDifference, n):
            if nums[mx := i if nums[i := j - indexDifference] > nums[mx] else mx] - nums[j] >= valueDifference:
                return [mx, j]
            if nums[j] - nums[mn := i if nums[i] < nums[mn] else mn] >= valueDifference:
                return [mn, j]
        return [-1, -1]
```
