# 1785 Minimum Elements to Add to Form a Given Sum 构成特定和需要添加的最少元素

__Description:__

You are given an integer array `nums` and two integers `limit` and `goal`. The array `nums` has an interesting property that `abs(nums[i]) <= limit`.

Return _the minimum number of elements you need to add to make the sum of the array equal to_ `goal`. The array must maintain its property that `abs(nums[i]) <= limit`.

Note that `abs(x)` equals `x` if `x >= 0`, and `-x` otherwise.

__Example:__

Example 1:

```text
Input: nums = [1,-1,1], limit = 3, goal = -4
Output: 2
Explanation: You can add -2 and -3, then the sum of the array will be 1 - 1 + 1 - 2 - 3 = -4.
```

Example 2:

```text
Input: nums = [1,-10,9,1], limit = 100, goal = 0
Output: 1
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= limit <= 10 ^ 6`
- `-limit <= nums[i] <= limit`
- `-10 ^ 9 <= goal <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` ，和两个整数 `limit` 与 `goal` 。数组 `nums` 有一条重要属性: `abs(nums[i]) <= limit` 。

返回使数组元素总和等于 `goal` 所需要向数组中添加的 __最少元素数量__ ，添加元素 __不应改变__ 数组中 `abs(nums[i]) <= limit` 这一属性。

注意，如果 `x >= 0` ，那么 `abs(x)` 等于 `x` ；否则，等于 `-x` 。

__示例:__

示例 1：

```text
输入：nums = [1,-1,1], limit = 3, goal = -4
输出：2
解释：可以将 -2 和 -3 添加到数组中，数组的元素总和变为 1 - 1 + 1 - 2 - 3 = -4 。
```

示例 2：

```text
输入：nums = [1,-10,9,1], limit = 100, goal = 0
输出：1
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= limit <= 10 ^ 6`
- `-limit <= nums[i] <= limit`
- `-10 ^ 9 <= goal <= 10 ^ 9`

__思路:__

```text
数学
求出数组的和
计算和与目标的差值的绝对值
贪心的加入接近 limit 的值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minElements(vector<int>& nums, int limit, int goal) 
    {
        return (abs(goal - accumulate(nums.begin(), nums.end(), 0l)) + limit - 1) / limit;
    }
};
```

__Java__:

```Java
class Solution {
    public int minElements(int[] nums, int limit, int goal) {
        return (int)((Math.abs(goal - IntStream.of(nums).mapToLong(x -> Long.valueOf(x)).sum()) + limit - 1) / limit);
    }
}
```

__Python__:

```Python
class Solution:
    def minElements(self, nums: List[int], limit: int, goal: int) -> int:
        return (abs(goal - sum(nums)) + limit - 1) // limit
```
