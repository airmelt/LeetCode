# 2501 Longest Square Streak in an Array 数组中最长的方波

__Description:__

You are given an integer array `nums`. A subsequence of `nums` is called a __square streak__ if:

- The length of the subsequence is at least `2`, and
- __after__ sorting the subsequence, each element (except the first element) is the __square__ of the previous number.

Return _the length of the __longest square streak__ in_ `nums`_, or return_ `-1` _if there is no __square streak__._

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [4,3,6,16,8,2]
Output: 3
Explanation: Choose the subsequence [4,16,2]. After sorting it, it becomes [2,4,16].
```

- 4 = 2 * 2.
- 16 = 4 * 4.

Therefore, [4,16,2] is a square streak.

It can be shown that every subsequence of length 4 is not a square streak.

Example 2:

```text
Input: nums = [2,3,5,6,7]
Output: -1
Explanation: There is no square streak in nums so return -1.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `2 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 。如果 `nums` 的子序列满足下述条件，则认为该子序列是一个 __方波__ :

- 子序列的长度至少为 `2` ，并且
- 将子序列从小到大排序 __之后__ ，除第一个元素外，每个元素都是前一个元素的 __平方__ 。

返回 `nums` 中 __最长方波__ 的长度，如果不存在 __方波__ 则返回 `-1` 。

子序列 也是一个数组，可以由另一个数组删除一些或不删除元素且不改变剩余元素的顺序得到。

__示例:__

示例 1 ：

```text
输入：nums = [4,3,6,16,8,2]
输出：3
解释：选出子序列 [4,16,2] 。排序后，得到 [2,4,16] 。
```

- 4 = 2 * 2.
- 16 = 4 * 4.

因此，[4,16,2] 是一个方波.

可以证明长度为 4 的子序列都不是方波。

示例 2 ：

```text
输入：nums = [2,3,5,6,7]
输出：-1
解释：nums 不存在方波，所以返回 -1 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `2 <= nums[i] <= 10 ^ 5`

__思路:__

```text
记忆化搜索
将数组去重
使用集合存储数组元素
遍历集合中的每个元素
计算当前元素的平方
如果平方在集合中，则继续计算平方的平方
直到平方不在集合中
记录当前元素的平方的个数
更新结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestSquareStreak(vector<int>& nums) 
    {
        int n = nums.size(), result = -1, cur = 0;
        unordered_set<long long> s(nums.begin(), nums.end());
        for (auto num : s) 
        {
            for (cur = 0; s.count(num); num *= num) ++cur;
            result = max(result, cur);
        }
        return result > 1 ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSquareStreak(int[] nums) {
        int n = nums.length, result = -1, cur = 0;
        var set = Arrays.stream(nums).mapToObj(Long::valueOf).collect(Collectors.toSet());
        for (var num : set) {
            for (cur = 0; set.contains(num); num *= num) ++cur;
            result = Math.max(result, cur);
        }
        return result > 1 ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def longestSquareStreak(self, nums: List[int]) -> int:
        s = set(nums)
        @lru_cache(None)
        def dfs(num: int) -> int:
            return 0 if num not in s else 1 + dfs(num ** 2)
        return result if (result := max(map(dfs, s))) > 1 else -1
```
