# 2190 Most Frequent Number Following Key In an Array 数组中紧跟 key 之后出现最频繁的数字

__Description:__

You are given a __0-indexed__ integer array `nums`. You are also given an integer `key`, which is present in `nums`.

For every unique integer `target` in `nums`, __count__ the number of times `target` immediately follows an occurrence of `key` in `nums`. In other words, count the number of indices `i` such that:

- `0 <= i <= nums.length - 2`,
- `nums[i] == key` and,
- `nums[i + 1] == target`.

Return _the_ `target` _with the __maximum__ count_. The test cases will be generated such that the `target` with maximum count is unique.

__Example:__

Example 1:

```text
Input: nums = [1,100,200,1,100], key = 1
Output: 100
Explanation: For target = 100, there are 2 occurrences at indices 1 and 4 which follow an occurrence of key.
No other integers follow an occurrence of key, so we return 100.
```

Example 2:

```text
Input: nums = [2,2,2,2,3], key = 2
Output: 2
Explanation: For target = 2, there are 3 occurrences at indices 1, 2, and 3 which follow an occurrence of key.
For target = 3, there is only one occurrence at index 4 which follows an occurrence of key.
target = 2 has the maximum number of occurrences following an occurrence of key, so we return 2.
```

__Constraints:__

- `2 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- The test cases will be generated such that the answer is unique.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，同时给你一个整数 `key` ，它在 `nums` 出现过。

__统计__ 在 `nums` 数组中紧跟着 `key` 后面出现的不同整数 `target` 的出现次数。换言之， `target` 的出现次数为满足以下条件的 `i` 的数目:

- `0 <= i <= n - 2`
- `nums[i] == key` 且
- `nums[i + 1] == target` 。

请你返回出现 __最多__ 次数的 `target` 。测试数据保证出现次数最多的 `target` 是唯一的。

__示例:__

示例 1：

```text
输入：nums = [1,100,200,1,100], key = 1
输出：100
解释：对于 target = 100 ，在下标 1 和 4 处出现过 2 次，且都紧跟着 key 。
没有其他整数在 key 后面紧跟着出现，所以我们返回 100 。
```

示例 2：

```text
输入：nums = [2,2,2,2,3], key = 2
输出：2
解释：对于 target = 2 ，在下标 1 ，2 和 3 处出现过 3 次，且都紧跟着 key 。
对于 target = 3 ，在下标 4 出出现过 1 次，且紧跟着 key 。
target = 2 是紧跟着 key 之后出现次数最多的数字，所以我们返回 2 。
```

__提示：__

- `2 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- 测试数据保证答案是唯一的。

__思路:__

```text
哈希表
统计所有 key 后面的数字出现的次数, 返回出现次数最多的数字
可以在一次遍历的时候统计出现次数, 将结果更新为出现次数最多的数字
时间复杂度为 O(N), 空间复杂度为 O(M), M 为数组中不同的数字个数
```

__代码:__

__C++__:

```C++
class Solution:
    def mostFrequent(self, nums: List[int], key: int) -> int:
        return Counter(nums[i + 1] for i, num in enumerate(nums[:-1]) if num == key).most_common(1)[0][0]
```

__Java__:

```Java
class Solution {
    public int mostFrequent(int[] nums, int key) {
        int result = 0, m = 0, n = nums.length, count[] = new int[1001];
        for (int i = 0; i < n - 1; i++) if (nums[i] == key && ++count[nums[i + 1]] > count[result]) result = nums[i + 1];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mostFrequent(self, nums: List[int], key: int) -> int:
        return Counter(nums[i + 1] for i, num in enumerate(nums[:-1]) if num == key).most_common(1)[0][0]
```
