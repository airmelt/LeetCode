# 1589 Maximum Sum Obtained of Any Permutation 所有排列中的最大和

__Description:__

We have an array of integers, `nums`, and an array of `requests` where `requests[i] = [starti, endi]`. The `i ^ th` request asks for the sum of `nums[starti] + nums[starti + 1] + ... + nums[endi - 1] + nums[endi]`. Both `starti` and `endi` are _0-indexed_.

Return _the maximum total sum of all requests __among all permutations__ of_ `nums`.

Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5], requests = [[1,3],[0,1]]
Output: 19
Explanation: One permutation of nums is [2,1,3,4,5] with the following result: 
requests[0] -> nums[1] + nums[2] + nums[3] = 1 + 3 + 4 = 8
requests[1] -> nums[0] + nums[1] = 2 + 1 = 3
Total sum: 8 + 3 = 11.
A permutation with a higher total sum is [3,5,4,2,1] with the following result:
requests[0] -> nums[1] + nums[2] + nums[3] = 5 + 4 + 2 = 11
requests[1] -> nums[0] + nums[1] = 3 + 5  = 8
Total sum: 11 + 8 = 19, which is the best that you can do.
```

Example 2:

```text
Input: nums = [1,2,3,4,5,6], requests = [[0,1]]
Output: 11
Explanation: A permutation with the max total sum is [6,5,4,3,2,1] with request sums [11].
```

Example 3:

```text
Input: nums = [1,2,3,4,5,10], requests = [[0,2],[1,3],[1,1]]
Output: 47
Explanation: A permutation with the max total sum is [4,10,5,3,2,1] with request sums [19,18,10].
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`
- `1 <= requests.length <= 10 ^ 5`
- `requests[i].length == 2`
- `0 <= starti <= endi < n`

__题目描述:__

有一个整数数组 `nums` ，和一个查询数组 `requests` ，其中 `requests[i] = [starti, endi]` 。第 `i` 个查询求 `nums[starti] + nums[starti + 1] + ... + nums[endi - 1] + nums[endi]` 的结果 ， `starti` 和 `endi` 数组索引都是 __从 0 开始__ 的。

你可以任意排列 `nums` 中的数字，请你返回所有查询结果之和的最大值。

由于答案可能会很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5], requests = [[1,3],[0,1]]
输出：19
解释：一个可行的 nums 排列为 [2,1,3,4,5]，并有如下结果：
requests[0] -> nums[1] + nums[2] + nums[3] = 1 + 3 + 4 = 8
requests[1] -> nums[0] + nums[1] = 2 + 1 = 3
总和为：8 + 3 = 11。
一个总和更大的排列为 [3,5,4,2,1]，并有如下结果：
requests[0] -> nums[1] + nums[2] + nums[3] = 5 + 4 + 2 = 11
requests[1] -> nums[0] + nums[1] = 3 + 5  = 8
总和为： 11 + 8 = 19，这个方案是所有排列中查询之和最大的结果。
```

示例 2：

```text
输入：nums = [1,2,3,4,5,6], requests = [[0,1]]
输出：11
解释：一个总和最大的排列为 [6,5,4,3,2,1] ，查询和为 [11]。
```

示例 3：

```text
输入：nums = [1,2,3,4,5,10], requests = [[0,2],[1,3],[1,1]]
输出：47
解释：一个和最大的排列为 [4,10,5,3,2,1] ，查询结果分别为 [19,18,10]。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`
- `1 <= requests.length <= 10 ^ 5`
- `requests[i].length == 2`
- `0 <= starti <= endi < n`

__思路:__

```text
贪心 ➕ 前缀和 ➕ 差分数组
为了使所有查询结果之和最大
我们需要将计算次数最多的下标分配给最大的数, 其余以此类推
可以使用差分数组记录所有区间, 区间左端点自增, 区间右端点自减
求出差分数组的前缀和 pre
将 pre 和 nums 排序, 那么计算次数和 nums 中的数就会一一对应
累加和为对应两个数的乘积的和
时间复杂度为 O(NlogN + M), 空间复杂度为 O(N), 其中 N 为数组 nums 的长度, M 为数组 requests 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSumRangeQuery(vector<int>& nums, vector<vector<int>>& requests) 
    {
        int n = nums.size();
        vector<int> diff(n + 1), pre(n + 1);
        long result = 0, MOD = 1e9 + 7;
        for (const auto& request: requests) 
        {
            ++diff[request.front()];
            --diff[request.back() + 1];
        }
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + diff[i];
        sort(pre.begin(), pre.end());
        sort(nums.begin(), nums.end());
        for (int i = 0; i < n; i++) result = (result + (long)nums[i] * pre[i + 1]) % MOD;
        return (int)result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSumRangeQuery(int[] nums, int[][] requests) {
        int n = nums.length, diff[] = new int[n + 1], pre[] = new int[n + 1];
        long result = 0, MOD = 1_000_000_007;
        for (int[] request: requests) {
            ++diff[request[0]];
            --diff[request[1] + 1];
        }
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + diff[i];
        Arrays.sort(pre);
        Arrays.sort(nums);
        for (int i = 0; i < n; i++) result = (result + (long)nums[i] * pre[i + 1]) % MOD;
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSumRangeQuery(self, nums: List[int], requests: List[List[int]]) -> int:
        diff, MOD = [0] * ((n := len(nums)) + 1), 10 ** 9 + 7
        for l, r in requests:
            diff[l] += 1
            diff[r + 1] -= 1
        pre, result = list(accumulate(diff)), 0
        nums.sort()
        pre.sort()
        for i in range(n):
            result += nums[i] * pre[i + 1] 
        return result % MOD
```
