# 3159 Find Occurrences of an Element in an Array 查询数组中元素的出现位置

__Description:__

You are given an integer array `nums`, an integer array `queries`, and an integer `x`.

For each `queries[i]`, you need to find the index of the `queries[i] ^ th` occurrence of `x` in the `nums` array. If there are fewer than `queries[i]` occurrences of `x`, the answer should be -1 for that query.

Return an integer array `answer` containing the answers to all queries.

__Example:__

Example 1:

```text
Input: nums = [1,3,1,7], queries = [1,3,2,4], x = 1

Output: [0,-1,2,-1]

Explanation:

For the 1st query, the first occurrence of 1 is at index 0.
For the 2nd query, there are only two occurrences of 1 in nums, so the answer is -1.
For the 3rd query, the second occurrence of 1 is at index 2.
For the 4th query, there are only two occurrences of 1 in nums, so the answer is -1.
```

Example 2:

```text
Input: nums = [1,2,3], queries = [10], x = 5

Output: [-1]

Explanation:

For the 1st query, 5 doesn't exist in nums, so the answer is -1.
```

__Constraints:__

- `1 <= nums.length, queries.length <= 10 ^ 5`
- `1 <= queries[i] <= 10 ^ 5`
- `1 <= nums[i], x <= 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` ，一个整数数组 `queries` 和一个整数 `x` 。

对于每个查询 `queries[i]` ，你需要找到 `nums` 中第 `queries[i]` 个 `x` 的位置，并返回它的下标。如果数组中 `x` 的出现次数少于 `queries[i]` ，该查询的答案为 -1 。

请你返回一个整数数组 `answer` ，包含所有查询的答案。

__示例:__

示例 1：

```text
输入：nums = [1,3,1,7], queries = [1,3,2,4], x = 1

输出：[0,-1,2,-1]

解释：

第 1 个查询，第一个 1 出现在下标 0 处。
第 2 个查询，nums 中只有两个 1 ，所以答案为 -1 。
第 3 个查询，第二个 1 出现在下标 2 处。
第 4 个查询，nums 中只有两个 1 ，所以答案为 -1 。
```

示例 2：

```text
输入：nums = [1,2,3], queries = [10], x = 5

输出：[-1]

解释：

第 1 个查询，nums 中没有 5 ，所以答案为 -1 。
```

__提示：__

- `1 <= nums.length, queries.length <= 10 ^ 5`
- `1 <= queries[i] <= 10 ^ 5`
- `1 <= nums[i], x <= 10 ^ 4`

__思路:__

```text
模拟
记录每个 x 的下标
如果 queries[i] < len(pos[x])
返回 pos[x][q] - 1
否则返回 -1
时间复杂度为 O(N + M), 空间复杂度为 O(N), 其中 M 为 queries 数组长度, N 为 nums 数组长度, 不计入返回空间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> occurrencesOfElement(vector<int>& nums, vector<int>& queries, int x) 
    {
        vector<int> pos;
        for (int i = 0, n = nums.size(); i < n; i++) if (nums[i] == x) pos.emplace_back(i);
        for (int i = 0, m = queries.size(); i < m; i++) queries[i] = queries[i] > pos.size() ? -1 : pos[queries[i] - 1];
        return queries;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] occurrencesOfElement(int[] nums, int[] queries, int x) {
        var pos = new ArrayList<Integer>();
        for (int i = 0, n = nums.length; i < n; i++) if (nums[i] == x) pos.add(i);
        for (int i = 0, m = queries.length; i < m; i++) queries[i] = queries[i] > pos.size() ? -1 : pos.get(queries[i] - 1);
        return queries;
    }
}
```

__Python__:

```Python
class Solution:
    def occurrencesOfElement(self, nums: List[int], queries: List[int], x: int) -> List[int]:
        return [-1 if q > len(pos) else pos[q - 1] for q in queries] if (pos := [i for i, num in enumerate(nums) if num == x]) else [-1] * len(queries)
```
