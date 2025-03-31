# 2488 Count Subarrays With Median K 统计中位数为 K 的子数组

__Description:__

You are given an array `nums` of size `n` consisting of __distinct__ integers from `1` to `n` and a positive integer `k`.

Return _the number of non-empty subarrays in_ `nums` _that have a __median__ equal to_ `k`.

Note:

- The median of an array is the __middle__ element after sorting the array in __ascending__ order. If the array is of even length, the median is the __left__ middle element.
  - For example, the median of `[2,3,1,4]` is `2`, and the median of `[8,4,3,5,1]` is `4`.
- A subarray is a contiguous part of an array.

__Example:__

Example 1:

```text
Input: nums = [3,2,1,4,5], k = 4
Output: 3
Explanation: The subarrays that have a median equal to 4 are: [4], [4,5] and [1,4,5].
```

Example 2:

```text
Input: nums = [2,3,1], k = 3
Output: 1
Explanation: [3] is the only subarray that has a median equal to 3.
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i], k <= n`
- The integers in `nums` are distinct.

__题目描述:__

给你一个长度为 `n` 的数组 `nums` ，该数组由从 `1` 到 `n` 的 __不同__ 整数组成。另给你一个正整数 `k` 。

统计并返回 `nums` 中的 __中位数__ 等于 `k` 的非空子数组的数目。

注意：

- 数组的中位数是按 __递增__ 顺序排列后位于 __中间__ 的那个元素，如果数组长度为偶数，则中位数是位于中间靠 __左__ 的那个元素。
  - 例如， `[2,3,1,4]` 的中位数是 `2` ， `[8,4,3,5,1]` 的中位数是 `4` 。
- 子数组是数组中的一个连续部分。

__示例:__

示例 1：

```text
输入：nums = [3,2,1,4,5], k = 4
输出：3
解释：中位数等于 4 的子数组有：[4]、[4,5] 和 [1,4,5] 。
```

示例 2：

```text
输入：nums = [2,3,1], k = 3
输出：1
解释：[3] 是唯一一个中位数等于 3 的子数组。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i], k <= n`
- `nums` 中的整数互不相同

__思路:__

```text
前缀和
数组中的数刚好是 1 到 n 的排列
设比 k 小的数为 -1, 等于 k 的数为 0, 大于 k 的数为 1
[3, 2, 1, 4, 5] -> [-1, -1, -1, 0, 1]
前缀和为 [-1, -2, -3, -3, -2]
到达 k 之前如果要使得中位数为 k, 累计前缀和的个数
当到达 k 时, 更新到达 k 的标志
到达 k 之后, 不再累计前缀和的个数
到达 k 或之后累计前缀和 s 和 s - 1 的个数
因为需要求的是前缀和中包含 k 且区间和为 0 或 1 的个数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSubarrays(vector<int>& nums, int k) 
    {
        unordered_map<int, int> m;
        m[0] = 1;
        int s = 0, result = 0, start = 0;
        for (const auto& x : nums)
        {
            s += (x > k ? 1 : (x == k ? 0 : -1));
            if (start or x == k)
            {
                if (x == k) start = 1;
                result += m[s] + m[s - 1];
            }
            else ++m[s];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countSubarrays(int[] nums, int k) {
        var map = new HashMap<Integer, Integer>();
        map.put(0, 1);
        int s = 0, result = 0, start = 0;
        for (int x : nums) {
            s += (x > k ? 1 : (x == k ? 0 : -1));
            if (start != 0 || x == k) {
                if (x == k) start = 1;
                result += map.getOrDefault(s, 0) + map.getOrDefault(s - 1, 0);
            }
            else map.merge(s, 1, Integer::sum);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        m, s, result, start = {0: 1}, 0, 0, 0
        for x in nums:
            s += (1 if x > k else 0 if x == k else -1)
            if start or x == k:
                start = 1 if x == k else start
                result += m.get(s, 0) + m.get(s - 1, 0)
            else:
                m[s] = m.get(s, 0) + 1
        return result
```
