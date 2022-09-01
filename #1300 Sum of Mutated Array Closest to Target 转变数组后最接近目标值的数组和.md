# 1300 Sum of Mutated Array Closest to Target 转变数组后最接近目标值的数组和

__Description:__

Given an integer array arr and a target value target, return the integer value such that when we change all the integers larger than value in the given array to be equal to value, the sum of the array gets as close as possible (in absolute difference) to target.

In case of a tie, return the minimum such integer.

Notice that the answer is not neccesarilly a number from arr.

__Example:__

Example 1:

Input: arr = [4,9,3], target = 10
Output: 3
Explanation: When using 3 arr converts to [3, 3, 3] which sums 9 and that's the optimal answer.

Example 2:

Input: arr = [2,3,5], target = 10
Output: 5

Example 3:

Input: arr = [60864,25176,27249,21296,20204], target = 56803
Output: 11361

__Constraints:__

1 <= arr.length <= 10^4
1 <= arr[i], target <= 10^5

__题目描述：__

给你一个整数数组 arr 和一个目标值 target ，请你返回一个整数 value ，使得将数组中所有大于 value 的值变成 value 后，数组的和最接近  target （最接近表示两者之差的绝对值最小）。

如果有多种使得和最接近 target 的方案，请你返回这些整数中的最小值。

请注意，答案不一定是 arr 中的数字。

__示例：__

示例 1：

输入：arr = [4,9,3], target = 10
输出：3
解释：当选择 value 为 3 时，数组会变成 [3, 3, 3]，和为 9 ，这是最接近 target 的方案。

示例 2：

输入：arr = [2,3,5], target = 10
输出：5

示例 3：

输入：arr = [60864,25176,27249,21296,20204], target = 56803
输出：11361

__提示：__

1 <= arr.length <= 10^4
1 <= arr[i], target <= 10^5

__思路：__

二分查找
先计算 arr 中所有数字的和, 如果还达不到 target, 直接返回 arr 中最大元素的值
然后二分查找, 查找区间为 [1, target], 注意到不一定返回 arr 中的数字, 区间起点应为 1
每次二分查找时, 计算和, 如果和超过 target 就不用计算了
超过 target, 右区间收缩, 不足 target, 左区间收缩
最后需要判断一下区间端点更接近 target 的值
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    int check(vector<int>& arr, int mid)
    {
        int result = 0;
        for (const auto& num : arr) result += min(num, mid);
        return result;
    }
public:
    int findBestValue(vector<int>& arr, int target) 
    {
        int sum = accumulate(arr.begin(), arr.end(), 0), low = 1, high = target, n = arr.size(), min_value = INT_MAX, record = 0;
        if (sum <= target) return *max_element(arr.begin(), arr.end());
        while (low <= high)
        {
            int mid = low + ((high - low) >> 1), pre = 0;
            for (int i = 0; i < n && pre <= target; i++) pre += arr[i] > mid ? mid : arr[i];
            if (pre > target)
            {
                high = mid - 1;
                if (pre - target < min_value) min_value = pre - target;
            }
            else if (pre < target)
            {
                low = mid + 1;
                if (target - pre < min_value) min_value = target - pre;
            }
            else return mid;
            record = mid;
        }
        return abs(check(arr, low) - target) >= abs(check(arr, high) - target) ? high : low;
    }
};
```

__Java__:

```Java
class Solution {
    public int findBestValue(int[] arr, int target) {
        int n = arr.length, pre = 0;
        Arrays.sort(arr);
        for (int i = 0; i < n; i++) {
            int cur = pre + (n - i) * arr[i];
            if (cur >= target) {
                int value = (target - pre) / (n - i), low = pre + value * (n - i), high = low + n - i;
                if (target - low <= high - target) return value;
                return value + 1;
            }
            pre += arr[i];
        }
        return arr[n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def findBestValue(self, arr: List[int], target: int) -> int:
        n, pre = len(arr), 0
        arr.sort()
        for i in range(n):
            curr_sum = pre + (n - i) * arr[i]
            if curr_sum >= target:
                value = (target - pre) // (n - i)
                sum_low = pre + value * (n - i)
                sum_high = sum_low + n - i
                return value if (target - sum_low) <= (sum_high - target) else value + 1
            pre += arr[i]
        return arr[-1]
```
