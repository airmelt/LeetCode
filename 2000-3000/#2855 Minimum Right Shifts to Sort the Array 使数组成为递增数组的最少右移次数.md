# 2855 Minimum Right Shifts to Sort the Array 使数组成为递增数组的最少右移次数

__Description:__

You are given a __0-indexed__ array `nums` of length `n` containing __distinct__ positive integers. Return _the __minimum__ number of __right shifts__ required to sort_ `nums` _and_ `-1` _if this is not possible._

A __right shift__ is defined as shifting the element at index `i` to index `(i + 1) % n`, for all indices.

__Example:__

Example 1:

```text
Input: nums = [3,4,5,1,2]
Output: 2
Explanation: 
After the first right shift, nums = [2,3,4,5,1].
After the second right shift, nums = [1,2,3,4,5].
Now nums is sorted; therefore the answer is 2.
```

Example 2:

```text
Input: nums = [1,3,5]
Output: 0
Explanation: nums is already sorted therefore, the answer is 0.
```

Example 3:

```text
Input: nums = [2,1,4]
Output: -1
Explanation: It's impossible to sort the array using right shifts.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `nums` contains distinct integers.

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的数组 `nums` ，数组中的元素为 __互不相同__ 的正整数。请你返回让 `nums` 成为递增数组的 __最少右移__ 次数，如果无法得到递增数组，返回 `-1` 。

一次 __右移__ 指的是同时对所有下标进行操作，将下标为 `i` 的元素移动到下标 `(i + 1) % n` 处。

__示例:__

示例 1：

```text
输入：nums = [3,4,5,1,2]
输出：2
解释：
第一次右移后，nums = [2,3,4,5,1] 。
第二次右移后，nums = [1,2,3,4,5] 。
现在 nums 是递增数组了，所以答案为 2 。
```

示例 2：

```text
输入：nums = [1,3,5]
输出：0
解释：nums 已经是递增数组了，所以答案为 0 。
```

示例 3：

```text
输入：nums = [2,1,4]
输出：-1
解释：无法将数组变为递增数组。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `nums` 中的整数互不相同。

__思路:__

```text
1. 暴力法
尝试移动 i 次
检查移动之后是否是递增数组
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 递增性质
如果一个数组右移之后能成为递增数组
那么这个数组最多只能有一个递减段
比如 [1, 3, 5]
5 -> 1 递减
而 [2, 1, 4]
2 -> 1, 4 -> 2 有两个递减段就不能成立
找到第一个递减段那么右移次数可能为第一个递减段段长度
如果找到第二个返回 -1
否则返回第一个递减段长度
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumRightShifts(vector<int>& nums) 
    {
        int n = nums.size(), idx = 0;
        for (int i = 0; i < n; i++) 
        {
            if (nums[i] > nums[(i + 1) % n]) 
            {
                if (idx == 0) idx = n - i - 1;
                else return -1;
            }
        }
        return idx;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumRightShifts(List<Integer> nums) {
        int n = nums.size(), idx = 0;
        for (int i = 0; i < n; i++) {
            if (nums.get(i) > nums.get((i + 1) % n)) {
                if (idx == 0) idx = n - i - 1;
                else return -1;
            }
        }
        return idx;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumRightShifts(self, nums: List[int]) -> int:
        return min([i for i in range(len(nums)) if sorted(nums[len(nums) - i:] + nums[:len(nums) - i]) == nums[len(nums) - i:] + nums[:len(nums) - i]], default=-1)
```
