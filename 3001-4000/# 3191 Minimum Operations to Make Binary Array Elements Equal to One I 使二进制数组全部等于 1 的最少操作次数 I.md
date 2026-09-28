# 3191 Minimum Operations to Make Binary Array Elements Equal to One I 使二进制数组全部等于 1 的最少操作次数 I

__Description:__

You are given a __binary array__ `nums`.

You can do the following operation on the array __any__ number of times (possibly zero):

- Choose __any__ 3 __consecutive__ elements from the array and __flip all__ of them.

__Flipping__ an element means changing its value from 0 to 1, and from 1 to 0.

Return the __minimum__ number of operations required to make all elements in `nums` equal to 1. If it is impossible, return -1.

__Example:__

Example 1:

```text
Input: nums = [0,1,1,1,0,0]

Output: 3

Explanation:
We can do the following operations:

Choose the elements at indices 0, 1 and 2. The resulting array is nums = [1,0,0,1,0,0].
Choose the elements at indices 1, 2 and 3. The resulting array is nums = [1,1,1,0,0,0].
Choose the elements at indices 3, 4 and 5. The resulting array is nums = [1,1,1,1,1,1].
```

Example 2:

```text
Input: nums = [0,1,1,1]

Output: -1

Explanation:
It is impossible to make all elements equal to 1.
```

__Constraints:__

- `3 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 1`

__题目描述:__

给你一个二进制数组 `nums` 。

你可以对数组执行以下操作 __任意__ 次（也可以 0 次）：

- 选择数组中 __任意连续__ 3 个元素，并将它们 __全部反转__ 。

__反转__ 一个元素指的是将它的值从 0 变 1 ，或者从 1 变 0 。

请你返回将 `nums` 中所有元素变为 1 的 __最少__ 操作次数。如果无法全部变成 1 ，返回 -1 。

__示例:__

示例 1：

```text
输入：nums = [0,1,1,1,0,0]

输出：3

解释：
我们可以执行以下操作：

选择下标为 0 ，1 和 2 的元素并反转，得到 nums = [1,0,0,1,0,0] 。
选择下标为 1 ，2 和 3 的元素并反转，得到 nums = [1,1,1,0,0,0] 。
选择下标为 3 ，4 和 5 的元素并反转，得到 nums = [1,1,1,1,1,1] 。
```

示例 2：

```text
输入：nums = [0,1,1,1]

输出：-1

解释：
无法将所有元素都变为 1 。
```

__提示：__

- `3 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 1`

__思路:__

```text
贪心
从左到右依次操作不为 0 的元素
如果第 i 位不为 0
将 i + 1 和 i + 2 都反转
最后 n - 1 和 n - 2 不能操作
如果这两位为 0
说明无法操作
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        for (int i = 0; i < n - 2; i++) 
        {
            if (!nums[i]) 
            {
                nums[i + 1] ^= 1;
                nums[i + 2] ^= 1;
                ++result;
            }
        }
        return nums[n - 1] == nums[n - 2] and nums.back() ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        int result = 0, n = nums.length;
        for (int i = 0; i < n - 2; i++) {
            if (nums[i] == 0) {
                nums[i + 1] ^= 1;
                nums[i + 2] ^= 1;
                ++result;
            }
        }
        return nums[n - 1] == nums[n - 2] && nums[n - 1] == 1 ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        result, n = 0, len(nums)
        for i in range(n - 2):
            if not nums[i]:
                nums[i + 1] ^= 1
                nums[i + 2] ^= 1
                result += 1
        return result if nums[-2] and nums[-1] else -1
```
