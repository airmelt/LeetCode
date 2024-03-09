# 2025 Maximum Number of Ways to Partition an Array 分割数组的最多方案数

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n`. The number of ways to __partition__ `nums` is the number of `pivot` indices that satisfy both conditions:

- `1 <= pivot < n`
- `nums[0] + nums[1] + ... + nums[pivot - 1] == nums[pivot] + nums[pivot + 1] + ... + nums[n - 1]`

You are also given an integer `k`. You can choose to change the value of __one__ element of `nums` to `k`, or to leave the array __unchanged__.

Return _the __maximum__ possible number of ways to __partition___ `nums` _to satisfy both conditions after changing __at most__ one element_.

__Example:__

Example 1:

```text
Input: nums = [2,-1,2], k = 3
Output: 1
Explanation: One optimal approach is to change nums[0] to k. The array becomes [3,-1,2].
There is one way to partition the array:
- For pivot = 2, we have the partition [3,-1 | 2]: 3 + -1 == 2.
```

Example 2:

```text
Input: nums = [0,0,0], k = 1
Output: 2
Explanation: The optimal approach is to leave the array unchanged.
There are two ways to partition the array:
- For pivot = 1, we have the partition [0 | 0,0]: 0 == 0 + 0.
- For pivot = 2, we have the partition [0,0 | 0]: 0 + 0 == 0.
```

Example 3:

```text
Input: nums = [22,4,-25,-20,-15,15,-16,7,19,-10,0,-13,-14], k = -33
Output: 4
Explanation: One optimal approach is to change nums[2] to k. The array becomes [22,4,-33,-20,-15,15,-16,7,19,-10,0,-13,-14].
There are four ways to partition the array.
```

__Constraints:__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 5 <= k, nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始且长度为 `n` 的整数数组 `nums` 。__分割__ 数组 `nums` 的方案数定义为符合以下两个条件的 `pivot` 数目:

- `1 <= pivot < n`
- `nums[0] + nums[1] + ... + nums[pivot - 1] == nums[pivot] + nums[pivot + 1] + ... + nums[n - 1]`

同时给你一个整数 `k` 。你可以将 `nums` 中 __一个__ 元素变为 `k` 或 __不改变__ 数组。

请你返回在 __至多__ 改变一个元素的前提下，__最多__ 有多少种方法 __分割__ `nums` 使得上述两个条件都满足。

__示例:__

示例 1：

```text
输入：nums = [2,-1,2], k = 3
输出：1
解释：一个最优的方案是将 nums[0] 改为 k 。数组变为 [3,-1,2] 。
有一种方法分割数组：
- pivot = 2 ，我们有分割 [3,-1 | 2]：3 + -1 == 2 。
```

示例 2：

```text
输入：nums = [0,0,0], k = 1
输出：2
解释：一个最优的方案是不改动数组。
有两种方法分割数组：
- pivot = 1 ，我们有分割 [0 | 0,0]：0 == 0 + 0 。
- pivot = 2 ，我们有分割 [0,0 | 0]: 0 + 0 == 0 。
```

示例 3：

```text
输入：nums = [22,4,-25,-20,-15,15,-16,7,19,-10,0,-13,-14], k = -33
输出：4
解释：一个最优的方案是将 nums[2] 改为 k 。数组变为 [22,4,-33,-20,-15,15,-16,7,19,-10,0,-13,-14] 。
有四种方法分割数组。
```

__提示：__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 5 <= k, nums[i] <= 10 ^ 5`

__思路:__

```text
前缀和 ➕ 哈希表
题意实际上就是在前缀和中查找 total >> 1 的次数
使用哈希表 left 和 right 分别记录左右两侧的前缀和出现次数
先计算前缀和数组 pre, 同时记录右侧前缀和出现次数
数组的和 total 为 pre 的最后一个元素
在 total 为偶数的情况下, 记录右侧前缀和出现次数为 total >> 1 的次数, 即为不改变数组的情况下的最大分割方案数
遍历数组, 计算改变元素后的最大分割方案数
如果 total + nums[i] - k 为奇数, 则不可能分割, 不更新结果
如果将 nums[i] 改为 k 后, 对于 nums[i] 的左侧统计 left[(total - nums[i] + k) >> 1], 对于 nums[i] 的右侧统计 right[(total + nums[i] - k) >> 1], 即前缀和的变化量, 结果更新为 left + right 的最大值
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int waysToPartition(vector<int>& nums, int k) 
    {
        int n = nums.size();
        vector<long long> pre(n, nums.front());
        unordered_map<long long, long long> left, right;
        for (int i = 1; i < n; i++) 
        {
            pre[i] = pre[i - 1] + nums[i];
            ++right[pre[i - 1]];
        }
        long long result = 0LL, total = pre.back();
        if (!(total & 1)) result = right[total >> 1];
        for (int i = 0; i < n; i++) 
        {
            if (!((total + nums[i] - k) & 1)) result = max(result, left[(total - nums[i] + k) >> 1] + right[(total + nums[i] - k) >> 1]);
            ++left[pre[i]];
            --right[pre[i]];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int waysToPartition(int[] nums, int k) {
        int n = nums.length, pre[] = new int[n + 1], result = 0, total = 0;
        Map<Integer, Integer> left = new HashMap<>(), right = new HashMap<>();
        for (int i = 0; i < n; i++) {
            pre[i + 1] = pre[i] + nums[i];
            if (i > 0) right.merge(pre[i], 1, Integer::sum);
        }
        total = pre[n];
        if ((total & 1) == 0) result = right.getOrDefault(total >> 1, 0);
        for (int i = 0; i < n; i++) {
            if (((total + nums[i] - k) & 1) == 0) result = Math.max(result, left.getOrDefault((total - nums[i] + k) >> 1, 0) + right.getOrDefault((total + nums[i] - k) >> 1, 0));
            left.merge(pre[i + 1], 1, Integer::sum);
            right.merge(pre[i + 1], -1, Integer::sum);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def waysToPartition(self, nums: List[int], k: int) -> int:
        n, pre = len(nums), list(accumulate(nums))
        result, left = (right := Counter(pre[:-1]))[(total := pre[-1]) >> 1], defaultdict(int)
        for i in range(n):
            if not (total + nums[i] - k) & 1:
                result = max(result, left[(total - nums[i] + k) >> 1] + right[(total + nums[i] - k) >> 1])
            left[pre[i]] += 1
            right[pre[i]] -= 1
        return result
```
