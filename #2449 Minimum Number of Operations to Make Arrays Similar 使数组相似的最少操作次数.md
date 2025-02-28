# 2449 Minimum Number of Operations to Make Arrays Similar 使数组相似的最少操作次数

__Description:__

You are given two positive integer arrays `nums` and `target`, of the same length.

In one operation, you can choose any two __distinct__ indices `i` and `j` where `0 <= i, j < nums.length` and:

- set `nums[i] = nums[i] + 2` and
- set `nums[j] = nums[j] - 2`.

Two arrays are considered to be similar if the frequency of each element is the same.

Return _the minimum number of operations required to make_ `nums` _similar to_ `target`. The test cases are generated such that `nums` can always be similar to `target`.

__Example:__

Example 1:

```text
Input: nums = [8,12,6], target = [2,14,10]
Output: 2
Explanation: It is possible to make nums similar to target in two operations:
```

- Choose i = 0 and j = 2, nums = [10,12,4].
- Choose i = 1 and j = 2, nums = [10,14,2].

It can be shown that 2 is the minimum number of operations needed.

Example 2:

```text
Input: nums = [1,2,5], target = [4,1,3]
Output: 1
Explanation: We can make nums similar to target in one operation:
```

- Choose i = 1 and j = 2, nums = [1,4,3].

Example 3:

```text
Input: nums = [1,1,1,1,1], target = [1,1,1,1,1]
Output: 0
Explanation: The array nums is already similar to target.
```

__Constraints:__

- `n == nums.length == target.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i], target[i] <= 10 ^ 6`
- It is possible to make `nums` similar to `target`.

__题目描述:__

给你两个正整数数组 `nums` 和 `target` ，两个数组长度相等。

在一次操作中，你可以选择两个 __不同__ 的下标 `i` 和 `j` ，其中 `0 <= i, j < nums.length` ，并且:

- 令 `nums[i] = nums[i] + 2` 且
- 令 `nums[j] = nums[j] - 2` 。

如果两个数组中每个元素出现的频率相等，我们称两个数组是 相似 的。

请你返回将 `nums` 变得与 `target` 相似的最少操作次数。测试数据保证 `nums` 一定能变得与 `target` 相似。

__示例:__

示例 1：

```text
输入：nums = [8,12,6], target = [2,14,10]
输出：2
解释：可以用两步操作将 nums 变得与 target 相似：
```

- 选择 i = 0 和 j = 2 ，nums = [10,12,4] 。
- 选择 i = 1 和 j = 2 ，nums = [10,14,2] 。

2 次操作是最少需要的操作次数。

示例 2：

```text
输入：nums = [1,2,5], target = [4,1,3]
输出：1
解释：一步操作可以使 nums 变得与 target 相似：
```

- 选择 i = 1 和 j = 2 ，nums = [1,4,3] 。

示例 3：

```text
输入：nums = [1,1,1,1,1], target = [1,1,1,1,1]
输出：0
解释：数组 nums 已经与 target 相似。
```

__提示：__

- `n == nums.length == target.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i], target[i] <= 10 ^ 6`
- `nums` 一定可以变得与 `target` 相似。

__思路:__

```text
贪心
由于操作不影响奇偶性
所以可以将 nums 和 target 中的奇数和偶数分开
分别排序
这里可以将奇数变成相反数
排序之后计算对应位置的差值的绝对值的和
操作 1 次可以减少 4 的差值
最后将和除以 4 即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long makeSimilar(vector<int>& nums, vector<int>& target) 
    {
        helper(nums);
        helper(target);
        long long result = 0LL;
        for (int i = 0, n = nums.size(); i < n; i++) result += abs(target[i] - nums[i]);
        return result >> 2;
    }
private:
    void helper(vector<int>& arr) 
    {
        for (int i = 0, n = arr.size(); i < n; i++) if (arr[i] & 1) arr[i] = -arr[i];
        sort(arr.begin(), arr.end());
    }
};
```

__Java__:

```Java
class Solution {
    public long makeSimilar(int[] nums, int[] target) {
        helper(nums);
        helper(target);
        long result = 0L;
        for (int i = 0, n = nums.length; i < n; i++) result += Math.abs(target[i] - nums[i]);
        return result >> 2;
    }

    private void helper(int[] arr) {
        for (int i = 0, n = arr.length; i < n; i++) if ((arr[i] & 1) == 1) arr[i] = -arr[i];
        Arrays.sort(arr);
    }
}
```

__Python__:

```Python
class Solution:
    def makeSimilar(self, nums: List[int], target: List[int]) -> int:
        return sum(abs(x - y) for x, y in zip(sorted(nums, key=lambda x: -x if x & 1 else x), sorted(target, key=lambda x: -x if x & 1 else x))) >> 2
```
