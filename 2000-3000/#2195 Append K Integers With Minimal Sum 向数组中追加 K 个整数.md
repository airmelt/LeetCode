# 2195 Append K Integers With Minimal Sum 向数组中追加 K 个整数

__Description:__

You are given an integer array `nums` and an integer `k`. Append `k` __unique positive__ integers that do __not__ appear in `nums` to `nums` such that the resulting total sum is __minimum__.

Return _the sum of the_ `k` _integers appended to_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,4,25,10,25], k = 2
Output: 5
Explanation: The two unique positive integers that do not appear in nums which we append are 2 and 3.
The resulting sum of nums is 1 + 4 + 25 + 10 + 25 + 2 + 3 = 70, which is the minimum.
The sum of the two integers appended is 2 + 3 = 5, so we return 5.
```

Example 2:

```text
Input: nums = [5,6], k = 6
Output: 25
Explanation: The six unique positive integers that do not appear in nums which we append are 1, 2, 3, 4, 7, and 8.
The resulting sum of nums is 5 + 6 + 1 + 2 + 3 + 4 + 7 + 8 = 36, which is the minimum. 
The sum of the six integers appended is 1 + 2 + 3 + 4 + 7 + 8 = 25, so we return 25.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 8`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。请你向 `nums` 中追加 `k` 个 __未__ 出现在 `nums` 中的、__互不相同__ 的 __正__ 整数，并使结果数组的元素和 __最小__ 。

返回追加到 `nums` 中的 `k` 个整数之和。

__示例:__

示例 1：

```text
输入：nums = [1,4,25,10,25], k = 2
输出：5
解释：在该解法中，向数组中追加的两个互不相同且未出现的正整数是 2 和 3 。
nums 最终元素和为 1 + 4 + 25 + 10 + 25 + 2 + 3 = 70 ，这是所有情况中的最小值。
所以追加到数组中的两个整数之和是 2 + 3 = 5 ，所以返回 5 。
```

示例 2：

```text
输入：nums = [5,6], k = 6
输出：25
解释：在该解法中，向数组中追加的两个互不相同且未出现的正整数是 1 、2 、3 、4 、7 和 8 。
nums 最终元素和为 5 + 6 + 1 + 2 + 3 + 4 + 7 + 8 = 36 ，这是所有情况中的最小值。
所以追加到数组中的两个整数之和是 1 + 2 + 3 + 4 + 7 + 8 = 25 ，所以返回 25 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 9`

__思路:__

```text
数学
如果不考虑出现的数字
实际上要得到 k 个数字, 由等差数列求和公式只需要返回 (1 + k) * k / 2 即可
现在需要得到不重复的 k 个数字
我们可以跳过 nums 中出现的数字
得到 k 再减去 nums 中所有数字之和即可
将 nums 排序并去重得到 unique 数组
对于每一个 unique[i], 如果 nums[i] <= k, 则 s += nums[i], ++k
否则跳过这个 unique[i]
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimalKSum(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int n = unique(nums.begin(), nums.end()) - nums.begin();
        long long s = 0;
        for (int i = 0; i < n; i++)
        {
            if (nums[i] <= k)
            {
                s += nums[i];
                ++k;
            }
        }
        return ((1LL + k) * k >> 1LL) - s;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimalKSum(int[] nums, int k) {
        TreeSet<Integer> set = new TreeSet<>();
        for (int num : nums) set.add(num);
        long s = 0;
        int[] unique = new int[set.size()];
        for (int i = 0, n = set.size(); i < n; i++) unique[i] = set.pollFirst();
        for (int i = 0, n = unique.length; i < n; i++) {
            if (unique[i] <= k) {
                s += unique[i];
                ++k;
            }
        }
        return ((1L + k) * k >> 1L) - s;
    }
}
```

__Python__:

```Python
class Solution:
    def minimalKSum(self, nums: List[int], k: int) -> int:
        nums = sorted(list(set(nums)))
        n, s = len(nums), 0
        for i in range(n):
            if nums[i] <= k:
                s += nums[i]
                k += 1
        return ((1 + k) * k >> 1) - s
```
