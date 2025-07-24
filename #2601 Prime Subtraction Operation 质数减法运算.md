# 2601 Prime Subtraction Operation 质数减法运算

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n`.

You can perform the following operation as many times as you want:

- Pick an index `i` that you haven’t picked before, and pick a prime `p` __strictly less than__ `nums[i]`, then subtract `p` from `nums[i]`.

Return _true if you can make `nums` a strictly increasing array using the above operation and false otherwise._

A strictly increasing array is an array whose each element is strictly greater than its preceding element.

__Example:__

Example 1:

```text
Input: nums = [4,9,6,10]
Output: true
Explanation: In the first operation: Pick i = 0 and p = 3, and then subtract 3 from nums[0], so that nums becomes [1,9,6,10].
In the second operation: i = 1, p = 7, subtract 7 from nums[1], so nums becomes equal to [1,2,6,10].
After the second operation, nums is sorted in strictly increasing order, so the answer is true.
```

Example 2:

```text
Input: nums = [6,8,11,12]
Output: true
Explanation: Initially nums is sorted in strictly increasing order, so we don't need to make any operations.
```

Example 3:

```text
Input: nums = [5,8,3]
Output: false
Explanation: It can be proven that there is no way to perform operations to make nums sorted in strictly increasing order, so the answer is false.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `nums.length == n`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，数组长度为 `n` 。

你可以执行无限次下述运算：

- 选择一个之前未选过的下标 `i` ，并选择一个 __严格小于__ `nums[i]` 的质数 `p` ，从 `nums[i]` 中减去 `p` 。

如果你能通过上述运算使得 `nums` 成为严格递增数组，则返回 `true` ；否则返回 `false` 。

严格递增数组 中的每个元素都严格大于其前面的元素。

__示例:__

示例 1：

```text
输入：nums = [4,9,6,10]
输出：true
解释：
在第一次运算中：选择 i = 0 和 p = 3 ，然后从 nums[0] 减去 3 ，nums 变为 [1,9,6,10] 。
在第二次运算中：选择 i = 1 和 p = 7 ，然后从 nums[1] 减去 7 ，nums 变为 [1,2,6,10] 。
第二次运算后，nums 按严格递增顺序排序，因此答案为 true 。
```

示例 2：

```text
输入：nums = [6,8,11,12]
输出：true
解释：nums 从一开始就按严格递增顺序排序，因此不需要执行任何运算。
```

示例 3：

```text
输入：nums = [5,8,3]
输出：false
解释：可以证明，执行运算无法使 nums 按严格递增顺序排序，因此答案是 false 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `nums.length == n`

__思路:__

```text
二分
预处理 1000 以内的所有质数
可以使用筛法, 或者直接打表
记前一个数为 pre, 初始化为 0
从左到右遍历 nums
如果当前数 nums[i] >= pre, 返回 false
否则, 计算 nums[i] - pre 的值, 使用二分查找找到最大的质数 p, 使得 pre < nums[i] - p
令 pre = nums[i] - p
时间复杂度为 O(N), 空间复杂度为 O(1), 不计预处理时间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool primeSubOperation(vector<int>& nums) 
    {
        int pre = 0;
        for (const auto& num : nums) 
        {
            if (num <= pre) return false;
            pre = num - *--lower_bound(primes.begin(), primes.end(), num - pre);
        }
        return true;
    }
private:
    vector<int> primes{0, 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997};
};
```

__Java__:

```Java
class Solution {
    private final static int MX = 1000;
    private final static int[] primes = new int[169];

    static {
        var flag = new boolean[MX + 1];
        for (int i = 2, j = 1; i <= MX; i++) {
            if (!flag[i]) {
                primes[j++] = i;
                for (int k = i; k <= MX / i; ++k) flag[i * k] = true;
            }
        }
    }

    public boolean primeSubOperation(int[] nums) {
        int pre = 0;
        for (int num : nums) {
            if (num <= pre) return false;
            pre = num - primes[helper(primes, num - pre)];
        }
        return true;
    }

    private int helper(int[] nums, int target) {
        int left = 0, right = nums.length - 1, mid = 0;
        while (left <= right) {
            if (nums[mid = left + ((right - left) >>> 1)] < target) left = mid + 1;
            else right = mid - 1;
        }
        return right;
    }
}
```

__Python__:

```Python
class Solution:
    def primeSubOperation(self, nums: List[int]) -> bool:
        primes, n, nums = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997], len(nums), [0] + nums
        for i in range(n):
            if nums[i + 1] > 2:
                nums[i + 1] -= primes[a - 1] if (a := bisect_left(primes, nums[i + 1] - nums[i])) else 0
        return sorted(nums) == nums and len(set(nums)) == n + 1

```
