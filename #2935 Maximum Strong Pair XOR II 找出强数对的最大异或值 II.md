# 2935 Maximum Strong Pair XOR II 找出强数对的最大异或值 II

__Description:__

You are given a __0-indexed__ integer array `nums`. A pair of integers `x` and `y` is called a __strong__ pair if it satisfies the condition:

- `|x - y| <= min(x, y)`

You need to select two integers from `nums` such that they form a strong pair and their bitwise `XOR` is the __maximum__ among all strong pairs in the array.

Return _the __maximum___ `XOR` _value out of all possible strong pairs in the array_ `nums`.

Note that you can pick the same integer twice to form a pair.

__Example:__

Example 1:

```text
Input:  nums = [1,2,3,4,5]
Output:  7
Explanation:  There are 11 strong pairs in the array nums: (1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) and (5, 5).
The maximum XOR possible from these pairs is 3 XOR 4 = 7.
```

Example 2:

```text
Input: nums = [10,100]
Output: 0
Explanation: There are 2 strong pairs in the array nums: (10, 10) and (100, 100).
The maximum XOR possible from these pairs is 10 XOR 10 = 0 since the pair (100, 100) also gives 100 XOR 100 = 0.
```

Example 3:

```text
Input: nums = [500,520,2500,3000]
Output: 1020
Explanation: There are 6 strong pairs in the array nums: (500, 500), (500, 520), (520, 520), (2500, 2500), (2500, 3000) and (3000, 3000).
The maximum XOR possible from these pairs is 500 XOR 520 = 1020 since the only other non-zero XOR value is 2500 XOR 3000 = 636.
```

__Constraints:__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `1 <= nums[i] <= 2 ^ 20 - 1`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。如果一对整数 `x` 和 `y` 满足以下条件，则称其为 __强数对__ :

- `|x - y| <= min(x, y)`

你需要从 `nums` 中选出两个整数，且满足:这两个整数可以形成一个强数对，并且它们的按位异或（ `XOR`）值是在该数组所有强数对中的 __最大值__ 。

返回数组 `nums` 所有可能的强数对中的 __最大__ 异或值。

注意，你可以选择同一个整数两次来形成一个强数对。

__示例:__

示例 1：

```text
输入: nums = [1,2,3,4,5]
输出: 7
解释: 数组 nums 中有 11 个强数对:(1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) 和 (5, 5) 。
这些强数对中的最大异或值是 3 XOR 4 = 7 。
```

示例 2：

```text
输入: nums = [10,100]
输出: 0
解释: 数组 nums 中有 2 个强数对:(10, 10) 和 (100, 100) 。
这些强数对中的最大异或值是 10 XOR 10 = 0 ，数对 (100, 100) 的异或值也是 100 XOR 100 = 0 。
```

示例 3：

```text
输入: nums = [500,520,2500,3000]
输出: 1020
解释: 数组 nums 中有 6 个强数对:(500, 500), (500, 520), (520, 520), (2500, 2500), (2500, 3000) 和 (3000, 3000) 。
这些强数对中的最大异或值是 500 XOR 520 = 1020 ；另一个异或值非零的数对是 (5, 6) ，其异或值是 2500 XOR 3000 = 636 。
```

__提示：__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `1 <= nums[i] <= 2 ^ 20 - 1`

__思路:__

```text
哈希表
由于和数组元素顺序无关, 可以先将数组排序
排序之后对于 x <= y 有 |x - y| = y - x, min(x, y) = x
所以 |x - y| <= min(x, y) 即 y - x <= x, 即 y <= x * 2
对于 2 ^ i, 或者 1 << i
设下一个答案为 next_result, next_result = result | (1 << i)
记录 mask 为前 i 个二进制位为 1 的数, 即 0b111...
由于 a ^ b ^ b = a
从所有的数组元素中找到一个数 num 使得 num & mask ^ next_result 在之前出现过, 且 d[num & mask ^ next_result] * 2 <= num
那么 next_result 就能被取到, 两个数分别为 num 和 d[num & mask ^ next_result]
然后将 d[num & mask] = num 记录在哈希表中
时间复杂度为 O(NlogM + NlogN), 空间复杂度为 O(N), M 为数组最大值, logM 也为最大值的二进制长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumStrongPairXor(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), max_length = 31 - __builtin_clz(nums.back()), mask = (1 << max_length), result = 0, next_result = (1 << max_length);
        unordered_map<int, int> m;
        for (int i = max_length; i > -1; i--, mask |= (1 << max(i, 0)), next_result = result | (1 << max(i, 0))) 
        {
            m.clear();
            for (int num : nums) 
            {
                if ((m[num & mask ^ next_result] << 1) >= num) 
                {
                    result = next_result;
                    break;
                }
                m[num & mask] = num;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumStrongPairXor(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length, maxLength = 31 - Integer.numberOfLeadingZeros(nums[n - 1]), mask = (1 << maxLength), result = 0, nextResult = (1 << maxLength);
        var map = new HashMap<Integer, Integer>();
        for (int i = maxLength; i > -1; i--, mask |= (1 << i), nextResult = result | (1 << i)) {
            map.clear();
            for (int num : nums) {
                if ((map.getOrDefault(num & mask ^ nextResult, 0) << 1) >= num) {
                    result = nextResult;
                    break;
                }
                map.put(num & mask, num);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumStrongPairXor(self, nums: List[int]) -> int:
        nums.sort()
        result, mask, max_length = 0, 0, nums[-1].bit_length() - 1
        for i in range(max_length, -1, -1):
            mask |= 1 << i
            next_result, d = result | (1 << i), defaultdict(int)
            for num in nums:
                if (d[(cur := num & mask) ^ next_result] << 1) >= num:
                    result = next_result
                    break
                d[cur] = num
        return result
```
