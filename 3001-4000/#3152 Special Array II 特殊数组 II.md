# 3152 Special Array II 特殊数组 II

__Description:__

An array is considered __special__ if every pair of its adjacent elements contains two numbers with different parity.

You are given an array of integer `nums` and a 2D integer matrix `queries`, where for `queries[i] = [from_i, to_i]` your task is to check that __subarray__ `nums[from_i..to_i]` is __special__ or not.

Return an array of booleans `answer` such that `answer[i]` is `true` if `nums[from_i..to_i]` is special.

__Example:__

Example 1:

```text
Input: nums = [3,4,1,2,6], queries = [[0,4]]

Output: [false]

Explanation:

The subarray is [3,4,1,2,6]. 2 and 6 are both even.
```

Example 2:

```text
Input: nums = [4,3,1,6], queries = [[0,2],[2,3]]

Output: [false,true]

Explanation:

The subarray is [4,3,1]. 3 and 1 are both odd. So the answer to this query is false.
The subarray is [1,6]. There is only one pair: (1,6) and it contains numbers with different parity. So the answer to this query is true.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= queries[i][0] <= queries[i][1] <= nums.length - 1`

__题目描述:__

如果数组的每一对相邻元素都是两个奇偶性不同的数字，则该数组被认为是一个 __特殊数组__ 。

你有一个整数数组 `nums` 和一个二维整数矩阵 `queries`，对于 `queries[i] = [from_i, to_i]`，请你帮助你检查 __子数组__ `nums[from_i..to_i]` 是不是一个 __特殊数组__ 。

返回布尔数组 `answer`，如果 `nums[from_i..to_i]` 是特殊数组，则 `answer[i]` 为 `true` ，否则，`answer[i]` 为 `false` 。

__示例:__

示例 1：

```text
输入：nums = [3,4,1,2,6], queries = [[0,4]]

输出：[false]

解释：

子数组是 [3,4,1,2,6]。2 和 6 都是偶数。
```

示例 2：

```text
输入：nums = [4,3,1,6], queries = [[0,2],[2,3]]

输出：[false,true]

解释：

子数组是 [4,3,1]。3 和 1 都是奇数。因此这个查询的答案是 false。
子数组是 [1,6]。只有一对：(1,6)，且包含了奇偶性不同的数字。因此这个查询的答案是 true。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= queries[i][0] <= queries[i][1] <= nums.length - 1`

__思路:__

```text
前缀和
记录相邻元素异或的值并与 1 异或的值的前缀和
直接返回前缀和的区间是否为 0 即可
时间复杂度为 O(N + Q), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) 
    {
        int n = nums.size(), m = queries.size();
        vector<int> pre(n);
        vector<bool> result(m);
        for (int i = 1; i < n; i++) pre[i] = pre[i - 1] + ((nums[i] ^ nums[i - 1] ^ 1) & 1);
        for (int i = 0; i < m; i++) result[i] = pre[queries[i].front()] == pre[queries[i].back()];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean[] isArraySpecial(int[] nums, int[][] queries) {
        int n = nums.length, m = queries.length, pre[] = new int[n];
        for (int i = 1; i < n; i++) pre[i] = pre[i - 1] + ((nums[i] ^ nums[i - 1] ^ 1) & 1);
        boolean[] result = new boolean[m];
        for (int i = 0; i < m; i++) result[i] = pre[queries[i][0]] == pre[queries[i][1]];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def isArraySpecial(self, nums: List[int], queries: List[List[int]]) -> List[bool]:
        return [pre[f] == pre[t] for f, t in queries] if (pre := list(accumulate(((x ^ y ^ 1) & 1 for x, y in pairwise(nums)), initial=0))) else []
```
