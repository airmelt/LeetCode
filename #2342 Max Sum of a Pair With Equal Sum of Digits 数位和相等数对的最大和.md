# 2342 Max Sum of a Pair With Equal Sum of Digits 数位和相等数对的最大和

__Description:__

You are given a __0-indexed__ array `nums` consisting of __positive__ integers. You can choose two indices `i` and `j`, such that `i != j`, and the sum of digits of the number `nums[i]` is equal to that of `nums[j]`.

Return _the __maximum__ value of_ `nums[i] + nums[j]` _that you can obtain over all possible indices_ `i` _and_ `j` _that satisfy the conditions._

__Example:__

Example 1:

```text
Input: nums = [18,43,36,13,7]
Output: 54
Explanation: The pairs (i, j) that satisfy the conditions are:
```

- (0, 2), both numbers have a sum of digits equal to 9, and their sum is 18 + 36 = 54.
- (1, 4), both numbers have a sum of digits equal to 7, and their sum is 43 + 7 = 50.

So the maximum sum that we can obtain is 54.

Example 2:

```text
Input: nums = [10,12,19,14]
Output: -1
Explanation: There are no two numbers that satisfy the conditions, so we return -1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，数组中的元素都是 __正__ 整数。请你选出两个下标 `i` 和 `j`（ `i != j`），且 `nums[i]` 的数位和 与  `nums[j]` 的数位和相等。

请你找出所有满足条件的下标 `i` 和 `j` ，找出并返回 `nums[i] + nums[j]` 可以得到的 __最大值__ _。_

__示例:__

示例 1：

```text
输入：nums = [18,43,36,13,7]
输出：54
解释：满足条件的数对 (i, j) 为：
```

- (0, 2) ，两个数字的数位和都是 9 ，相加得到 18 + 36 = 54 。
- (1, 4) ，两个数字的数位和都是 7 ，相加得到 43 + 7 = 50 。

所以可以获得的最大和是 54 。

示例 2：

```text
输入：nums = [10,12,19,14]
输出：-1
解释：不存在满足条件的数对，返回 -1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
哈希表
使用数组或者哈希表记录数位和
如果已经出现过相同的数位和, 则更新最大值
数组或哈希表中只记录最大值
时间复杂度为 O(NlogM), 空间复杂度为 O(ClogM), 其中 N 为数组长度, M 为数组中的最大值, C 为 9, 因为每位数字最大为 9
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumSum(vector<int>& nums) 
    {
        int result = -1;
        vector<int> c(82);
        for (const auto& num : nums) 
        {
            int s = 0;
            for (int x = num; x > 0; x /= 10) s += x % 10;
            if (c[s]) result = max(result, c[s] + num);
            c[s] = max(c[s], num);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumSum(int[] nums) {
        int result = -1, c[] = new int[82];
        for (int num : nums) {
            int s = 0;
            for (int x = num; x > 0; x /= 10) s += x % 10;
            if (c[s] > 0) result = Math.max(result, c[s] + num);
            c[s] = Math.max(c[s], num);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSum(self, nums: List[int]) -> int:
        d, result = defaultdict(int), -1
        for num in nums:
            if d[(s := sum(map(int, str(num))))]:
                result = max(result, d[s] + num)
            d[s] = max(d[s], num)
        return result
```
