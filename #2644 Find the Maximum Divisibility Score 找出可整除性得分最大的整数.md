# 2644 Find the Maximum Divisibility Score 找出可整除性得分最大的整数

__Description:__

You are given two integer arrays `nums` and `divisors`.

The __divisibility score__ of `divisors[i]` is the number of indices `j` such that `nums[j]` is divisible by `divisors[i]`.

Return the integer `divisors[i]` with the __maximum__ divisibility score. If multiple integers have the maximum score, return the smallest one.

__Example:__

Example 1:

```text
Input: nums = [2,9,15,50], divisors = [5,3,7,2]

Output: 2

Explanation:

The divisibility score of divisors[0] is 2 since nums[2] and nums[3] are divisible by 5.

The divisibility score of divisors[1] is 2 since nums[1] and nums[2] are divisible by 3.

The divisibility score of divisors[2] is 0 since none of the numbers in nums is divisible by 7.

The divisibility score of divisors[3] is 2 since nums[0] and nums[3] are divisible by 2.

As divisors[0], divisors[1], and divisors[3] have the same divisibility score, we return the smaller one which is divisors[3].
```

Example 2:

```text
Input: nums = [4,7,9,3,9], divisors = [5,2,3]

Output: 3

Explanation:

The divisibility score of divisors[0] is 0 since none of numbers in nums is divisible by 5.

The divisibility score of divisors[1] is 1 since only nums[0] is divisible by 2.

The divisibility score of divisors[2] is 3 since nums[2], nums[3] and nums[4] are divisible by 3.
```

Example 3:

```text
Input: nums = [20,14,21,10], divisors = [10,16,20]

Output: 10

Explanation:

The divisibility score of divisors[0] is 2 since nums[0] and nums[3] are divisible by 10.

The divisibility score of divisors[1] is 0 since none of the numbers in nums is divisible by 16.

The divisibility score of divisors[2] is 1 since nums[0] is divisible by 20.
```

__Constraints:__

- `1 <= nums.length, divisors.length <= 1000`
- `1 <= nums[i], divisors[i] <= 10 ^ 9`

__题目描述:__

给你两个整数数组 `nums` 和 `divisors` 。

`divisors[i]` 的 __可整除性得分__ 等于满足 `nums[j]` 能被 `divisors[i]` 整除的下标 `j` 的数量。

返回 __可整除性得分__ 最大的整数 `divisors[i]` 。如果有多个整数具有最大得分，则返回数值最小的一个。

__示例:__

示例 1：

```text
输入：nums = [2,9,15,50], divisors = [5,3,7,2]

输出：2

解释：

divisors[0] 的可整除性分数为 2 因为 nums[2] 和 nums[3] 能被 5 整除。

divisors[1] 的可整除性分数为 2 因为 nums[1] 和 nums[2] 能被 3 整除。

divisors[2] 的可整除性分数为 0 因为 nums 中没有数字能被 7 整除。

divisors[3] 的可整除性分数为 2 因为 nums[0] 和 nums[3] 能够被 2 整除。

因为 divisors[0] 、divisor[1] 和 divisors[3] 有相同的可整除性分数，我们返回更小的那个 divisors[3]。
```

示例 2：

```text
输入：nums = [4,7,9,3,9], divisors = [5,2,3]

输出：3

解释：

divisors[0] 的可整除性分数为 0 因为 nums 中没有数字能被 5 整除。

divisors[1] 的可整除性分数为 1 因为只有 nums[0] 能被 2 整除。

divisors[2] 的可整除性分数为 3 因为 nums[2] ，nums[3] 和 nums[4] 能被 3 整除。
```

示例 3：

```text
输入：nums = [20,14,21,10], divisors = [10,16,20]

输出：10

解释：

divisors[0] 的可整除性分数为 2 因为 nums[0] 和 nums[3] 能被 10 整除。

divisors[1] 的可整除性分数为 0 因为 nums 中没有数字能被 16 整除。

divisors[2] 的可整除性分数为 1 因为 nums[0] 能被 20 整除。

因为 divisors[0] 的可整除性分数最大，我们返回 divisors[0]。
```

__提示：__

- `1 <= nums.length, divisors.length <= 1000`
- `1 <= nums[i], divisors[i] <= 10 ^ 9`

__思路:__

```text
模拟
对每一个 divisor 中的 d
计算其可整除性分数
遍历 nums 求 x % d == 0 的个数
如果小于当前值或者等于当前值但比当前的 divisor 小
就更新结果
可以对 nums 进行排序提前退出循环
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxDivScore(vector<int>& nums, vector<int>& divisors) 
    {
        sort(nums.begin(), nums.end(), greater<int>());
        int mx = -1, result = 0;
        for (const auto& d : divisors) 
        {
            int cur = 0;
            for (const auto& x : nums)
            {
                if (x < d) break;
                cur += (!(x % d));
            }
            if (cur > mx or cur == mx and d < result) 
            {
                result = d;
                mx = cur;
            }
        }
        return result;    
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDivScore(int[] nums, int[] divisors) {
        int result = 0, mx = -1, cur = 0;
        for (int d : divisors) {
            if ((cur = (int)Arrays.stream(nums).filter(x -> x % d == 0).count()) > mx || cur == mx && d < result) {
                result = d;
                mx = cur;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDivScore(self, nums: List[int], divisors: List[int]) -> int:
        return min(divisors) if not (c := max(sum(1 for x in nums if not x % d) for d in divisors)) else min(d for d in divisors if sum(1 for x in nums if not x % d) == c)
```
