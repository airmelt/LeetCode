# 2784 Check if Array is Good 检查数组是否是好的

__Description:__

You are given an integer array `nums`. We consider an array __good__ if it is a permutation of an array `base[n]`.

`base[n] = [1, 2, ..., n - 1, n, n]` (in other words, it is an array of length `n + 1` which contains `1` to `n - 1` exactly once, plus two occurrences of `n`). For example, `base[1] = [1, 1]` and `base[3] = [1, 2, 3, 3]`.

Return `true` _if the given array is good, otherwise return_ `false`.

Note: A permutation of integers represents an arrangement of these numbers.

__Example:__

Example 1:

```text
Input: nums = [2, 1, 3]
Output: false
Explanation: Since the maximum element of the array is 3, the only candidate n for which this array could be a permutation of base[n], is n = 3. However, base[3] has four elements but array nums has three. Therefore, it can not be a permutation of base[3] = [1, 2, 3, 3]. So the answer is false.
```

Example 2:

```text
Input: nums = [1, 3, 3, 2]
Output: true
Explanation: Since the maximum element of the array is 3, the only candidate n for which this array could be a permutation of base[n], is n = 3. It can be seen that nums is a permutation of base[3] = [1, 2, 3, 3] (by swapping the second and fourth elements in nums, we reach base[3]). Therefore, the answer is true.
```

Example 3:

```text
Input: nums = [1, 1]
Output: true
Explanation: Since the maximum element of the array is 1, the only candidate n for which this array could be a permutation of base[n], is n = 1. It can be seen that nums is a permutation of base[1] = [1, 1]. Therefore, the answer is true.
```

Example 4:

```text
Input: nums = [3, 4, 4, 1, 2, 1]
Output: false
Explanation: Since the maximum element of the array is 4, the only candidate n for which this array could be a permutation of base[n], is n = 4. However, base[4] has five elements but array nums has six. Therefore, it can not be a permutation of base[4] = [1, 2, 3, 4, 4]. So the answer is false.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= num[i] <= 200`

__题目描述:__

给你一个整数数组 `nums` ，如果它是数组 `base[n]` 的一个排列，我们称它是个 __好__ 数组。

`base[n] = [1, 2, ..., n - 1, n, n]` （换句话说，它是一个长度为 `n + 1` 且包含 `1` 到 `n - 1` 恰好各一次，包含 `n` 两次的一个数组）。比方说， `base[1] = [1, 1]` ， `base[3] = [1, 2, 3, 3]` 。

如果数组是一个好数组，请你返回 `true` ，否则返回 `false` 。

注意：数组的排列是这些数字按任意顺序排布后重新得到的数组。

__示例:__

示例 1：

```text
输入：nums = [2, 1, 3]
输出：false
解释：因为数组的最大元素是 3 ，唯一可以构成这个数组的 base[n] 对应的 n = 3 。但是 base[3] 有 4 个元素，但数组 nums 只有 3 个元素，所以无法得到 base[3] = [1, 2, 3, 3] 的排列，所以答案为 false 。
```

示例 2：

```text
输入：nums = [1, 3, 3, 2]
输出：true
解释：因为数组的最大元素是 3 ，唯一可以构成这个数组的 base[n] 对应的 n = 3 ，可以看出数组是 base[3] = [1, 2, 3, 3] 的一个排列（交换 nums 中第二个和第四个元素）。所以答案为 true 。
```

示例 3：

```text
输入：nums = [1, 1]
输出：true
解释：因为数组的最大元素是 1 ，唯一可以构成这个数组的 base[n] 对应的 n = 1，可以看出数组是 base[1] = [1, 1] 的一个排列。所以答案为 true 。
```

示例 4：

```text
输入：nums = [3, 4, 4, 1, 2, 1]
输出：false
解释：因为数组的最大元素是 4 ，唯一可以构成这个数组的 base[n] 对应的 n = 4 。但是 base[n] 有 5 个元素而 nums 有 6 个元素。所以答案为 false 。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= num[i] <= 200`

__思路:__

```text
1. 排序(Python)
先将数组排序
再检查是否为 1 - n, 且 n 出现两次
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 遍历
用一个数组记录出现过的次数
除了 n 出现 2 次
其余数字只能出现 1 次
并且所有数字都在 [1, n] 范围内
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isGood(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int> visited(n);
        for (const auto& num : nums) 
        {
            if (num > n - 1 or num == n - 1 and visited[num] > 1 or num < n - 1 and visited[num]) return false;
            ++visited[num];
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isGood(int[] nums) {
        int n = nums.length, visited[] = new int[n];
        for (int num : nums) {
            if (num > n - 1 || num == n - 1 && visited[num] > 1 || num < n - 1 && visited[num] == 1) return false;
            ++visited[num];
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isGood(self, nums: List[int]) -> bool:
        return sorted(nums) == list(range(1, len(nums) - 1)) + [len(nums) - 1, len(nums) - 1]
```
