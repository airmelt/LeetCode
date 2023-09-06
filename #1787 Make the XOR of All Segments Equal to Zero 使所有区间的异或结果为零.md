# 1787 Make the XOR of All Segments Equal to Zero 使所有区间的异或结果为零

__Description:__

You are given an array `nums`​​​ and an integer `k`​​​​​. The XOR of a segment `[left, right]` where `left <= right` is the `XOR` of all the elements with indices between `left` and `right`, inclusive: `nums[left] XOR nums[left+1] XOR ... XOR nums[right]`.

Return _the minimum number of elements to change in the array_ such that the `XOR` of all segments of size `k`​​​​​​ is equal to zero.

__Example:__

Example 1:

```text
Input: nums = [1,2,0,3,0], k = 1
Output: 3
Explanation: Modify the array from [1,2,0,3,0] to from [0,0,0,0,0].
```

Example 2:

```text
Input: nums = [3,4,5,2,1,7,3,4,7], k = 3
Output: 3
Explanation: Modify the array from [3,4,5,2,1,7,3,4,7] to [3,4,7,3,4,7,3,4,7].
```

Example 3:

```text
Input: nums = [1,2,4,1,2,5,1,2,6], k = 3
Output: 3
Explanation: Modify the array from [1,2,4,1,2,5,1,2,6] to [1,2,3,1,2,3,1,2,3].
```

__Constraints:__

- `1 <= k <= nums.length <= 2000`
- `​​​​​​0 <= nums[i] < 2 ^ 10`

__题目描述:__

给你一个整数数组 `nums`​​​ 和一个整数 `k`​​​​​ 。区间 `[left, right]`（ `left <= right`）的 __异或结果__ 是对下标位于 `left` 和 `right`（包括 `left` 和 `right` ）之间所有元素进行 `XOR` 运算的结果: `nums[left] XOR nums[left+1] XOR ... XOR nums[right]` 。

返回数组中 __要更改的最小元素数__ ，以使所有长度为 `k` 的区间异或结果等于零。

__示例:__

示例 1：

```text
输入：nums = [1,2,0,3,0], k = 1
输出：3
解释：将数组 [1,2,0,3,0] 修改为 [0,0,0,0,0]
```

示例 2：

```text
输入：nums = [3,4,5,2,1,7,3,4,7], k = 3
输出：3
解释：将数组 [3,4,5,2,1,7,3,4,7] 修改为 [3,4,7,3,4,7,3,4,7]
```

示例 3：

```text
输入：nums = [1,2,4,1,2,5,1,2,6], k = 3
输出：3
解释：将数组[1,2,4,1,2,5,1,2,6] 修改为 [1,2,3,1,2,3,1,2,3]
```

__提示：__

- `1 <= k <= nums.length <= 2000`
- `​​​​​​0 <= nums[i] < 2 ^ 10`

__思路:__

```text
动态规划
由于 k 个数的异或结果为 0
即 nums[i] ^ nums[i + 1] ^ ... ^ nums[i + k - 1] = 0
且 nums[i + 1] ^ nums[i + 2] ^ ... ^ nums[i + k] = 0
两式合并, 有 nums[i] ^ nums[i + k] = 0, 即 nums[i] = nums[i + k]
所以这个数组最后是一个以 k 为周期的数组
设 dp[val] 表示进行 i 次循环后, 组内的异或值为 val 的最小修改次数
初始化 dp[0] = 0, dp[val] = INF, val != 0
统计每个组内的下标的个数, 以及每个组内的元素的出现次数
对于第 i 次循环的 cur 数组初始化为 min(dp) + size[i], 因为总可以把组内的所有元素全部替换为 min(dp) 对应的元素值
另外一种情况是选择一个曾经出现过的元素
cur[val] = min(cur[val], dp[val ^ k] + size[i] - groups[i][val])
完成 k 次循环之后返回 dp[0] 即为答案
时间复杂度为 O(M(N + K)), 空间复杂度为 O(N + M), 其中 M 为 nums 数组值的范围, 即 1 << 10
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minChanges(vector<int>& nums, int k) 
    {
        int n = nums.size(), m = (1 << 10), INF = 0x3f3f3f3f;
        vector<unordered_map<int, int>> groups(k);
        vector<int> size(k), dp(m, INF);
        dp.front() = 0;
        for (int i = 0; i < n; i++)
        {
            ++groups[i % k][nums[i]];
            ++size[i % k];
        }
        for (int i = 0; i < k; i++)
        {
            vector<int> cur(m, *min_element(dp.begin(), dp.end()) + size[i]);
            for (int j = 0; j < m; j++) if (dp[j] != INF) for (const auto& [k, v] : groups[i]) cur[k ^ j] = min(cur[k ^ j], dp[j] + size[i] - v);
            dp = move(cur);
        }
        return dp.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int minChanges(int[] nums, int k) {
        int n = nums.length, m = (1 << 10), dp[] = new int[m], size[] = new int[k], INF = 0x3f3f3f3f;
        Arrays.fill(dp, INF);
        dp[0] = 0;
        List<Map<Integer, Integer>> groups = new ArrayList<>();
        for (int i = 0; i < n; i++) groups.add(new HashMap<>());
        for (int i = 0; i < n; i++) {
            groups.get(i % k).put(nums[i], groups.get(i % k).getOrDefault(nums[i], 0) + 1);
            ++size[i % k];
        }
        for (int i = 0; i < k; i++) {
            int cur[] = new int[m], minValue = INF;
            for (int j = 0; j < m; j++) minValue = Math.min(minValue, dp[j]);
            Arrays.fill(cur, minValue + size[i]);
            for (int j = 0; j < m; j++) if (dp[j] != INF) for (Map.Entry<Integer, Integer> entry : groups.get(i).entrySet()) cur[entry.getKey() ^ j] = Math.min(cur[entry.getKey() ^ j], dp[j] + size[i] - entry.getValue());
            dp = cur;
        }
        return dp[0];
    }
}
```

__Python__:

```Python
class Solution:
    def minChanges(self, nums: List[int], k: int) -> int:
        n, groups, size, dp = len(nums), [defaultdict(int) for _ in range(k)], [0] * k, [0] + [inf] * ((1 << 10) - 1)
        for i in range(n):
            groups[i % k][nums[i]] += 1
            size[i % k] += 1
        for i in range(k):
            cur = [min(dp) + size[i]] * (1 << 10)
            for j in range(1 << 10):
                if dp[j] != inf:
                    for k, v in groups[i].items():
                        cur[k ^ j] = min(cur[k ^ j], dp[j] + size[i] - v)
            dp = cur
        return dp[0]
```
