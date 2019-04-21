__Description__:
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

__Example__:

Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

__Follow up__:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

__题目描述__:
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

 __示例__:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

__进阶__:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

__思路__:
1. 对每一个数分别求取最大连续子数组(Python)
[-2,1,-3,4,-1,2,1,-5,4] -> [2, 4, 3, 6, 2, 3, -1, 4]
时间复杂度为O(n^2), 空间复杂度为O(1)
2. 分治法(Java):
最大子序和要么在左半部分, 要么在右半部分, 要么横跨两个部分.  最后这个部分应该包含左半部分的最后一个元素以及右半部分的第一个元素
注意边界情况的处理
时间复杂度为O(nlgn), 空间复杂度O(lgn)
3. 动态规划(C++):
从方法1可以看到, 求取子数组的时候大量的进行了重复的运算
另外, 如果已知(n - 1)个元素的最大连续子数组, 那么对第 n个元素an, 要么加入这个元素, 凑成更大的最大连续子数组, 要么这个元素比之前的最大连续子数组还要大, 直接选择这个元素
可以得到这样一个等式: f(n) = max{[f(n - 1) + an], an}
时间复杂度为O(n), 空间复杂度为O(1)

[动态规划 dynamic programming-wiki](https://en.wikipedia.org/wiki/Dynamic_programming):
__动态规划(dynamic programming)__ 与分治方法相似, 都是通过组合子问题的解来求解原问题(在这里, "programming"指的是一种表格法, 并非编写计算机程序)
动态规划方法通常用来求解 __最优化问题(optimization problem)__. 我们称这样的解为问题的 __一个最优解(an optimal solution)__, 而不是 __最优解(the optimal solution)__, 因为可能有多个解都达到最优值.
总体上来说, 动态规划是一种以空间换时间的高效解法.
其步骤大致如下:
1. 刻画一个最优解的结构特征.
2. 递归地定义最优解的值.
3. 计算最优解的值, 通常采用自底向上的方法.
4. 利用计算出的信息构造一个最优解.

对于上面的最大子序和
可以转换为, 对前 n - 1个数有最优解 f(n - 1). 那么构成 n个数的最优解 f(n), 要么是f(n - 1), 要么包含第 n个元素 an, 即 f(n - 1) + an.
则 f(n) = max[f(n - 1), f(n - 1) + an]

解法通常有两种:
第一种方法称为 __带备忘的自顶向下法(top-down with memoization)__
递归过程是 __带备忘的(memoized)__
第二种方法称为 __自底向上法(bottom-up method)__
自顶向下方法并未真正递归地考察所有可能的子问题. 由于没有频繁的递归函数调用的开销, 自底向上方法的时间复杂性函数通常具有更小的系数.


__代码__:
__C++__:
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = nums[0];
        int temp = nums[0] > 0 ? nums[0] : 0;
        for (int i = 1; i < nums.size(); i++) {
            temp += nums[i];
            if (temp > result) result = temp;
            if (temp < 0) temp = 0;
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int maxSubArray(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        return divide(nums, low, high);
    }

    private int divide(int[] nums, int low, int high) {
        if (low == high) return nums[low];
        int mid = (low + high) / 2;
        int left = divide(nums, low, mid);
        int right = divide(nums, mid + 1, high);
        int l = nums[mid];
        int r = nums[mid + 1];
        int lmax = nums[mid];
        int rmax = nums[mid + 1];
        for (int i = mid + 2; i < high + 1; i++) {
            r += nums[i];
            if (r > rmax) rmax = r;
        }
        for (int i = mid - 1; i > low - 1; i--) {
            l += nums[i];
            if (l > lmax) lmax = l;
        }
        int result = lmax + rmax;
        result = result > left ? result : left;
        result = result > right ? result : right;
        return result;
    }
}
```

__Python__:
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        for i in range(1, len(nums)):
            nums[i] += max(nums[i - 1], 0)
        return max(nums)
```
