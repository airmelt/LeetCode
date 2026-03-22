# 805 Split Array With Same Average 数组的均值分割

__Description__:
You are given an integer array nums.

You should move each element of nums into one of the two arrays A and B such that A and B are non-empty, and average(A) == average(B).

Return true if it is possible to achieve that and false otherwise.

Note that for an array arr, average(arr) is the sum of all the elements of arr over the length of arr.

__Example:__
Example 1:

Input: nums = [1,2,3,4,5,6,7,8]
Output: true
Explanation: We can split the array into [1,4,5,8] and [2,3,6,7], and both of them have an average of 4.5.

Example 2:

Input: nums = [3,1]
Output: false

__Constraints:__

1 <= nums.length <= 30
0 <= nums[i] <= 10^4

__题目描述__:
给定的整数数组 A ，我们要将 A数组 中的每个元素移动到 B数组 或者 C数组中。（B数组和C数组在开始的时候都为空）

返回true ，当且仅当在我们的完成这样的移动后，可使得B数组的平均值和C数组的平均值相等，并且B数组和C数组都不为空。

__示例 :__

输入:
[1,2,3,4,5,6,7,8]
输出: true
解释: 我们可以将数组分割为 [1,4,5,8] 和 [2,3,6,7], 他们的平均值都是4.5。

__注意:__

A 数组的长度范围为 [1, 30].
A[i] 的数据范围为 [0, 10000].

__思路__:

1. 动态规划
若 B 数组长度为 k, A 数组长度为 n
avg(B) = sum(B) / k
avg(C) = (sum(A) - sum(B)) / (n - k)
avg(B) = avg(C) => sum(B) \* (n - k) = (sum(A) - sum(B)) \* k => sum(B) \* n = sum(A) \* k => sum(B) / k = sum(A) / n
即需要使得 B 的平均值等于 A 的平均值
预处理将 A 的每个元素减去平均值, 转化为找出 k 个数使得他们的和为 0
但是这一步计算会产生浮点数, 所以可以将 A 中每个元素扩大至 n 倍, 这样能保证 A 中元素还是整数
找出所有前 n / 2 的数组和 后 n / 2 的子序列和, 注意需要去掉全不选的情况 (即将第一个元素弹出)
然后将数组进行排序, 然后用双指针查找 left 和 right 中是否有相反数即可
时间复杂度为 O(2 ^ n), 空间复杂度为 O(2 ^ n)
2. DFS
sum(B) / k = sum(A) / n => sum(B) = sum(A) \* k / n, 注意到左边为整数所以右边也必须为整数
从第 1 个数开始查找, 找到 target = sum(A) \* i / n 即满足条件, 进入 DFS 之前注意去掉非整数剪枝
查找的时候可以略去重复元素剪枝
时间复杂度为 O(sn ^ 2), 空间复杂度为 O(n ^ 2), 其中 n 为 nums的大小, s 为 nums 的和

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<double> get_all(vector<double> nums)
    {
        int n = nums.size();
        vector<double> result(1 << n);
        for (int i = 0; i < n; i++) for (int j = 0; j < (1 << i); j++) result[j + (1 << i)] = result[j] + nums[i];
        return result;
    }
public:
    bool splitArraySameAverage(vector<int>& nums) 
    {
        int n = nums.size(), sum = accumulate(nums.begin(), nums.end(), 0), l = 0, r = 0;
        if (n == 1) return false;
        vector<double> dnums(n);
        for (int i = 0; i < n; i++) dnums[i] = nums[i] - (double)sum / n;
        vector<double> left = get_all({dnums.begin(), dnums.begin() + (n >> 1)}), right = get_all({dnums.begin() + (n >> 1), dnums.end()});
        left.erase(left.begin());
        for (const auto i : left) if (fabs(i) < 1e-6) return true; 
        left.pop_back();
        right.erase(right.begin());
        for (const auto i : right) if (fabs(i) < 1e-6) return true;
        right.pop_back();
        sort(left.rbegin(), left.rend());
        sort(right.begin(), right.end());
        while (l < left.size() and r < right.size())
        {
            double v = left[l] + right[r];
            if (fabs(v) < 1e-6) return true;
            else if (v > 0) ++l;
            else ++r;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean splitArraySameAverage(int[] nums) {
        int s = 0, n = nums.length;
        for (int num : nums) s += num;
        Arrays.sort(nums);
        for (int i = 1; i <= (n >> 1); i++) if (s * i % n == 0 && helper(nums, 0, n, i, s * i / n)) return true;
        return false;
    }
    
    private boolean helper(int[] nums, int start, int n, int s, int target) {
        if (s == 0) return target == 0;
        if (target < s * nums[start]) return false;
        for (int i = start; i <= n - s; i++) {
            if (i > start && nums[i] == nums[i - 1]) continue;
            if (helper(nums, i + 1, n, s - 1, target - nums[i])) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def splitArraySameAverage(self, nums: List[int]) -> bool:
        nums = [i * ( n := len(nums)) - (avg := sum(nums)) for i in nums]
        if 0 in nums and len(nums) > 1:
            return True
        a, b, dpa, dpb = [n for n in nums if n > 0], [-n for n in nums if n < 0], set(), set()
        for n in a:
            dpa |= {n + m for m in dpa} | {n}
        for n in b:
            dpb |= {n + m for m in dpb} | {n}
        return len(dpa & dpb) > 1
```
