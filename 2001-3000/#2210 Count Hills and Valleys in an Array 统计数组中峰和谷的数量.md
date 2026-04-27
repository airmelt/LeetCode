# 2210 Count Hills and Valleys in an Array 统计数组中峰和谷的数量

__Description:__

You are given a __0-indexed__ integer array `nums`. An index `i` is part of a __hill__ in `nums` if the closest non-equal neighbors of `i` are smaller than `nums[i]`. Similarly, an index `i` is part of a __valley__ in `nums` if the closest non-equal neighbors of `i` are larger than `nums[i]`. Adjacent indices `i` and `j` are part of the __same__ hill or valley if `nums[i] == nums[j]`.

Note that for an index to be part of a hill or valley, it must have a non-equal neighbor on both the left and right of the index.

Return _the number of hills and valleys in_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [2,4,1,1,6,5]
Output: 3
Explanation:
At index 0: There is no non-equal neighbor of 2 on the left, so index 0 is neither a hill nor a valley.
At index 1: The closest non-equal neighbors of 4 are 2 and 1. Since 4 > 2 and 4 > 1, index 1 is a hill. 
At index 2: The closest non-equal neighbors of 1 are 4 and 6. Since 1 < 4 and 1 < 6, index 2 is a valley.
At index 3: The closest non-equal neighbors of 1 are 4 and 6. Since 1 < 4 and 1 < 6, index 3 is a valley, but note that it is part of the same valley as index 2.
At index 4: The closest non-equal neighbors of 6 are 1 and 5. Since 6 > 1 and 6 > 5, index 4 is a hill.
At index 5: There is no non-equal neighbor of 5 on the right, so index 5 is neither a hill nor a valley. 
There are 3 hills and valleys so we return 3.
```

Example 2:

```text
Input: nums = [6,6,5,5,4,1]
Output: 0
Explanation:
At index 0: There is no non-equal neighbor of 6 on the left, so index 0 is neither a hill nor a valley.
At index 1: There is no non-equal neighbor of 6 on the left, so index 1 is neither a hill nor a valley.
At index 2: The closest non-equal neighbors of 5 are 6 and 4. Since 5 < 6 and 5 > 4, index 2 is neither a hill nor a valley.
At index 3: The closest non-equal neighbors of 5 are 6 and 4. Since 5 < 6 and 5 > 4, index 3 is neither a hill nor a valley.
At index 4: The closest non-equal neighbors of 4 are 5 and 1. Since 4 < 5 and 4 > 1, index 4 is neither a hill nor a valley.
At index 5: There is no non-equal neighbor of 1 on the right, so index 5 is neither a hill nor a valley.
There are 0 hills and valleys so we return 0.
```

__Constraints:__

- `3 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。如果两侧距 `i` 最近的不相等邻居的值均小于 `nums[i]` ，则下标 `i` 是 `nums` 中，某个峰的一部分。类似地，如果两侧距 `i` 最近的不相等邻居的值均大于 `nums[i]` ，则下标 `i` 是 `nums` 中某个谷的一部分。对于相邻下标 `i` 和 `j` ，如果 `nums[i] == nums[j]` ， 则认为这两下标属于 __同一个__ 峰或谷。

注意，要使某个下标所做峰或谷的一部分，那么它左右两侧必须 都 存在不相等邻居。

返回 `nums` 中峰和谷的数量。

__示例:__

示例 1：

```text
输入：nums = [2,4,1,1,6,5]
输出：3
解释：
在下标 0 ：由于 2 的左侧不存在不相等邻居，所以下标 0 既不是峰也不是谷。
在下标 1 ：4 的最近不相等邻居是 2 和 1 。由于 4 > 2 且 4 > 1 ，下标 1 是一个峰。
在下标 2 ：1 的最近不相等邻居是 4 和 6 。由于 1 < 4 且 1 < 6 ，下标 2 是一个谷。
在下标 3 ：1 的最近不相等邻居是 4 和 6 。由于 1 < 4 且 1 < 6 ，下标 3 符合谷的定义，但需要注意它和下标 2 是同一个谷的一部分。
在下标 4 ：6 的最近不相等邻居是 1 和 5 。由于 6 > 1 且 6 > 5 ，下标 4 是一个峰。
在下标 5 ：由于 5 的右侧不存在不相等邻居，所以下标 5 既不是峰也不是谷。
共有 3 个峰和谷，所以返回 3 。
```

示例 2：

```text
输入：nums = [6,6,5,5,4,1]
输出：0
解释：
在下标 0 ：由于 6 的左侧不存在不相等邻居，所以下标 0 既不是峰也不是谷。
在下标 1 ：由于 6 的左侧不存在不相等邻居，所以下标 1 既不是峰也不是谷。
在下标 2 ：5 的最近不相等邻居是 6 和 4 。由于 5 < 6 且 5 > 4 ，下标 2 既不是峰也不是谷。
在下标 3 ：5 的最近不相等邻居是 6 和 4 。由于 5 < 6 且 5 > 4 ，下标 3 既不是峰也不是谷。
在下标 4 ：4 的最近不相等邻居是 5 和 1 。由于 4 < 5 且 4 > 1 ，下标 4 既不是峰也不是谷。
在下标 5 ：由于 1 的右侧不存在不相等邻居，所以下标 5 既不是峰也不是谷。
共有 0 个峰和谷，所以返回 0 。
```

__提示：__

- `3 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
模拟
数组开头和结尾的元素不可能为峰或谷
遍历数组, 找到左右两侧不相等的元素
如果左侧元素小于当前元素, 右侧元素也小于当前元素, 则当前元素为峰
如果左侧元素大于当前元素, 右侧元素也大于当前元素, 则当前元素为谷
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countHillValley(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        for (int i = 1, left = i - 1, right = i + 1; i < n - 1; i++, left = i - 1, right = i + 1) 
        {
            if (nums[i] != nums[i - 1]) 
            {
                while (left > -1 and nums[left] == nums[i]) --left;
                while (right < n - 1 and nums[right] == nums[i]) ++right;
                result += (nums[left] < nums[i] and nums[i] > nums[right]) + (nums[left] > nums[i] and nums[i] < nums[right]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countHillValley(int[] nums) {
        int result = 0, n = nums.length;
        for (int i = 1, left = i - 1, right = i + 1; i < n - 1; i++, left = i - 1, right = i + 1) {
            if (nums[i] != nums[i - 1]) {
                while (left > -1 && nums[left] == nums[i]) --left;
                while (right < n - 1 && nums[right] == nums[i]) ++right;
                result += (nums[left] < nums[i] && nums[i] > nums[right] ? 1 : 0) + (nums[left] > nums[i] && nums[i] < nums[right] ? 1 : 0);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countHillValley(self, nums: List[int]) -> int:
        result, n = 0, len(nums)
        for i in range(1, n - 1):
            if nums[i] != nums[i - 1]:
                left, right = i - 1, i + 1
                while left > -1 and nums[left] == nums[i]:
                    left -= 1
                while right < n - 1 and nums[right] == nums[i]:
                    right += 1
                result += (nums[left] < nums[i] > nums[right]) + (nums[left] > nums[i] < nums[right])
        return result
```
