# 2150 Find All Lonely Numbers in the Array 找出数组中的所有孤独数字

__Description:__

You are given an integer array `nums`. A number `x` is __lonely__ when it appears only __once__, and no __adjacent__ numbers (i.e. `x + 1` and `x - 1`) appear in the array.

Return ___all__ lonely numbers in_ `nums`. You may return the answer in __any order__.

__Example:__

Example 1:

```text
Input: nums = [10,6,5,8]
Output: [10,8]
Explanation: 
```

- 10 is a lonely number since it appears exactly once and 9 and 11 does not appear in nums.
- 8 is a lonely number since it appears exactly once and 7 and 9 does not appear in nums.
- 5 is not a lonely number since 6 appears in nums and vice versa.

Hence, the lonely numbers in nums are [10, 8].
Note that [8, 10] may also be returned.

Example 2:

```text
Input: nums = [1,3,5,3]
Output: [1,5]
Explanation: 
```

- 1 is a lonely number since it appears exactly once and 0 and 2 does not appear in nums.
- 5 is a lonely number since it appears exactly once and 4 and 6 does not appear in nums.
- 3 is not a lonely number since it appears twice.

Hence, the lonely numbers in nums are [1, 5].
Note that [5, 1] may also be returned.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个整数数组 `nums` 。如果数字 `x` 在数组中仅出现 __一次__ ，且没有 __相邻__ 数字（即， `x + 1` 和 `x - 1`）出现在数组中，则认为数字 `x` 是 __孤独数字__ 。

返回 `nums` 中的 __所有__ 孤独数字。你可以按 __任何顺序__ 返回答案。

__示例:__

示例 1：

```text
输入：nums = [10,6,5,8]
输出：[10,8]
解释：
```

- 10 是一个孤独数字，因为它只出现一次，并且 9 和 11 没有在 nums 中出现。
- 8 是一个孤独数字，因为它只出现一次，并且 7 和 9 没有在 nums 中出现。
- 5 不是一个孤独数字，因为 6 出现在 nums 中，反之亦然。

因此，nums 中的孤独数字是 [10, 8] 。
注意，也可以返回 [8, 10] 。

示例 2：

```text
输入：nums = [1,3,5,3]
输出：[1,5]
解释：
```

- 1 是一个孤独数字，因为它只出现一次，并且 0 和 2 没有在 nums 中出现。
- 5 是一个孤独数字，因为它只出现一次，并且 4 和 6 没有在 nums 中出现。
- 3 不是一个孤独数字，因为它出现两次。

因此，nums 中的孤独数字是 [1, 5] 。
注意，也可以返回 [5, 1] 。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__思路:__

```text
1. 哈希表
用哈希表记录每个元素出现的个数
再次遍历, 如果元素出现的次数为 1, 且相邻元素出现次数为 0, 就加入列表
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 排序
将列表排序
与相邻元素差距均为 2 或以上的就加入结果
为了避免边界讨论, 也可以加入两个哨兵 -2 和 1000002
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findLonely(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        vector<int> result;
        for (int i = 0, n = nums.size(); i < n; i++) if ((n > 1 and ((!i and nums[1] - nums.front() > 1) or (i == n - 1 and nums.back() - nums[i - 1] > 1))) or (i and i < n - 1 and nums[i] - nums[i - 1] > 1 and nums[i + 1] - nums[i] > 1)) result.emplace_back(nums[i]);
        return nums.size() == 1 ? nums : result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findLonely(int[] nums) {
        List<Integer> result = new ArrayList<>();
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) map.merge(num, 1, Integer::sum);
        for (int num : nums) if (!map.containsKey(num + 1) && !map.containsKey(num - 1) && map.get(num) == 1) result.add(num);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findLonely(self, nums: List[int]) -> List[int]:
        return [] if not (c := Counter(nums)) else [i for i in nums if c[i] == 1 and c[i + 1] == c[i - 1] == 0] 
```
