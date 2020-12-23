# 410 Split Array Largest Sum 分割数组的最大值

__Description__:
Given an array nums which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays.

Write an algorithm to minimize the largest sum among these m subarrays.

__Example:__
Example 1:

Input: nums = [7,2,5,10,8], m = 2
Output: 18
Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.

Example 2:

Input: nums = [1,2,3,4,5], m = 2
Output: 9

Example 3:

Input: nums = [1,4,4], m = 3
Output: 4

__Constraints:__

1 <= nums.length <= 1000
0 <= nums[i] <= 10^6
1 <= m <= min(50, nums.length)

__题目描述__:
给定一个非负整数数组和一个整数 m，你需要将这个数组分成 m 个非空的连续子数组。设计一个算法使得这 m 个子数组各自和的最大值最小。

__注意:__
数组长度 n 满足以下条件:

1 ≤ n ≤ 1000
1 ≤ m ≤ min(50, n)

__示例 :__

输入:
nums = [7,2,5,10,8]
m = 2

输出:
18

解释:
一共有四种方法将nums分割为2个子数组。
其中最好的方式是将其分为[7,2,5] 和 [10,8]，
因为此时这两个子数组各自的和的最大值为18，在所有情况中最小。

__思路__:
二分法, 贪心, 检测是否存在一种方案, 使得最大子数组之和的不超过 x
如果 m == 1, 则输出 sum(nums)
如果 m == len(nums), 则输出 max(nums)
现在 1 <= m <= n, 则在 [max(nums), sum(nums)]中找到一个合适的值使得划分之后的数组的最大值最小, 将这个区间的长度缩小到 1时, 就是结果
二分值 mid表示数组的容量, need表示子数组的个数, cur表示子数组之和
将数组元素逐个放入一个虚拟数组, 如果超过了 mid, 说明需要再开一个虚拟数组放剩下的元素, need自增, cur归零
如果 need > m, 说明子数组的数量多了, 那么 left应该右移, 增大子数组的容量
否则 right左移, 减少子数组的容量
时间复杂度O(nlgn), 空间复杂度O(1), 时间复杂度为 O(nlg(sum(nums) - max(nums)), max(nums)和 sum(nums)都可以在线性时间内计算得到

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int splitArray(vector<int>& nums, int m) 
    {
        long left = (long)(*max_element(nums.begin(), nums.end())), right = 0;
        for (auto &num : nums) right += num;
        while (left - right)
        {
            long mid = ((long)(left + right) >> 1), need = 1, cur = 0;
            for (auto &num : nums)
            {
                if (cur + num > mid)
                {
                    ++need;
                    cur = 0;
                }
                cur += num;
            }
            if (need > m) left = mid + 1;
            else right = mid;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int splitArray(int[] nums, int m) {
        long left = 0, right = 0;
        for (int num : nums) {
            left = Math.max(left, (long)num);
            right += num;
        }
        while (left - right != 0) {
            long mid = ((long)(left + right) >>> 1), need = 1, cur = 0;
            for (int num : nums) {
                if (cur + num > mid) {
                    ++need;
                    cur = 0;
                }
                cur += num;
            }
            if (need > m) left = mid + 1;
            else right = mid;
        }
        return (int)left;
    }
}
```

__Python__:

```Python
class Solution:
    def splitArray(self, nums: List[int], m: int) -> int:
        left, right = max(nums), sum(nums)
        while left - right:
            mid, need, cur = left + ((right - left) >> 1), 1, 0
            for num in nums:
                if cur + num > mid:
                    need += 1
                    cur = 0
                cur += num
            if need > m:
                left = mid + 1
            else:
                right = mid
        return left
```
