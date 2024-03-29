# 1802 Maximum Value at a Given Index in a Bounded Array 有界数组中指定下标处的最大值

__Description:__

You are given three positive integers: `n`, `index`, and `maxSum`. You want to construct an array `nums` (__0-indexed__) that satisfies the following conditions:

- `nums.length == n`
- `nums[i]` is a __positive__ integer where `0 <= i < n`.
- `abs(nums[i] - nums[i+1]) <= 1` where `0 <= i < n-1`.
- The sum of all the elements of `nums` does not exceed `maxSum`.
- `nums[index]` is __maximized__.

Return `nums[index]` _of the constructed array_.

Note that `abs(x)` equals `x` if `x >= 0`, and `-x` otherwise.

__Example:__

Example 1:

```text
Input: n = 4, index = 2,  maxSum = 6
Output: 2
Explanation: nums = [1,2,2,1] is one array that satisfies all the conditions.
There are no arrays that satisfy all the conditions and have nums[2] == 3, so 2 is the maximum nums[2].
```

Example 2:

```text
Input: n = 6, index = 1,  maxSum = 10
Output: 3
```

__Constraints:__

- `1 <= n <= maxSum <= 10 ^ 9`
- `0 <= index < n`

__题目描述:__

给你三个正整数 `n`、 `index` 和 `maxSum` 。你需要构造一个同时满足下述所有条件的数组 `nums`（下标 __从 0 开始__ 计数）:

- `nums.length == n`
- `nums[i]` 是 __正整数__ ，其中 `0 <= i < n`
- `abs(nums[i] - nums[i+1]) <= 1` ，其中 `0 <= i < n-1`
- `nums` 中所有元素之和不超过 `maxSum`
- `nums[index]` 的值被 __最大化__

返回你所构造的数组中的 `nums[index]` 。

注意: `abs(x)` 等于 `x` 的前提是 `x >= 0` ；否则， `abs(x)` 等于 `-x` 。

__示例:__

示例 1：

```text
输入：n = 4, index = 2,  maxSum = 6
输出：2
解释：数组 [1,1,2,1] 和 [1,2,2,1] 满足所有条件。不存在其他在指定下标处具有更大值的有效数组。
```

示例 2：

```text
输入：n = 6, index = 1,  maxSum = 10
输出：3
```

__提示：__

- `1 <= n <= maxSum <= 10 ^ 9`
- `0 <= index < n`

__思路:__

```text
1. 贪心 ➕ 二分查找
令 index 对应的元素为山峰
两边依次递减 1 直到边界或者减到 1
利用等差数列求和公式求出两边的和
用二分查找得到最大值
时间复杂度为 O(logM), 空间复杂度为 O(1), M 为 maxSum
2. 数学
通过解一元二次方程得到上限
比较其与 maxSum 的大小得到结果
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxValue(int n, int index, int maxSum) 
    {
        int left = index, right = n - 1 - index, l = 1, r = maxSum;
        while (l <= r)
        {
            int mid = ((l + r) >> 1);
            if (check(mid, left, right, maxSum)) r = mid - 1;
            else l = mid + 1; 
        }
        return l - 1;
    }
private:
    bool check(int mid, int left, int right, int maxSum)
    {
        return (long)mid + (mid > left ? ((long)left * ((mid << 1) - 1 - left) >> 1) : ((((long)mid - 3) * mid >> 1) + left + 1)) + (mid > right ? ((long)right * ((mid << 1) - 1 - right) >> 1) : ((((long)mid - 3) * mid >> 1) + right + 1)) > (long)maxSum;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxValue(int n, int index, int maxSum) {
        int left = index, right = n - 1 - index, l = 1, r = maxSum;
        while (l <= r) {
            int mid = ((l + r) >> 1);
            if (check(mid, left, right, maxSum)) r = mid - 1;
            else l = mid + 1; 
        }
        return l - 1;
    }

    private boolean check(int mid, int left, int right, int maxSum)
    {
        return (long)mid + (mid > left ? ((long)left * ((mid << 1) - 1 - left) >> 1) : ((((long)mid - 3) * mid >> 1) + left + 1)) + (mid > right ? ((long)right * ((mid << 1) - 1 - right) >> 1) : ((((long)mid - 3) * mid >> 1) + right + 1)) > (long)maxSum;
    }
}
```

__Python__:

```Python
class Solution:
    def maxValue(self, n: int, index: int, maxSum: int) -> int:
        return floor(((2 + (4 - 4 * (left + right + 2 - maxSum)) ** 0.5) / 2)) if ((((left := min(index, n - index - 1)) + 1) ** 2 - 3 * (left + 1)) // 2 + left + 1 + (left + 1) + ((left + 1) ** 2 - 3 * (left + 1)) // 2 + (right := max(index, n - index - 1)) + 1) >= maxSum else floor(((0.5 - left + ((0.5 - left) ** 2 - 2 * (right + 1 + (-left - 1) * left / 2 - maxSum)) ** 0.5))) if ((2 * (right + 1) - left - 1) * left // 2 + (right + 1) + ((right + 1) ** 2 - 3 * (right + 1)) // 2 + right + 1) >= maxSum else floor(-((-left ** 2 - left - right ** 2 - right) / 2 - maxSum) / (left + right + 1))
```
