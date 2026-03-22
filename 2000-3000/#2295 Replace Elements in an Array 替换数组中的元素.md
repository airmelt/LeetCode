# 2295 Replace Elements in an Array 替换数组中的元素

__Description:__

You are given a __0-indexed__ array `nums` that consists of `n` __distinct__ positive integers. Apply `m` operations to this array, where in the `i ^ th` operation you replace the number `operations[i][0]` with `operations[i][1]`.

It is guaranteed that in the `i ^ th` operation:

- `operations[i][0]` __exists__ in `nums`.
- `operations[i][1]` does __not__ exist in `nums`.

Return the array obtained after applying all the operations.

__Example:__

Example 1:

```text
Input: nums = [1,2,4,6], operations = [[1,3],[4,7],[6,1]]
Output: [3,2,7,1]
Explanation: We perform the following operations on nums:
```

- Replace the number 1 with 3. nums becomes [3,2,4,6].
- Replace the number 4 with 7. nums becomes [3,2,7,6].
- Replace the number 6 with 1. nums becomes [3,2,7,1].

We return the final array [3,2,7,1].

Example 2:

```text
Input: nums = [1,2], operations = [[1,3],[2,1],[3,2]]
Output: [2,1]
Explanation: We perform the following operations to nums:
```

- Replace the number 1 with 3. nums becomes [3,2].
- Replace the number 2 with 1. nums becomes [3,1].
- Replace the number 3 with 2. nums becomes [2,1].

We return the array [2,1].

__Constraints:__

- `n == nums.length`
- `m == operations.length`
- `1 <= n, m <= 10 ^ 5`
- All the values of `nums` are __distinct__.
- `operations[i].length == 2`
- `1 <= nums[i], operations[i][0], operations[i][1] <= 10 ^ 6`
- `operations[i][0]` will exist in `nums` when applying the `i ^ th` operation.
- `operations[i][1]` will not exist in `nums` when applying the `i ^ th` operation.

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，它包含 `n` 个 __互不相同__ 的正整数。请你对这个数组执行 `m` 个操作，在第 `i` 个操作中，你需要将数字 `operations[i][0]` 替换成 `operations[i][1]` 。

题目保证在第 `i` 个操作中:

- `operations[i][0]` 在 `nums` 中存在。
- `operations[i][1]` 在 `nums` 中不存在。

请你返回执行完所有操作后的数组。

__示例:__

示例 1：

```text
输入：nums = [1,2,4,6], operations = [[1,3],[4,7],[6,1]]
输出：[3,2,7,1]
解释：我们对 nums 执行以下操作：
```

- 将数字 1 替换为 3 。nums 变为 [3,2,4,6] 。
- 将数字 4 替换为 7 。nums 变为 [3,2,7,6] 。
- 将数字 6 替换为 1 。nums 变为 [3,2,7,1] 。

返回最终数组 [3,2,7,1] 。

示例 2：

```text
输入：nums = [1,2], operations = [[1,3],[2,1],[3,2]]
输出：[2,1]
解释：我们对 nums 执行以下操作：
```

- 将数字 1 替换为 3 。nums 变为 [3,2] 。
- 将数字 2 替换为 1 。nums 变为 [3,1] 。
- 将数字 3 替换为 2 。nums 变为 [2,1] 。

返回最终数组 [2,1] 。

__提示：__

- `n == nums.length`
- `m == operations.length`
- `1 <= n, m <= 10 ^ 5`
- `nums` 中所有数字 __互不相同__ 。
- `operations[i].length == 2`
- `1 <= nums[i], operations[i][0], operations[i][1] <= 10 ^ 6`
- 在执行第 `i` 个操作时， `operations[i][0]` 在 `nums` 中存在。
- 在执行第 `i` 个操作时， `operations[i][1]` 在 `nums` 中不存在。

__思路:__

```text
哈希表
1. 正序
用哈希表存储 nums 中的元素和其下标的映射关系
由于 operations[i][0] 在 nums 中存在，operations[i][1] 在 nums 中不存在
所以我们可以直接将 operations[i][0] 替换为 operations[i][1]
然后将 nums 中 operations[i][0] 的下标更新为 operations[i][1] 的下标
时间复杂度为 O(max(M, N)), 空间复杂度为 O(N)
2. 逆序
设 x = operations[i][0], y = operations[i][1]
将 x 映射到 m[y], 即 m[x] = m[y], y 不存在时 m[y] = y
最后将 nums 中的元素替换为 m[nums[i]], nums[i] 不存在时 m[nums[i]] = nums[i]
时间复杂度为 O(max(M, N)), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> arrayChange(vector<int>& nums, vector<vector<int>>& operations) 
    {
        unordered_map<int, int> m;
        for (int i = 0, n = nums.size(); i < n; i++) m[nums[i]] = i;
        for (const auto& o : operations) 
        {
            m[o.back()] = m[o.front()];
            nums[m[o.front()]] = o.back();
        }
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] arrayChange(int[] nums, int[][] operations) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0, n = nums.length; i < n; i++) map.put(nums[i], i);
        for (int[] o : operations) {
            map.put(o[1], map.get(o[0]));
            nums[map.get(o[0])] = o[1];
        }
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def arrayChange(self, nums: List[int], operations: List[List[int]]) -> List[int]:
        m = {}
        for x, y in operations[::-1]:
            m[x] = m.get(y, y)
        return [m.get(num, num) for num in nums]
```
