# 1984 Minimum Difference Between Highest and Lowest of K Scores 学生分数的最小差值

__Description:__

You are given a __0-indexed__ integer array `nums`, where `nums[i]` represents the score of the `i ^ th` student. You are also given an integer `k`.

Pick the scores of any `k` students from the array so that the __difference__ between the __highest__ and the __lowest__ of the `k` scores is __minimized__.

Return the minimum possible difference.

__Example:__

Example 1:

```text
Input: nums = [90], k = 1
Output: 0
Explanation: There is one way to pick score(s) of one student:
```

- [90]. The difference between the highest and lowest score is 90 - 90 = 0.

The minimum possible difference is 0.

Example 2:

```text
Input: nums = [9,4,1,7], k = 2
Output: 2
Explanation: There are six ways to pick score(s) of two students:
```

- [9,4,1,7]. The difference between the highest and lowest score is 9 - 4 = 5.
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 1 = 8.
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 7 = 2.
- [9,4,1,7]. The difference between the highest and lowest score is 4 - 1 = 3.
- [9,4,1,7]. The difference between the highest and lowest score is 7 - 4 = 3.
- [9,4,1,7]. The difference between the highest and lowest score is 7 - 1 = 6.

The minimum possible difference is 2.

__Constraints:__

- `1 <= k <= nums.length <= 1000`
- `0 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个 __下标从 0 开始__ 的整数数组 `nums` ，其中 `nums[i]` 表示第 `i` 名学生的分数。另给你一个整数 `k` 。

从数组中选出任意 `k` 名学生的分数，使这 `k` 个分数间 __最高分__ 和 __最低分__ 的 __差值__ 达到 __最小化__ 。

返回可能的 最小差值 。

__示例:__

示例 1：

```text
输入：nums = [90], k = 1
输出：0
解释：选出 1 名学生的分数，仅有 1 种方法：
```

- [90] 最高分和最低分之间的差值是 90 - 90 = 0

可能的最小差值是 0

示例 2：

```text
输入：nums = [9,4,1,7], k = 2
输出：2
解释：选出 2 名学生的分数，有 6 种方法：
```

- [9,4,1,7] 最高分和最低分之间的差值是 9 - 4 = 5
- [9,4,1,7] 最高分和最低分之间的差值是 9 - 1 = 8
- [9,4,1,7] 最高分和最低分之间的差值是 9 - 7 = 2
- [9,4,1,7] 最高分和最低分之间的差值是 4 - 1 = 3
- [9,4,1,7] 最高分和最低分之间的差值是 7 - 4 = 3
- [9,4,1,7] 最高分和最低分之间的差值是 7 - 1 = 6

可能的最小差值是 2

__提示：__

- `1 <= k <= nums.length <= 1000`
- `0 <= nums[i] <= 10 ^ 5`

__思路:__

```text
排序 ➕ 滑动窗口
对数组进行排序
然后使用滑动窗口，遍历数组，每次计算窗口内的最大值和最小值的差值，取最小值
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 时间复杂度主要来自排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDifference(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), result = nums[k - 1] - nums.front();
        for (int i = k; i < n; i++) result = min(result, nums[i] - nums[i - k + 1]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, result = nums[k - 1] - nums[0];
        for (int i = k; i < n; i++) result = Math.min(result, nums[i] - nums[i - k + 1]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        nums.sort()
        result, n = nums[k - 1] - nums[0], len(nums)
        for i in range(k, n):
            result = min(result, nums[i] - nums[i - k + 1])
        return result
```
