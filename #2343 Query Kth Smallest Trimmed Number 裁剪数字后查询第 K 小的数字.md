# 2343 Query Kth Smallest Trimmed Number 裁剪数字后查询第 K 小的数字

__Description:__

You are given a __0-indexed__ array of strings `nums`, where each string is of __equal length__ and consists of only digits.

You are also given a __0-indexed__ 2D integer array `queries` where `queries[i] = [k_i, trim_i]`. For each `queries[i]`, you need to:

- __Trim__ each number in `nums` to its __rightmost__ `trim_i` digits.
- Determine the __index__ of the `k_i ^ th` smallest trimmed number in `nums`. If two trimmed numbers are equal, the number with the __lower__ index is considered to be smaller.
- Reset each number in `nums` to its original length.

Return _an array_ `answer` _of the same length as_ `queries`, _where_ `answer[i]` _is the answer to the_ `i ^ th` _query._

Note:

- To trim to the rightmost `x` digits means to keep removing the leftmost digit, until only `x` digits remain.
- Strings in `nums` may contain leading zeros.

__Example:__

Example 1:

```text
Input: nums = ["102","473","251","814"], queries = [[1,1],[2,3],[4,2],[1,2]]
Output: [2,2,1,0]
Explanation:
```

1. After trimming to the last digit, nums = ["2","3","1","4"]. The smallest number is 1 at index 2.
2. Trimmed to the last 3 digits, nums is unchanged. The 2nd smallest number is 251 at index 2.
3. Trimmed to the last 2 digits, nums = ["02","73","51","14"]. The 4th smallest number is 73.
4. Trimmed to the last 2 digits, the smallest number is 2 at index 0.

Note that the trimmed number "02" is evaluated as 2.

Example 2:

```text
Input: nums = ["24","37","96","04"], queries = [[2,1],[2,2]]
Output: [3,0]
Explanation:
```

1. Trimmed to the last digit, nums = ["4","7","6","4"]. The 2nd smallest number is 4 at index 3.
   There are two occurrences of 4, but the one at index 0 is considered smaller than the one at index 3.
2. Trimmed to the last 2 digits, nums is unchanged. The 2nd smallest number is 24.

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i].length <= 100`
- `nums[i]` consists of only digits.
- All `nums[i].length` are __equal__.
- `1 <= queries.length <= 100`
- `queries[i].length == 2`
- `1 <= k_i <= nums.length`
- `1 <= trim_i <= nums[i].length`

Follow up: Could you use the Radix Sort Algorithm to solve this problem? What will be the complexity of that solution?

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `nums` ，其中每个字符串 __长度相等__ 且只包含数字。

再给你一个下标从 __0__ 开始的二维整数数组 `queries` ，其中 `queries[i] = [k_i, trim_i]` 。对于每个 `queries[i]` ，你需要:

- 将 `nums` 中每个数字 __裁剪__ 到剩下 __最右边__ `trim_i` 个数位。
- 在裁剪过后的数字中，找到 `nums` 中第 `k_i` 小数字对应的 __下标__ 。如果两个裁剪后数字一样大，那么下标 __更小__ 的数字视为更小的数字。
- 将 `nums` 中每个数字恢复到原本字符串。

请你返回一个长度与 `queries` 相等的数组 `answer`，其中 `answer[i]`是第 `i` 次查询的结果。

__提示：__

- 裁剪到剩下最右边 `x` 个数位的意思是不断删除最左边的数位，直到剩下 `x` 个数位。
- `nums` 中的字符串可能会有前导 0 。

__示例:__

示例 1：

```text
输入：nums = ["102","473","251","814"], queries = [[1,1],[2,3],[4,2],[1,2]]
输出：[2,2,1,0]
解释：
```

1. 裁剪到只剩 1 个数位后，nums = ["2","3","1","4"] 。最小的数字是 1 ，下标为 2 。
2. 裁剪到剩 3 个数位后，nums 没有变化。第 2 小的数字是 251 ，下标为 2 。
3. 裁剪到剩 2 个数位后，nums = ["02","73","51","14"] 。第 4 小的数字是 73 ，下标为 1 。
4. 裁剪到剩 2 个数位后，最小数字是 2 ，下标为 0 。

注意，裁剪后数字 "02" 值为 2 。

示例 2：

```text
输入：nums = ["24","37","96","04"], queries = [[2,1],[2,2]]
输出：[3,0]
解释：
```

1. 裁剪到剩 1 个数位，nums = ["4","7","6","4"] 。第 2 小的数字是 4 ，下标为 3 。
   有两个 4 ，下标为 0 的 4 视为小于下标为 3 的 4 。
2. 裁剪到剩 2 个数位，nums 不变。第二小的数字是 24 ，下标为 0 。

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i].length <= 100`
- `nums[i]` 只包含数字。
- 所有 `nums[i].length` 的长度 _相同_ 。
- `1 <= queries.length <= 100`
- `queries[i].length == 2`
- `1 <= k_i <= nums.length`
- `1 <= trim_i <= nums[0].length`

进阶：你能使用 基数排序算法 解决此问题吗？这种解法的复杂度又是多少？

__思路:__

```text
1. 暴力法
按照题目要求裁剪数位并排序
取第 k 小的下标
时间复杂度为 O(QMNlogN), 空间复杂度为 O(N), 其中 Q 为 queries 的长度, N 为 nums 的长度, M 为 nums 中字符串的长度
2. 离线 ➕ 基数排序
将 queries 按照 trim_i 从小到大排序
返回时也按照这一顺序回答
每次排序时, 从 1 到 queries[i][1] 依次进行基数排序
在给 queries 排序时, 可以只排序下标, 然后根据下标进行遍历
使用稳定排序, 保证相同的数字的顺序不变
时间复杂度为 O(QlogQ + MN), 空间复杂度为 O(Q + N), 其中 Q 为 queries 的长度, N 为 nums 的长度, M 为 nums 中字符串的长度, 使用基数排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> smallestTrimmedNumbers(vector<string>& nums, vector<vector<int>>& queries) 
    {
        int size = queries.size(), qid[size], n = nums.size(), m = nums.front().size(), idx[n], p = 1;
        iota(qid, qid + size, 0);
        sort(qid, qid + size, [&](int a, int b) { return queries[a][1] < queries[b][1]; });
        iota(idx, idx + n, 0);
        vector<int> result(size);
        for (const auto& i : qid)
        {
            for (; p <= queries[i].back(); p++) stable_sort(idx, idx + n, [&](int a, int b) { return nums[a][m - p] < nums[b][m - p]; });
            result[i] = idx[queries[i].front() - 1];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] smallestTrimmedNumbers(String[] nums, int[][] queries) {
        var qid = IntStream.range(0, queries.length).boxed().toArray(Integer[]::new);
        Arrays.sort(qid, (i, j) -> queries[i][1] - queries[j][1]);
        int m = nums[0].length(), result[] = new int[queries.length], p = 1;
        var idx = new ArrayList<>(Arrays.asList(IntStream.range(0, nums.length).boxed().toArray(Integer[]::new)));
        for (var i : qid) {
            for (; p <= queries[i][1]; ++p) {
                final int pp = p++;
                Collections.sort(idx, (a, b) -> nums[a].charAt(m - pp) - nums[b].charAt(m - pp));
            }
            result[i] = idx.get(queries[i][0] - 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestTrimmedNumbers(self, nums: List[str], queries: List[List[int]]) -> List[int]:
        return [sorted((s[-trim:], i) for i, s in enumerate(nums))[k - 1][1] for k, trim in queries]
```
