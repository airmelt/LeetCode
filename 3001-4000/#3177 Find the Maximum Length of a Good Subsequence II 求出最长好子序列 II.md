# 3177 Find the Maximum Length of a Good Subsequence II 求出最长好子序列 II

__Description:__

You are given an integer array `nums` and a __non-negative__ integer `k`. A sequence of integers `seq` is called __good__ if there are __at most__ `k` indices `i` in the range `[0, seq.length - 2]` such that `seq[i] != seq[i + 1]`.

Return the __maximum__ possible length of a __good__ __subsequence__ of `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,2,1,1,3], k = 2

Output: 4

Explanation:

The maximum length subsequence is [1,2,1,1,3].
```

Example 2:

```text
Input: nums = [1,2,3,4,5,1], k = 0

Output: 2

Explanation:

The maximum length subsequence is [1,2,3,4,5,1].
```

__Constraints:__

- `1 <= nums.length <= 5 * 10 ^ 3`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= k <= min(50, nums.length)`

__题目描述:__

给你一个整数数组 `nums` 和一个 __非负__ 整数 `k` 。如果一个整数序列 `seq` 满足在范围下标范围 `[0, seq.length - 2]` 中存在 __不超过__ `k` 个下标 `i` 满足 `seq[i] != seq[i + 1]` ，那么我们称这个整数序列为 __好__ 序列。

请你返回 `nums` 中 __好__ __子序列__ 的最长长度

__示例:__

示例 1：

```text
输入：nums = [1,2,1,1,3], k = 2

输出：4

解释：

最长好子序列为 [1,2,1,1,3] 。
```

示例 2：

```text
输入：nums = [1,2,3,4,5,1], k = 0

输出：2

解释：

最长好子序列为 [1,2,3,4,5,1] 。
```

__提示：__

- `1 <= nums.length <= 5 * 10 ^ 3`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= k <= min(50, nums.length)`

__思路:__

```text
动态规划
设 dp[num][j] 表示以 num 结尾至多有 j 个相邻不同的数
初始化 dp[num][j] = 0
贪心的选择 num
要么 num 和之前的数字相同, dp[num][j] + 1 接在它的后面
要么 num 和之前的数字不同, 那么需要选择一个最大的 dp[num2][j - 1] + 1, 新增的最后一个一定和之前的 num2 不同
如果遍历 num2, 需要 10 ^ 9 的时间/空间
但是实际上只需要记录最大值 mx, 和次大值 mx2
一定从这两个值转移, 设最大的 dp[num2][j - 1] 为 m
则 dp[num][j] = max(dp[num][j], m) + 1
用一个数组 mx 来记录 j 个相邻不同的数的最大值
如果 num != num2, 可以直接使用 mx[j] + 1 来更新
如果 num == num2, 用 dp[num][j - 1] 来更新, 因为 dp[num][j] >= dp[num][j - 1]
而 mx[j + 1] 会使用 dp[num][j] 来更新, 所以是合理的
时间复杂度为 O(NK), 空间复杂度为 O(NK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumLength(vector<int>& nums, int k) 
    {
        unordered_map<int, vector<int>> fs;
        vector<int> mx(k + 2);
        for (const auto& num : nums)
        {
            auto& dp = fs[num];
            dp.resize(k + 1);
            for (int j = k; j > -1; j--)
            {
                dp[j] = max(dp[j], mx[j]) + 1;
                mx[j + 1] = max(mx[j + 1], dp[j]);
            }
        }    
        return mx.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumLength(int[] nums, int k) {
        var fs = new HashMap<Integer, int[]>();
        int[] mx = new int[k + 2];
        for (int num : nums) {
            int[] dp = fs.getOrDefault(num, new int[k + 1]);
            for (int j = k; j > -1; j--) {
                dp[j] = Math.max(dp[j], mx[j]) + 1;
                mx[j + 1] = Math.max(mx[j + 1], dp[j]);
            }
            fs.put(num, dp);
        }
        return mx[k + 1];
    }
}
```

__Python__:

```Python
class Solution:
    def maximumLength(self, nums: List[int], k: int) -> int:
        fs, mx = defaultdict(lambda: [0] * (k + 1)), [0] * (k + 2)
        for num in nums:
            dp = fs[num]
            for j in range(k, -1, -1):
                dp[j], mx[j + 1] = max(dp[j], mx[j]) + 1, max(mx[j + 1], max(dp[j], mx[j]) + 1)
        return mx[-1]
```
