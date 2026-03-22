# 2006 Count Number of Pairs With Absolute Difference K 差的绝对值为 K 的数对数目

__Description:__

Given an integer array `nums` and an integer `k`, return _the number of pairs_ `(i, j)` _where_ `i < j` _such that_ `|nums[i] - nums[j]| == k`.

The value of `|x|` is defined as:

- `x` if `x >= 0`.
- `-x` if `x < 0`.

__Example:__

Example 1:

```text
Input: nums = [1,2,2,1], k = 1
Output: 4
Explanation: The pairs with an absolute difference of 1 are:
```

- [__1__,__2__,2,1]
- [__1__,2,__2__,1]
- [1,__2__,2,__1__]
- [1,2,__2__,__1__]

Example 2:

```text
Input: nums = [1,3], k = 3
Output: 0
Explanation: There are no pairs with an absolute difference of 3.
```

Example 3:

```text
Input: nums = [3,2,1,5,4], k = 2
Output: 3
Explanation: The pairs with an absolute difference of 2 are:
```

- [__3__,2,__1__,5,4]
- [3,__2__,1,5,__4__]
- [__3__,2,1,__5__,4]

__Constraints:__

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`
- `1 <= k <= 99`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回数对 `(i, j)` 的数目，满足 `i < j` 且 `|nums[i] - nums[j]| == k` 。

`|x|` 的值定义为:

- 如果 `x >= 0` ，那么值为 `x` 。
- 如果 `x < 0` ，那么值为 `-x` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,2,1], k = 1
输出：4
解释：差的绝对值为 1 的数对为：
```

- [__1__,__2__,2,1]
- [__1__,2,__2__,1]
- [1,__2__,2,__1__]
- [1,2,__2__,__1__]

示例 2：

```text
输入：nums = [1,3], k = 3
输出：0
解释：没有任何数对差的绝对值为 3 。
```

示例 3：

```text
输入：nums = [3,2,1,5,4], k = 2
输出：3
解释：差的绝对值为 2 的数对为：
```

- [__3__,2,__1__,5,4]
- [3,__2__,1,5,__4__]
- [__3__,2,1,__5__,4]

__提示：__

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`
- `1 <= k <= 99`

__思路:__

```text
1. 暴力法
遍历所有的数对，计算差的绝对值是否等于 k
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 哈希表
记录每个数出现的次数
对于 nums[j]
如果 nums[j] - nums[i] = k 或者 nums[i] - nums[j] = k
即 nums[j] - k = nums[i] 或者 nums[j] + k = nums[i]
结果需要加上 nums[j] - k 或者 nums[j] + k 出现的次数
即 nums[i] 出现的次数
遍历 nums 记录每个数出现的次数并累加
时间复杂度为 O(N), 空间复杂度为 O(N), 实际上只需要用到 101 个空间, 即数组元素的范围
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countKDifference(vector<int>& nums, int k) 
    {
        int result = 0;
        vector<int> counter(101);
        for (const auto& num : nums)
        {
            if (num + k < 101) result += counter[num + k];
            if (num - k > 0) result += counter[num - k];
            ++counter[num];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countKDifference(int[] nums, int k) {
        int result = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.merge(num, 1, Integer::sum);
            result += map.getOrDefault(num - k, 0) + map.getOrDefault(num + k, 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countKDifference(self, nums: List[int], k: int) -> int:
        return sum(abs(nums[i] - nums[j]) == k for i in range(len(nums)) for j in range(i + 1, len(nums)))
```
