# 2177 Find Three Consecutive Integers That Sum to a Given Number 找到和为给定整数的三个连续整数

__Description:__

Given an integer `num`, return _three consecutive integers (as a sorted array)_ _that __sum__ to_ `num`. If `num` cannot be expressed as the sum of three consecutive integers, return _an __empty__ array._

__Example:__

Example 1:

```text
Input: num = 33
Output: [10,11,12]
Explanation: 33 can be expressed as 10 + 11 + 12 = 33.
10, 11, 12 are 3 consecutive integers, so we return [10, 11, 12].
```

Example 2:

```text
Input: num = 4
Output: []
Explanation: There is no way to express 4 as the sum of 3 consecutive integers.
```

__Constraints:__

- `0 <= num <= 10 ^ 15`

__题目描述:__

给你一个整数 `num` ，请你返回三个连续的整数，它们的 __和__ 为 `num` 。如果 `num` 无法被表示成三个连续整数的和，请你返回一个 __空__ 数组。

__示例:__

示例 1：

```text
输入：num = 33
输出：[10,11,12]
解释：33 可以表示为 10 + 11 + 12 = 33 。
10, 11, 12 是 3 个连续整数，所以返回 [10, 11, 12] 。
```

示例 2：

```text
输入：num = 4
输出：[]
解释：没有办法将 4 表示成 3 个连续整数的和。
```

__提示：__

- `0 <= num <= 10 ^ 15`

__思路:__

```text
数学
检查 n 是否为 3 的倍数
如果是, 返回 [n / 3 - 1, n / 3, n / 3 + 1]
否则, 返回 []
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> sumOfThree(long long num) 
    {
        return (num % 3) ? vector<long long>(0) : vector<long long>{num / 3 - 1, num / 3, num / 3 + 1};
    }
};
```

__Java__:

```Java
class Solution {
    public long[] sumOfThree(long num) {
        return num % 3 != 0 ? new long[0] : new long[]{num / 3 - 1, num / 3, num / 3 + 1};
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfThree(self, num: int) -> List[int]:
        return [] if num % 3 else [num // 3 - 1, num // 3, num // 3 + 1]
```
