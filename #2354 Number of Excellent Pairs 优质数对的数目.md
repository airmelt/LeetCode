# 2354 Number of Excellent Pairs 优质数对的数目

__Description:__

You are given a __0-indexed__ positive integer array `nums` and a positive integer `k`.

A pair of numbers `(num1, num2)` is called __excellent__ if the following conditions are satisfied:

- __Both__ the numbers `num1` and `num2` exist in the array `nums`.
- The sum of the number of set bits in `num1 OR num2` and `num1 AND num2` is greater than or equal to `k`, where `OR` is the bitwise __OR__ operation and `AND` is the bitwise __AND__ operation.

Return the number of distinct excellent pairs.

Two pairs `(a, b)` and `(c, d)` are considered distinct if either `a != c` or `b != d`. For example, `(1, 2)` and `(2, 1)` are distinct.

__Note__ that a pair `(num1, num2)` such that `num1 == num2` can also be excellent if you have at least __one__ occurrence of `num1` in the array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,1], k = 3
Output: 5
Explanation: The excellent pairs are the following:
```

- (3, 3). (3 AND 3) and (3 OR 3) are both equal to (11) in binary. The total number of set bits is 2 + 2 = 4, which is greater than or equal to k = 3.
- (2, 3) and (3, 2). (2 AND 3) is equal to (10) in binary, and (2 OR 3) is equal to (11) in binary. The total number of set bits is 1 + 2 = 3.
- (1, 3) and (3, 1). (1 AND 3) is equal to (01) in binary, and (1 OR 3) is equal to (11) in binary. The total number of set bits is 1 + 2 = 3.

So the number of excellent pairs is 5.

Example 2:

```text
Input: nums = [5,1,1], k = 10
Output: 0
Explanation: There are no excellent pairs for this array.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 60`

__题目描述:__

给你一个下标从 __0__ 开始的正整数数组 `nums` 和一个正整数 `k` 。

如果满足下述条件，则数对 `(num1, num2)` 是 __优质数对__ :

- `num1` 和 `num2` __都__ 在数组 `nums` 中存在。
- `num1 OR num2` 和 `num1 AND num2` 的二进制表示中值为 __1__ 的位数之和大于等于 `k` ，其中 `OR` 是按位 __或__ 操作，而 `AND` 是按位 __与__ 操作。

返回 不同 优质数对的数目。

如果 `a != c` 或者 `b != d` ，则认为 `(a, b)` 和 `(c, d)` 是不同的两个数对。例如， `(1, 2)` 和 `(2, 1)` 不同。

__注意:__如果 `num1` 在数组中至少出现 __一次__ ，则满足 `num1 == num2` 的数对 `(num1, num2)` 也可以是优质数对。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,1], k = 3
输出：5
解释：有如下几个优质数对：
```

- (3, 3)：(3 AND 3) 和 (3 OR 3) 的二进制表示都等于 (11) 。值为 1 的位数和等于 2 + 2 = 4 ，大于等于 k = 3 。
- (2, 3) 和 (3, 2)： (2 AND 3) 的二进制表示等于 (10) ，(2 OR 3) 的二进制表示等于 (11) 。值为 1 的位数和等于 1 + 2 = 3 。
- (1, 3) 和 (3, 1)： (1 AND 3) 的二进制表示等于 (01) ，(1 OR 3) 的二进制表示等于 (11) 。值为 1 的位数和等于 1 + 2 = 3 。

所以优质数对的数目是 5 。

示例 2：

```text
输入：nums = [5,1,1], k = 10
输出：0
解释：该数组中不存在优质数对。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 60`

__思路:__

```text
注意到重复的数字不需要参与计算
因此可以使用哈希表记录每个数字的出现次数
如果 x | y 和 x & y 的二进制表示中 1 的个数之和大于等于 k, 则 x 和 y 各自的 1 的个数之和也大于等于 k
比如 x = 0b110, y = 0b101, x | y = 0b111, x & y = 0b100, 1 的个数之和为 3 + 1 = 4, 刚好就等于 x 和 y 各自的 1 的个数之和, 也就是 2 + 2 = 4
这是由于可以将 x 和 y 分别看作是 1 的集合, (x | y) + (x & y) = |x| + |y|
所以记录哈希表的时候只需要记录每个数字的 1 的个数即可
然后遍历哈希表, 当 c1 + c2 >= k 时, 优质数对的个数就是 v1 * v2
时间复杂度为 O(N + C), 空间复杂度为 O(N + C), 其中 N 为数组的长度, C 为数字的二进制表示中 1 的个数的种类, 这里 nums[i] <= 10 ^ 9, 二进制表示中 1 的个数最多为 30
```

__代码:__

__C++__:

```C++
class Solution:
    def countExcellentPairs(self, nums: List[int], k: int) -> int:
        return sum(v1 * v2 for c1, v1 in c.items() for c2, v2 in c.items() if c1 + c2 >= k) if (c := Counter(x.bit_count() for x in set(nums))) else 0
```

__Java__:

```Java
class Solution {
    public long countExcellentPairs(int[] nums, int k) {
        long bits[] = new long[32], result = 0L;
        Set<Integer> set = new HashSet<>();
        for (int num : nums) if (set.add(num)) ++bits[Integer.bitCount(num)];
        for (int i = 0; i < 32; i++) for (int j = 0; j < 32; j++) if (i + j >= k) result += (long)bits[i] * bits[j];
        return result;
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    long long countExcellentPairs(vector<int>& nums, int k) 
    {
        unordered_map<int, int> m;
        for (int x : unordered_set<int>(nums.begin(), nums.end())) ++m[__builtin_popcount(x)];
        long result = 0L;
        for (auto& [c1, v1] : m) for (auto& [c2, v2] : m) if (c1 + c2 >= k) result += (long)v1 * v2;
        return result;
    }
};
```
