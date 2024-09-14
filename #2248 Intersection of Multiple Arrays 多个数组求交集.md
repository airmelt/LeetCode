# 2248 Intersection of Multiple Arrays 多个数组求交集

__Description:__

__Example:__

Example 1:

```text
Input: nums = [[3,1,2,4,5],[1,2,3,4],[3,4,5,6]]
Output: [3,4]
Explanation: 
The only integers present in each of nums[0] = [3,1,2,4,5], nums[1] = [1,2,3,4], and nums[2] = [3,4,5,6] are 3 and 4, so we return [3,4].
```

Example 2:

```text
Input: nums = [[1,2,3],[4,5,6]]
Output: []
Explanation: 
There does not exist any integer present both in nums[0] and nums[1], so we return an empty list [].
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= sum(nums[i].length) <= 1000`
- `1 <= nums[i][j] <= 1000`
- All the values of `nums[i]` are __unique__.

__题目描述:__

给你一个二维整数数组 `nums` ，其中 `nums[i]` 是由 __不同__ 正整数组成的一个非空数组，按 __升序排列__ 返回一个数组，数组中的每个元素在 `nums` __所有数组__ 中都出现过。

__示例:__

示例 1：

```text
输入：nums = [[3,1,2,4,5],[1,2,3,4],[3,4,5,6]]
输出：[3,4]
解释：
nums[0] = [3,1,2,4,5]，nums[1] = [1,2,3,4]，nums[2] = [3,4,5,6]，在 nums 中每个数组中都出现的数字是 3 和 4 ，所以返回 [3,4] 。
```

示例 2：

```text
输入：nums = [[1,2,3],[4,5,6]]
输出：[]
解释：
不存在同时出现在 nums[0] 和 nums[1] 的整数，所以返回一个空列表 [] 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= sum(nums[i].length) <= 1000`
- `1 <= nums[i][j] <= 1000`
- `nums[i]` 中的所有值 __互不相同__

__思路:__

```text
模拟
统计数字出现的个数
如果出现的次数和 nums 长度相同就加入结果
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> intersection(vector<vector<int>>& nums) 
    {
        vector<int> result, c(1001);
        for (const auto& x : nums) for (const auto& y : x) ++c[y];
        for (int i = 1, n = nums.size(); i < 1001; i++) if (c[i] == n) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> intersection(int[][] nums) {
        List<Integer> result = new ArrayList<>();
        int count[] = new int[1001], n = nums.length;
        for (int[] x : nums) for (int y : x) ++count[y];
        for (int i = 1; i < 1001; i++) if (count[i] == n) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def intersection(self, nums: List[List[int]]) -> List[int]:
        return sorted(list(reduce(lambda x, y: x & y, [set(i) for i in nums])))
```
