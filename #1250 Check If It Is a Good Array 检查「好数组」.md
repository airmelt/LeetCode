# 1250 Check If It Is a Good Array 检查「好数组」

__Description:__

Given an array nums of positive integers. Your task is to select some subset of nums, multiply each element by an integer and add all these numbers. The array is said to be good if you can obtain a sum of 1 from the array by any possible subset and multiplicand.

Return True if the array is good otherwise return False.

__Example:__

Example 1:

Input: nums = [12,5,7,23]
Output: true
Explanation: Pick numbers 5 and 7.
5*3 + 7*(-2) = 1

Example 2:

Input: nums = [29,6,10]
Output: true
Explanation: Pick numbers 29, 6 and 10.
29*1 + 6*(-3) + 10*(-1) = 1

Example 3:

Input: nums = [3,6]
Output: false

__Constraints:__

1 <= nums.length <= 10^5
1 <= nums[i] <= 10^9

__题目描述：__

给你一个正整数数组 nums，你需要从中任选一些子集，然后将子集中每一个数乘以一个 任意整数，并求出他们的和。

假如该和结果为 1，那么原数组就是一个「好数组」，则返回 True；否则请返回 False。

__示例：__

示例 1：

输入：nums = [12,5,7,23]
输出：true
解释：挑选数字 5 和 7。
5*3 + 7*(-2) = 1

示例 2：

输入：nums = [29,6,10]
输出：true
解释：挑选数字 29, 6 和 10。
29*1 + 6*(-3) + 10*(-1) = 1

示例 3：

输入：nums = [3,6]
输出：false

__提示：__

1 <= nums.length <= 10^5
1 <= nums[i] <= 10^9

__思路：__

数学
实际上就是求数组中所有数的最大公约数是否为 1
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    bool isGoodArray(vector<int>& nums) 
    {
        return accumulate(next(nums.begin()), nums.end(), nums.front(), [](const auto& a, const auto & b){return __gcd(a, b);}) == 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isGoodArray(int[] nums) {
        return Arrays.asList(IntStream.of(nums).boxed().toArray(Integer[]::new)).stream().reduce((a, b) -> gcd(a, b)).get() == 1;
    }
    
    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def isGoodArray(self, nums: List[int]) -> bool:
        return functools.reduce(math.gcd, nums) == 1
```
