# 2772 Apply Operations to Make All Array Elements Equal to Zero 使数组中的所有元素都等于零

__Description:__

You are given a __0-indexed__ integer array `nums` and a positive integer `k`.

You can apply the following operation on the array any number of times:

- Choose __any__ subarray of size `k` from the array and __decrease__ all its elements by `1`.

Return `true` _if you can make all the array elements equal to_ `0`_, or_ `false` _otherwise_.

A subarray is a contiguous non-empty part of an array.

__Example:__

Example 1:

```text
Input: nums = [2,2,3,1,1,0], k = 3
Output: true
Explanation: We can do the following operations:
```

- Choose the subarray [2,2,3]. The resulting array will be nums = [1,1,2,1,1,0].
- Choose the subarray [2,1,1]. The resulting array will be nums = [1,1,1,0,0,0].
- Choose the subarray [1,1,1]. The resulting array will be nums = [0,0,0,0,0,0].

Example 2:

```text
Input: nums = [1,3,1,1], k = 2
Output: false
Explanation: It is not possible to make all the array elements equal to 0.
```

__Constraints:__

- `1 <= k <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个正整数 `k` 。

你可以对数组执行下述操作 任意次 ：

- 从数组中选出长度为 `k` 的 __任一__ 子数组，并将子数组中每个元素都 __减去__ `1` 。

如果你可以使数组中的所有元素都等于 `0` ，返回  `true` ；否则，返回 `false` 。

子数组 是数组中的一个非空连续元素序列。

__示例:__

示例 1：

```text
输入：nums = [2,2,3,1,1,0], k = 3
输出：true
解释：可以执行下述操作：
```

- 选出子数组 [2,2,3] ，执行操作后，数组变为 nums = [1,1,2,1,1,0] 。
- 选出子数组 [2,1,1] ，执行操作后，数组变为 nums = [1,1,1,0,0,0] 。
- 选出子数组 [1,1,1] ，执行操作后，数组变为 nums = [0,0,0,0,0,0] 。

示例 2：

```text
输入：nums = [1,3,1,1], k = 2
输出：false
解释：无法使数组中的所有元素等于 0 。
```

__提示：__

- `1 <= k <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__思路:__

```text
差分数组
对于 nums[0] 如果不等于 0, 那么 nums[:k] 都必须减少 nums[0]
在此基础上 nums[1] 还需要减少 nums[1] - nums[0] 才能为 0
如果 i + k 已经超过 n, 那么无论如何也不能把后面的元素减少到 0
或者如果前面的数比后面的还小也是不能减到 0 的
用差分数组和待减少的值
遍历数组, 待减少的值先加上此前的差分
nums[i] 加上这个待减少的值, 即完成一次减去的操作
如果 nums[i] 已经为 0, 那么后续不用进行操作了
如果 nums[i] < 0 由于不能加操作, 直接返回 false
如果 nums[i] > 0, 待减少的值减去 nums[i], 尝试用后面的值去减
diff[i + k] 再加上 nums[i], 表示 [i:i + k] 都需要减去 nums[i]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkArray(vector<int>& nums, int k) 
    {
        int n = nums.size(), s = 0;
        vector<int> diff(n + 1);
        for (int i = 0; i < n; i++) 
        {
            s += diff[i];
            if (nums[i] += s) 
            {
                if (nums[i] < 0 or i + k > n) return false;
                s -= nums[i];
                diff[i + k] += nums[i];
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkArray(int[] nums, int k) {
        int n = nums.length, diff[] = new int[n + 1], s = 0;
        for (int i = 0; i < n; i++) {
            s += diff[i];
            if ((nums[i] += s) != 0) {
                if (nums[i] < 0 || i + k > n) return false;
                s -= nums[i];
                diff[i + k] += nums[i];
            }
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def checkArray(self, nums: List[int], k: int) -> bool:
        diff, s = [0] * ((n := len(nums)) + 1), 0
        for i, num in enumerate(nums):
            s += diff[i]
            if (num := num + s):
                if num < 0 or i + k > n:
                    return False
                s -= num
                diff[i + k] += num
        return True
```
