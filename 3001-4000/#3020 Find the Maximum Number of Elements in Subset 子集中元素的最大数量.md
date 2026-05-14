# 3020 Find the Maximum Number of Elements in Subset 子集中元素的最大数量

__Description:__

You are given an array of __positive__ integers `nums`.

You need to select a <span data-keyword="subset" class=" cursor-pointer relative text-dark-blue-s text-sm"><button type="button" aria-haspopup="dialog" aria-expanded="false" aria-controls="radix-_r_hg_" data-state="closed" class="">subset</button></span> of `nums` which satisfies the following condition:

- You can place the selected elements in a __0-indexed__ array such that it follows the pattern: `[x, x ^ 2, x ^ 4, ..., x ^ k/2, x ^ k, x ^ k/2, ..., x ^ 4, x ^ 2, x]` (__Note__ that `k` can be be any __non-negative__ power of `2`). For example, `[2, 4, 16, 4, 2]` and `[3, 9, 3]` follow the pattern while `[2, 4, 8, 4, 2]` does not.

Return the maximum number of elements in a subset that satisfies these conditions.

__Example:__

Example 1:

```text
Input: nums = [5,4,1,2,2]
Output: 3
Explanation: We can select the subset {4,2,2}, which can be placed in the array as [2,4,2] which follows the pattern and 22 == 4. Hence the answer is 3.
```

Example 2:

```text
Input: nums = [1,3,2,4]
Output: 1
Explanation: We can select the subset {1}, which can be placed in the array as [1] which follows the pattern. Hence the answer is 1. Note that we could have also selected the subsets {2}, {3}, or {4}, there may be multiple subsets which provide the same answer.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个 __正整数__ 数组 `nums` 。

你需要从数组中选出一个满足下述条件的子集：

- 你可以将选中的元素放置在一个下标从 __0__ 开始的数组中，并使其遵循以下模式: `[x, x ^ 2, x ^ 4, ..., x ^ k/2, x ^ k, x ^ k/2, ..., x ^ 4, x ^ 2, x]`（__注意__， `k` 可以是任何 __非负__ 的 2 的幂）。例如， `[2, 4, 16, 4, 2]` 和 `[3, 9, 3]` 都符合这一模式，而 `[2, 4, 8, 4, 2]` 则不符合。

返回满足这些条件的子集中，元素数量的 最大值 。

__示例:__

示例 1：

```text
输入：nums = [5,4,1,2,2]
输出：3
解释：选择子集 {4,2,2} ，将其放在数组 [2,4,2] 中，它遵循该模式，且 22 == 4 。因此答案是 3 。
```

示例 2：

```text
输入：nums = [1,3,2,4]
输出：1
解释：选择子集 {1}，将其放在数组 [1] 中，它遵循该模式。因此答案是 1 。注意我们也可以选择子集 {2} 、{4} 或 {3} ，可能存在多个子集都能得到相同的答案。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
暴力法
计算 2 ^ (2 ^ k) <= 10 ^ 9 得 k < 5
所以每一个子数组最长为 9
用哈希集合记录每个数字出现的次数
先统计 1 出现的次数
由于模式需要奇数长度, 所以使用 a - 1 | 1 求出不大于 a 的最大奇数
然后删除 1, 后续遍历就不需要再讨论 1
然后从数组中的元素开始查找下一个 2 ^ k 倍数
最后更新结果
时间复杂度为 O(NloglogM), 空间复杂度为 O(N), 每个元素最多查询 5 次, M 为 max(nums)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumLength(vector<int>& nums) 
    {
        unordered_map<long long, int> m;
        for (const auto& num : nums) ++m[num];
        int result = m[1LL] - 1 | 1, cur = 0;
        m.erase(1LL);
        for (auto& [x, _] : m) 
        {
            long long v = x;
            for (cur = 0; m.count(v) and m[v] > 1; v *= v) cur += 2;
            result = max(result, cur + ((int)m.count(v) - 1 | 1));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumLength(int[] nums) {
        var map = new HashMap<Long, Integer>();
        for (int num : nums) map.merge((long)num, 1, Integer::sum);
        int result = map.getOrDefault(1L, 0) - 1 | 1, cur = 0;
        map.remove(1L);
        for (long v : map.keySet()) {
            for (cur = 0; map.getOrDefault(v, 0) > 1; v *= v) cur += 2;
            result = Math.max(result, cur + (map.getOrDefault(v, 0) - 1 | 1));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumLength(self, nums: List[int]) -> int:
        result = (c := Counter(nums))[1] - 1 | 1
        del c[1]
        for v in c:
            cur = 0
            while c[v] > 1:
                cur += 2
                v *= v
            result = max(result, cur + ((v in c) - 1 | 1))
        return result
```
