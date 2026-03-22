# 1712 Ways to Split Array Into Three Subarrays 将数组分成三个子数组的方案数

__Description:__

A split of an integer array is good if:

- The array is split into three __non-empty__ contiguous subarrays - named `left`, `mid`, `right` respectively from left to right.
- The sum of the elements in `left` is less than or equal to the sum of the elements in `mid`, and the sum of the elements in `mid` is less than or equal to the sum of the elements in `right`.

Given `nums`, an array of __non-negative__ integers, return _the number of __good__ ways to split_ `nums`. As the number may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [1,1,1]
Output: 1
Explanation: The only good way to split nums is [1] [1] [1].
```

Example 2:

```text
Input: nums = [1,2,2,2,5,0]
Output: 3
Explanation: There are three good ways of splitting nums:
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]
```

Example 3:

```text
Input: nums = [3,2,1]
Output: 0
Explanation: There is no good way to split nums.
```

__Constraints:__

- `3 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 4`

__题目描述:__

我们称一个分割整数数组的方案是 好的 ，当它满足：

- 数组被分成三个 __非空__ 连续子数组，从左至右分别命名为 `left` ， `mid` ， `right` 。
- `left` 中元素和小于等于 `mid` 中元素和， `mid` 中元素和小于等于 `right` 中元素和。

给你一个 __非负__ 整数数组 `nums` ，请你返回 __好的__ 分割 `nums` 方案数目。由于答案可能会很大，请你将结果对 `10 ^ 9 + 7` 取余后返回。

__示例:__

示例 1：

```text
输入：nums = [1,1,1]
输出：1
解释：唯一一种好的分割方案是将 nums 分成 [1] [1] [1] 。
```

示例 2：

```text
输入：nums = [1,2,2,2,5,0]
输出：3
解释：nums 总共有 3 种好的分割方案：
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]
```

示例 3：

```text
输入：nums = [3,2,1]
输出：0
解释：没有好的分割方案。
```

__提示：__

- `3 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 4`

__思路:__

```text
前缀和 ➕ 二分
求出数组的前缀和, 因为数组中元素非负, 数组的前缀和一定是有序的
用二分法分别找到以 i 为 left 数组的两个端点加入结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int waysToSplit(vector<int>& nums) 
    {
        int n = nums.size(), MOD = 1e9 + 7, result = 0;
        vector<int> pre(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (int i = 1, left, right; i < n - 1; i++) if ((right = upper_bound(pre.begin() + i + 1, pre.begin() + n, (pre[i] + pre[n]) / 2) - pre.begin() - 1) >= (left = lower_bound(pre.begin() + i + 1, pre.begin() + n, 2 * pre[i]) - pre.begin())) result = (result + right - left + 1) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int waysToSplit(int[] nums) {
        int n = nums.length, result = 0, m = 2, pre[] = new int[n + 1], MOD = 1_000_000_007;
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (int i = 1, left = pre[i]; i < n - 1; left = pre[++i]) {
            m = Math.max(m, i + 1);
            while (m < n && pre[m] < (left << 1)) ++m;
            if (m == n) break;
            int low = m, high = n - 1;
            while (low <= high) {
                int mid = low + ((high - low) >> 1);
                if (pre[n] - pre[mid] < pre[mid] - left) high = mid - 1;
                else low = mid + 1;
            }
            result = (result + high - m + 1) % MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def waysToSplit(self, nums: List[int]) -> int:
        pre, result, m, n, MOD = [0] + list(accumulate(nums)), 0, 2, len(nums), 10 ** 9 + 7
        for i in range(1, n - 1):
            m, left = max(m, i + 1), pre[i]
            while m < n and pre[m] - left < left:
                m += 1
            if m == n:
                break
            low, high = m, n - 1
            while low <= high:
                mid = low + ((high - low) >> 1)
                if pre[n] - pre[mid] < pre[mid] - left:
                    high = mid - 1
                else:
                    low = mid + 1
            result += high - m + 1
        return result % (10 ** 9 + 7)
```
