# 2870 Minimum Number of Operations to Make Array Empty 使数组为空的最少操作次数

__Description:__

You are given a __0-indexed__ array `nums` consisting of positive integers.

There are two types of operations that you can apply on the array any number of times:

- Choose __two__ elements with __equal__ values and __delete__ them from the array.
- Choose __three__ elements with __equal__ values and __delete__ them from the array.

Return _the __minimum__ number of operations required to make the array empty, or_ `-1` _if it is not possible_.

__Example:__

Example 1:

```text
Input: nums = [2,3,3,2,2,4,2,3,4]
Output: 4
Explanation: We can apply the following operations to make the array empty:
```

- Apply the first operation on the elements at indices 0 and 3. The resulting array is nums = [3,3,2,4,2,3,4].
- Apply the first operation on the elements at indices 2 and 4. The resulting array is nums = [3,3,4,3,4].
- Apply the second operation on the elements at indices 0, 1, and 3. The resulting array is nums = [4,4].
- Apply the first operation on the elements at indices 0 and 1. The resulting array is nums = [].

It can be shown that we cannot make the array empty in less than 4 operations.

Example 2:

```text
Input: nums = [2,1,2,2,3,3]
Output: -1
Explanation: It is impossible to empty the array.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

Note: This question is the same as 2244: Minimum Rounds to Complete All Tasks.

__题目描述:__

给你一个下标从 __0__ 开始的正整数数组 `nums` 。

你可以对数组执行以下两种操作 任意次 ：

- 从数组中选择 __两个__ 值 __相等__ 的元素，并将它们从数组中 __删除__ 。
- 从数组中选择 __三个__ 值 __相等__ 的元素，并将它们从数组中 __删除__ 。

请你返回使数组为空的 __最少__ 操作次数，如果无法达成，请返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [2,3,3,2,2,4,2,3,4]
输出：4
解释：我们可以执行以下操作使数组为空：
```

- 对下标为 0 和 3 的元素执行第一种操作，得到 nums = [3,3,2,4,2,3,4] 。
- 对下标为 2 和 4 的元素执行第一种操作，得到 nums = [3,3,4,3,4] 。
- 对下标为 0 ，1 和 3 的元素执行第二种操作，得到 nums = [4,4] 。
- 对下标为 0 和 1 的元素执行第一种操作，得到 nums = [] 。

至少需要 4 步操作使数组为空。

示例 2：

```text
输入：nums = [2,1,2,2,3,3]
输出：-1
解释：无法使数组为空。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
模拟
统计元素出现的个数
如果为 1 不能操作
否则一直减去 3 直到最后一次减去 2
所以能除尽 3 就取除 3 的结果
否则除 3 之后需要加 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        for (const auto& num : nums) ++m[num];
        int result = 0;
        for (const auto& [_, v] : m) 
        {
            if (v <= 1) return -1;
            if (!(v % 3)) result += v / 3;
            else result += v / 3 + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        var map = new HashMap<Integer, Integer>();
        for (int num : nums) map.merge(num, 1, Integer::sum);
        int result = 0;
        for (int v : map.values()) {
            if (v <= 1) return -1;
            if (v % 3 == 0) result += v / 3;
            else result += v / 3 + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        return sum(v // 3 if not v % 3 else v // 3 + 1 for v in c.values()) if (c := Counter(nums)) and all(v > 1 for v in c.values()) else -1
```
