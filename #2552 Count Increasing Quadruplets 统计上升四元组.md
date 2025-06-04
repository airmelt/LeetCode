# 2552 Count Increasing Quadruplets 统计上升四元组

__Description:__

Given a __0-indexed__ integer array `nums` of size `n` containing all numbers from `1` to `n`, return _the number of increasing quadruplets_.

A quadruplet `(i, j, k, l)` is increasing if:

- `0 <= i < j < k < l < n`, and
- `nums[i] < nums[k] < nums[j] < nums[l]`.

__Example:__

Example 1:

```text
Input: nums = [1,3,2,4,5]
Output: 2
Explanation: 
```

- When i = 0, j = 1, k = 2, and l = 3, nums[i] < nums[k] < nums[j] < nums[l].
- When i = 0, j = 1, k = 2, and l = 4, nums[i] < nums[k] < nums[j] < nums[l].

There are no other quadruplets, so we return 2.

Example 2:

```text
Input: nums = [1,2,3,4]
Output: 0
Explanation: There exists only one quadruplet with i = 0, j = 1, k = 2, l = 3, but since nums[j] < nums[k], we return 0.
```

__Constraints:__

- `4 <= nums.length <= 4000`
- `1 <= nums[i] <= nums.length`
- All the integers of `nums` are __unique__. `nums` is a permutation.

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `nums` ，它包含 `1` 到 `n` 的所有数字，请你返回上升四元组的数目。

如果一个四元组 `(i, j, k, l)` 满足以下条件，我们称它是上升的:

- `0 <= i < j < k < l < n` 且
- `nums[i] < nums[k] < nums[j] < nums[l]` 。

__示例:__

示例 1：

```text
输入：nums = [1,3,2,4,5]
输出：2
解释：
```

- 当 i = 0 ，j = 1 ，k = 2 且 l = 3 时，有 nums[i] < nums[k] < nums[j] < nums[l] 。
- 当 i = 0 ，j = 1 ，k = 2 且 l = 4 时，有 nums[i] < nums[k] < nums[j] < nums[l] 。

没有其他的四元组，所以我们返回 2 。

示例 2：

```text
输入：nums = [1,2,3,4]
输出：0
解释：只存在一个四元组 i = 0 ，j = 1 ，k = 2 ，l = 3 ，但是 nums[j] < nums[k] ，所以我们返回 0 。
```

__提示：__

- `4 <= nums.length <= 4000`
- `1 <= nums[i] <= nums.length`
- `nums` 中所有数字 __互不相同__ ， `nums` 是一个排列。

__思路:__

```text
枚举
对于本题可以看作是 1324 模式
即从小到大枚举 (i, j, k, l) -> (1, 2, 3, 4)
大小关系为 1324 模式
枚举右端点 l
则 l 应该是最大的数
此时需要找到一个比 l 小的 j, 即 3 代表的数
并统计比 3 小的 12 的个数
注意到数组是 1 - n 的一个排列
用一个数组 count 记录 12 的个数
再用一个变量 cur 记录当前比 j 小的个数, 这个即为 12 的个数
对于每个 l, 从左到右遍历 j
如果 nums[j] < nums[l], 则说明 j 是一个合法的 3
此时将结果 result 加上 count[j]，即当前的 132 的个数, 此时 j 和 l 也能构成一个合法的 12 模式, cur 加 1
否则 nums[j] > nums[l], 说明 j 和 l 能构成一个合法的 12 模式, 将 count[j] 加上 cur, 即当前的 12 的个数, 记录下了以 j 为 3 的 12 的个数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countQuadruplets(vector<int>& nums) 
    {
        long long result = 0LL;
        vector<int> cnt(nums.size());
        for (int i = 2, cur = 0, n = nums.size(); i < n; i++, cur = 0) 
        {
            for (int j = 0; j < i; j++) 
            {
                if (nums[j] < nums[i]) 
                {
                    result += cnt[j];
                    ++cur;
                } 
                else cnt[j] += cur;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countQuadruplets(int[] nums) {
        long result = 0L;
        int n = nums.length, count[] = new int[n];
        for (int i = 2, cur = 0; i < n; i++, cur = 0) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    result += count[j];
                    ++cur;
                } else count[j] += cur;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        result, count = 0, [0] * (n := len(nums))
        for i in range(2, n):
            cur = 0
            for j in range(i):
                if nums[j] < nums[i]:
                    result += count[j]
                    cur += 1
                else:
                    count[j] += cur
        return result
```
