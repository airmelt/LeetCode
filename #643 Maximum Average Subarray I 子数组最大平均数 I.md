__Description__:
Given an array consisting of n integers, find the contiguous subarray of given length k that has the maximum average value. And you need to output the maximum average value.

__Example:__
Example 1:

Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation: Maximum average is (12-5-6+50)/4 = 51/4 = 12.75

__Note:__

1 <= k <= n <= 30,000.
Elements of the given array will be in the range [-10,000, 10,000].

__题目描述__:
给定 n 个整数，找出平均数最大且长度为 k 的连续子数组，并输出该最大平均数。

__示例 :__
示例 1:

输入: [1,12,-5,-6,50,3], k = 4
输出: 12.75
解释: 最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
 
__注意：__

1 <= k <= n <= 30,000。
所给数据范围 [-10,000，10,000]。

__思路__:
采取滑动窗口的思想, 遍历的时候删除第一个元素然后加上最后一个元素
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double result = 0;
        for (int i = 0; i < k; i++) result += nums[i];
        double temp = result;
        for (int i = 1;i + k - 1< nums.size(); i++) {
            temp += nums[i + k - 1] - nums[i - 1];
            if (temp > result) result = temp;
        }
        return result / k;
    }
};
```

__Java__:
```
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double result = 0;
        for (int i = 0; i < k; i++) result += nums[i];
        double temp = result;
        for (int i = 1;i + k - 1< nums.length; i++) {
            temp += nums[i + k - 1] - nums[i - 1];
            if (temp > result) result = temp;
        }
        return result / k;
    }
}
```

__Python__:
```
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        result, temp = sum(nums[:k]), sum(nums[:k])
        for i in range(len(nums) - k):
            temp += nums[i + k] - nums[i]
            if temp > result:
                result = temp
        return result * 1.0 / k
```