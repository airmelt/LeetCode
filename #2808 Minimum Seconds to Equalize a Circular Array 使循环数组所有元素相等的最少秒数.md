# 2808 Minimum Seconds to Equalize a Circular Array 使循环数组所有元素相等的最少秒数

__Description:__

You are given a __0-indexed__ array `nums` containing `n` integers.

At each second, you perform the following operation on the array:

- For every index `i` in the range `[0, n - 1]`, replace `nums[i]` with either `nums[i]`, `nums[(i - 1 + n) % n]`, or `nums[(i + 1) % n]`.

Note that all the elements get replaced simultaneously.

Return _the __minimum__ number of seconds needed to make all elements in the array_ `nums` _equal_.

__Example:__

Example 1:

```text
Input: nums = [1,2,1,2]
Output: 1
Explanation: We can equalize the array in 1 second in the following way:
```

- At 1st second, replace values at each index with [nums[3],nums[1],nums[3],nums[3]]. After replacement, nums = [2,2,2,2].

It can be proven that 1 second is the minimum amount of seconds needed for equalizing the array.

Example 2:

```text
Input: nums = [2,1,3,3,2]
Output: 2
Explanation: We can equalize the array in 2 seconds in the following way:
```

- At 1st second, replace values at each index with [nums[0],nums[2],nums[2],nums[2],nums[3]]. After replacement, nums = [2,3,3,3,3].
- At 2nd second, replace values at each index with [nums[1],nums[1],nums[2],nums[3],nums[4]]. After replacement, nums = [3,3,3,3,3].

It can be proven that 2 seconds is the minimum amount of seconds needed for equalizing the array.

Example 3:

```text
Input: nums = [5,5,5,5]
Output: 0
Explanation: We don't need to perform any operations as all elements in the initial array are the same.
```

__Constraints:__

- `1 <= n == nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的数组 `nums` 。

每一秒，你可以对数组执行以下操作：

- 对于范围在 `[0, n - 1]` 内的每一个下标 `i` ，将 `nums[i]` 替换成 `nums[i]` ， `nums[(i - 1 + n) % n]` 或者 `nums[(i + 1) % n]` 三者之一。

注意，所有元素会被同时替换。

请你返回将数组 `nums` 中所有元素变成相等元素所需要的 __最少__ 秒数。

__示例:__

示例 1：

```text
输入：nums = [1,2,1,2]
输出：1
解释：我们可以在 1 秒内将数组变成相等元素：
```

- 第 1 秒，将每个位置的元素分别变为 [nums[3],nums[1],nums[3],nums[3]] 。变化后，nums = [2,2,2,2] 。

1 秒是将数组变成相等元素所需要的最少秒数。

示例 2：

```text
输入：nums = [2,1,3,3,2]
输出：2
解释：我们可以在 2 秒内将数组变成相等元素：
```

- 第 1 秒，将每个位置的元素分别变为 [nums[0],nums[2],nums[2],nums[2],nums[3]] 。变化后，nums = [2,3,3,3,3] 。
- 第 2 秒，将每个位置的元素分别变为 [nums[1],nums[1],nums[2],nums[3],nums[4]] 。变化后，nums = [3,3,3,3,3] 。

2 秒是将数组变成相等元素所需要的最少秒数。

示例 3：

```text
输入：nums = [5,5,5,5]
输出：0
解释：不需要执行任何操作，因为一开始数组中的元素已经全部相等。
```

__提示：__

- `1 <= n == nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
模拟
最后数组一定会全部变成数组中的某个数
按照数组元素对下标进行分组
要达成所有数都是同一个数
那么每组都需要靠在一起
也就是说要找到 max(j - i) for i, j in pairwise(group), 找到组中相邻元素的最大间距
由于组是循环的, 可以在组的末尾加上第一个元素加长度或者初始化的时候初始化为 m + group[0] - group[-1] 来解决
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumSeconds(vector<int>& nums) 
    {
        int n = nums.size(), result = n;
        unordered_map<int, vector<int>> pos;
        for (int i = 0; i < n; i++) pos[nums[i]].emplace_back(i);
        for (const auto& [_, v] : pos)
        {
            int m = v.size(), cur = n - v.back() + v.front();
            for (int i = 1; i < m; i++) cur = max(cur, v[i] - v[i - 1]);
            result = min(result, cur);
        }
        return result >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSeconds(List<Integer> nums) {
        int n = nums.size(), result = n;
        var pos = new HashMap<Integer, List<Integer>>();
        for (int i = 0; i < n; i++) pos.computeIfAbsent(nums.get(i), k -> new ArrayList<>()).add(i);
        for (var v : pos.values()) {
            int m = v.size(), cur = n - v.get(m - 1) + v.get(0);
            for (int i = 1; i < m; i++) cur = Math.max(cur, v.get(i) - v.get(i - 1));
            result = Math.min(result, cur);
        }
        return result >>> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSeconds(self, nums: List[int]) -> int:
        pos, n = defaultdict(list), len(nums)
        for i, num in enumerate(nums):
            pos[num].append(i)
        return min(max(j - i for i, j in pairwise(v + [v[0] + n])) for v in pos.values()) >> 1
```
