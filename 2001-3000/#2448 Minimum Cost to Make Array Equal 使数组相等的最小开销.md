# 2448 Minimum Cost to Make Array Equal 使数组相等的最小开销

__Description:__

You are given two __0-indexed__ arrays `nums` and `cost` consisting each of `n` __positive__ integers.

You can do the following operation any number of times:

- Increase or decrease __any__ element of the array `nums` by `1`.

The cost of doing one operation on the `i ^ th` element is `cost[i]`.

Return _the __minimum__ total cost such that all the elements of the array_ `nums` _become __equal___.

__Example:__

Example 1:

```text
Input: nums = [1,3,5,2], cost = [2,3,1,14]
Output: 8
Explanation: We can make all the elements equal to 2 in the following way:
```

- Increase the 0th element one time. The cost is 2.
- Decrease the 1st element one time. The cost is 3.
- Decrease the 2nd element three times. The cost is 1 + 1 + 1 = 3.

The total cost is 2 + 3 + 3 = 8.

It can be shown that we cannot make the array equal with a smaller cost.

Example 2:

```text
Input: nums = [2,2,2,2,2], cost = [4,2,8,1,3]
Output: 0
Explanation: All the elements are already equal, so no operations are needed.
```

__Constraints:__

- `n == nums.length == cost.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i], cost[i] <= 10 ^ 6`
- Test cases are generated in a way that the output doesn't exceed 2 ^ 53-1

__题目描述:__

给你两个下标从 __0__ 开始的数组 `nums` 和 `cost` ，分别包含 `n` 个 __正__ 整数。

你可以执行下面操作 任意 次：

- 将 `nums` 中 __任意__ 元素增加或者减小 `1` 。

对第 `i` 个元素执行一次操作的开销是 `cost[i]` 。

请你返回使 `nums` 中所有元素 __相等__ 的 __最少__ 总开销。

__示例:__

示例 1：

```text
输入：nums = [1,3,5,2], cost = [2,3,1,14]
输出：8
解释：我们可以执行以下操作使所有元素变为 2 ：
```

- 增加第 0 个元素 1 次，开销为 2 。
- 减小第 1 个元素 1 次，开销为 3 。
- 减小第 2 个元素 3 次，开销为 1 + 1 + 1 = 3 。

总开销为 2 + 3 + 3 = 8 。

这是最小开销。

示例 2：

```text
输入：nums = [2,2,2,2,2], cost = [4,2,8,1,3]
输出：0
解释：数组中所有元素已经全部相等，不需要执行额外的操作。
```

__提示：__

- `n == nums.length == cost.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i], cost[i] <= 10 ^ 6`
- 测试用例确保输出不超过 2 ^ 53-1。

__思路:__

```text
1. 枚举
将 nums 和 cost 组成一个 pair 数组
按照 nums 的大小排序
假设按照减到 nums[0] 的大小来计算
total = sum(abs(nums[i] - nums[0]) * cost[i])
设 s = sum(cost)
如果要修改成 nums[1]
对于 cost[0] 来说需要增加 nums[1] - nums[0]
对于其他的 cost[i], 即 s - cost[0] 来说需要减少 nums[1] - nums[0]
总的来说 total -= (s - (cost[0] << 1)) * (nums[1] - nums[0])
取最小的 total 即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 中位数贪心
把 cost 看成是每个数出现的次数
找到第一个 x 使得 sum(cost[:x]) >= sum(cost) / 2
把所有的数都变化成 nums[x]
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minCost(vector<int>& nums, vector<int>& cost) 
    {
        long long mid = (accumulate(cost.begin(), cost.end(), 0LL) + 1LL) >> 1LL, s = 0LL, x = 0LL, result = 0LL;
        vector<pair<long long, long long>> arr;
        for (int i = 0, n = nums.size(); i < n; i++) arr.emplace_back(make_pair(nums[i], cost[i]));
        sort(arr.begin(), arr.end());
        for (int i = 0, n = nums.size(); i < n and !x; i++) if ((s = s + arr[i].second) >= mid) x = arr[i].first;
        for (const auto& p : arr) result += abs(x - p.first) * p.second;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minCost(int[] nums, int[] cost) {
        int n = nums.length, arr[][] = new int[n][2];
        for (int i = 0; i < n; i++) arr[i][0] = nums[i];
        for (int i = 0; i < n; i++) arr[i][1] = cost[i];
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        long total = 0L, s = Arrays.stream(cost).mapToLong(a -> a).sum();
        for (int i = 1; i < n; i++) total += (long)(arr[i][0] - arr[0][0]) * arr[i][1];
        long result = total;
        for (int i = 1; i < n; i++) result = Math.min(result, total = total - (s = s - (arr[i - 1][1] << 1)) * (arr[i][0] - arr[i - 1][0]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, nums: List[int], cost: List[int]) -> int:
        return sum(abs(x - s) * cost[i] for i, x in enumerate(nums)) if (s := bisect_left(range(10 ** 6), 0, key = lambda y: sum(cost[i] if y >= x else -cost[i] for i, x in enumerate(nums)))) else 0
```
