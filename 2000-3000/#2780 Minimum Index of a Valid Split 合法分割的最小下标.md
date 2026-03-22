# 2780 Minimum Index of a Valid Split 合法分割的最小下标

__Description:__

An element `x` of an integer array `arr` of length `m` is __dominant__ if __more than half__ the elements of `arr` have a value of `x`.

You are given a __0-indexed__ integer array `nums` of length `n` with one __dominant__ element.

You can split `nums` at an index `i` into two arrays `nums[0, ..., i]` and `nums[i + 1, ..., n - 1]`, but the split is only __valid__ if:

- `0 <= i < n - 1`
- `nums[0, ..., i]`, and `nums[i + 1, ..., n - 1]` have the same dominant element.

Here, `nums[i, ..., j]` denotes the subarray of `nums` starting at index `i` and ending at index `j`, both ends being inclusive. Particularly, if `j < i` then `nums[i, ..., j]` denotes an empty subarray.

Return _the __minimum__ index of a __valid split___. If no valid split exists, return `-1`.

__Example:__

Example 1:

```text
Input: nums = [1,2,2,2]
Output: 2
Explanation: We can split the array at index 2 to obtain arrays [1,2,2] and [2]. 
In array [1,2,2], element 2 is dominant since it occurs twice in the array and 2 * 2 > 3. 
In array [2], element 2 is dominant since it occurs once in the array and 1 * 2 > 1.
Both [1,2,2] and [2] have the same dominant element as nums, so this is a valid split. 
It can be shown that index 2 is the minimum index of a valid split.
```

Example 2:

```text
Input: nums = [2,1,3,1,1,1,7,1,2,1]
Output: 4
Explanation: We can split the array at index 4 to obtain arrays [2,1,3,1,1] and [1,7,1,2,1].
In array [2,1,3,1,1], element 1 is dominant since it occurs thrice in the array and 3 * 2 > 5.
In array [1,7,1,2,1], element 1 is dominant since it occurs thrice in the array and 3 * 2 > 5.
Both [2,1,3,1,1] and [1,7,1,2,1] have the same dominant element as nums, so this is a valid split.
It can be shown that index 4 is the minimum index of a valid split.
```

Example 3:

```text
Input: nums = [3,3,3,3,7,2,2]
Output: -1
Explanation: It can be shown that there is no valid split.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `nums` has exactly one dominant element.

__题目描述:__

如果在长度为 `m` 的整数数组 `arr` 中 __超过一半__ 的元素值为 `x`，那么我们称 `x` 是 __支配元素__ 。

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` ，数据保证它含有一个 __支配__ 元素。

你需要在下标 `i` 处将 `nums` 分割成两个数组 `nums[0, ..., i]` 和 `nums[i + 1, ..., n - 1]` ，如果一个分割满足以下条件，我们称它是 __合法__ 的:

- `0 <= i < n - 1`
- `nums[0, ..., i]` 和 `nums[i + 1, ..., n - 1]` 的支配元素相同。

这里， `nums[i, ..., j]` 表示 `nums` 的一个子数组，它开始于下标 `i` ，结束于下标 `j` ，两个端点都包含在子数组内。特别地，如果 `j < i` ，那么 `nums[i, ..., j]` 表示一个空数组。

请你返回一个 __合法分割__ 的 __最小__ 下标。如果合法分割不存在，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,2,2]
输出：2
解释：我们将数组在下标 2 处分割，得到 [1,2,2] 和 [2] 。
数组 [1,2,2] 中，元素 2 是支配元素，因为它在数组中出现了 2 次，且 2 * 2 > 3 。
数组 [2] 中，元素 2 是支配元素，因为它在数组中出现了 1 次，且 1 * 2 > 1 。
两个数组 [1,2,2] 和 [2] 都有与 nums 一样的支配元素，所以这是一个合法分割。
下标 2 是合法分割中的最小下标。
```

示例 2：

```text
输入：nums = [2,1,3,1,1,1,7,1,2,1]
输出：4
解释：我们将数组在下标 4 处分割，得到 [2,1,3,1,1] 和 [1,7,1,2,1] 。
数组 [2,1,3,1,1] 中，元素 1 是支配元素，因为它在数组中出现了 3 次，且 3 * 2 > 5 。
数组 [1,7,1,2,1] 中，元素 1 是支配元素，因为它在数组中出现了 3 次，且 3 * 2 > 5 。
两个数组 [2,1,3,1,1] 和 [1,7,1,2,1] 都有与 nums 一样的支配元素，所以这是一个合法分割。
下标 4 是所有合法分割中的最小下标。
```

示例 3：

```text
输入：nums = [3,3,3,3,7,2,2]
输出：-1
解释：没有合法分割。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `nums` 有且只有一个支配元素。

__思路:__

```text
哈希表
由于两个子数组的支配元素一样
假设为 x
x * 2 > i and x * 2 > n - i
那么有 x * 2 > i + n - i = n
所以 x 也是原数组支配元素, 即 x 出现次数超过数组的一半
那么可以用投票法选出支配元素, 统计支配元素出现的次数
然后遍历找到满足 x * 2 > i and x * 2 > n - i 的分界线
找不到返回 -1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumIndex(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        int k = 0, cur = 0, n = nums.size();
        for (const auto& num : nums) if ((++m[num] << 1) > n and !k) k = num;
        for (int i = 0, v = m[k]; i < n; i++) if (((cur = cur + (nums[i] == k)) << 1) > i + 1 and ((v - cur) << 1) > n - i - 1) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumIndex(List<Integer> nums) {
        var map = new HashMap<Integer, Integer>();
        int k = -1, cur = 0, n = nums.size();
        for (int num : nums) if ((map.merge(num, 1, Integer::sum) << 1) > n && k < 0) k = num;
        for (int i = 0, v = map.get(k); i < n; i++) if (((cur = cur + (nums.get(i) == k ? 1 : 0)) << 1) > i + 1 && ((v - cur) << 1) > n - i - 1) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumIndex(self, nums: List[int]) -> int:
        (k, v), cur, n = Counter(nums).most_common()[0], 0, len(nums)
        for i, num in enumerate(nums):
            if ((cur := cur + (num == k)) << 1) > i + 1 and ((v - cur) << 1) > n - i - 1:
                return i
        return -1
```
