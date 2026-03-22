# 2057 Smallest Index With Equal Value 值相等的最小索引

__Description:__

Given a __0-indexed__ integer array `nums`, return _the __smallest__ index_ `i` _of_ `nums` _such that_ `i mod 10 == nums[i]`_, or_ `-1` _if such index does not exist_.

`x mod y` denotes the __remainder__ when `x` is divided by `y`.

__Example:__

Example 1:

```text
Input: nums = [0,1,2]
Output: 0
Explanation: 
i=0: 0 mod 10 = 0 == nums[0].
i=1: 1 mod 10 = 1 == nums[1].
i=2: 2 mod 10 = 2 == nums[2].
All indices have i mod 10 == nums[i], so we return the smallest index 0.
```

Example 2:

```text
Input: nums = [4,3,2,1]
Output: 2
Explanation: 
i=0: 0 mod 10 = 0 != nums[0].
i=1: 1 mod 10 = 1 != nums[1].
i=2: 2 mod 10 = 2 == nums[2].
i=3: 3 mod 10 = 3 != nums[3].
2 is the only index which has i mod 10 == nums[i].
```

Example 3:

```text
Input: nums = [1,2,3,4,5,6,7,8,9,0]
Output: -1
Explanation: No index satisfies i mod 10 == nums[i].
```

__Constraints:__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 9`

__题目描述:__

给你一个下标从 0 开始的整数数组 `nums` ，返回 `nums` 中满足 `i mod 10 == nums[i]` 的最小下标 `i` ；如果不存在这样的下标，返回 `-1` 。

`x mod y` 表示 `x` 除以 `y` 的 __余数__ 。

__示例:__

示例 1：

```text
输入：nums = [0,1,2]
输出：0
解释：
i=0: 0 mod 10 = 0 == nums[0].
i=1: 1 mod 10 = 1 == nums[1].
i=2: 2 mod 10 = 2 == nums[2].
所有下标都满足 i mod 10 == nums[i] ，所以返回最小下标 0
```

示例 2：

```text
输入：nums = [4,3,2,1]
输出：2
解释：
i=0: 0 mod 10 = 0 != nums[0].
i=1: 1 mod 10 = 1 != nums[1].
i=2: 2 mod 10 = 2 == nums[2].
i=3: 3 mod 10 = 3 != nums[3].
2 唯一一个满足 i mod 10 == nums[i] 的下标
```

示例 3：

```text
输入：nums = [1,2,3,4,5,6,7,8,9,0]
输出：-1
解释：不存在满足 i mod 10 == nums[i] 的下标
```

示例 4：

```text
输入：nums = [2,1,3,5,2]
输出：1
解释：1 是唯一一个满足 i mod 10 == nums[i] 的下标
```

__提示：__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 9`

__思路:__

```text
模拟
如果找到 i mod 10 == nums[i] 的下标 i, 则返回 i
否则返回 -1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int smallestEqual(vector<int>& nums) 
    {
        for (int i = 0, n = nums.size(); i < n; i++) if (i % 10 == nums[i]) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestEqual(int[] nums) {
        for (int i = 0, n = nums.length; i < n; i++) if (i % 10 == nums[i]) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestEqual(self, nums: List[int]) -> int:
        return min([i for i in range(len(nums)) if i % 10 == nums[i]], default=-1)
```
