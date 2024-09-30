# 2269 Find the K-Beauty of a Number 找到一个数字的 K 美丽值

__Description:__

The __k-beauty__ of an integer `num` is defined as the number of __substrings__ of `num` when it is read as a string that meet the following conditions:

- It has a length of `k`.
- It is a divisor of `num`.

Given integers `num` and `k`, return _the k-beauty of_ `num`.

Note:

- __Leading zeros__ are allowed.
- `0` is not a divisor of any value.

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: num = 240, k = 2
Output: 2
Explanation: The following are the substrings of num of length k:
```

- "24" from "240": 24 is a divisor of 240.
- "40" from "240": 40 is a divisor of 240.

Therefore, the k-beauty is 2.

Example 2:

```text
Input: num = 430043, k = 2
Output: 2
Explanation: The following are the substrings of num of length k:
```

- "43" from "430043": 43 is a divisor of 430043.
- "30" from "430043": 30 is not a divisor of 430043.
- "00" from "430043": 0 is not a divisor of 430043.
- "04" from "430043": 4 is not a divisor of 430043.
- "43" from "430043": 43 is a divisor of 430043.

Therefore, the k-beauty is 2.

__Constraints:__

- `1 <= num <= 10 ^ 9`
- `1 <= k <= num.length` (taking `num` as a string)

__题目描述:__

一个整数 `num` 的 __k__ 美丽值定义为 `num` 中符合以下条件的 __子字符串__ 数目:

- 子字符串长度为 `k` 。
- 子字符串能整除 `num` 。

给你整数 `num` 和 `k` ，请你返回 `num` 的 k 美丽值。

注意：

- 允许有 __前缀__ __0__ 。
- `0` 不能整除任何值。

一个 子字符串 是一个字符串里的连续一段字符序列。

__示例:__

示例 1：

```text
输入：num = 240, k = 2
输出：2
解释：以下是 num 里长度为 k 的子字符串：
```

- "240" 中的 "24" ：24 能整除 240 。
- "240" 中的 "40" ：40 能整除 240 。

所以，k 美丽值为 2 。

示例 2：

```text
输入：num = 430043, k = 2
输出：2
解释：以下是 num 里长度为 k 的子字符串：
```

- "430043" 中的 "43" ：43 能整除 430043 。
- "430043" 中的 "30" ：30 不能整除 430043 。
- "430043" 中的 "00" ：0 不能整除 430043 。
- "430043" 中的 "04" ：4 不能整除 430043 。
- "430043" 中的 "43" ：43 能整除 430043 。

所以，k 美丽值为 2 。

__提示：__

- `1 <= num <= 10 ^ 9`
- `1 <= k <= num.length` （将 `num` 视为字符串）

__思路:__

```text
1. 字符串
将 num 转化为字符串
然后遍历字符串
每次取长度为 k 的子字符串
判断是否能整除 num
时间复杂度为 O(NK), 空间复杂度为 O(N)
2. 数学
取 x = num % 10 ^ k, 判断 x % 10 ^ k 是否能整除 num
然后将 x 除以 10, 继续判断 x % 10 ^ k 是否能整除 num
直到 x 比 10 ^ k 小
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int divisorSubstrings(int num, int k) 
    {
        int p = pow(10, k), result = 0;
        for (int x = num, min_value = p / 10, t = 0; x >= min_value; x /= 10) if ((t = x % p) and !(num % t)) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int divisorSubstrings(int num, int k) {
        int p = (int)Math.pow(10, k), result = 0;
        for (int x = num, minValue = p / 10, t = 0; x >= minValue; x /= 10) if ((t = x % p) > 0 && num % t == 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def divisorSubstrings(self, num: int, k: int) -> int:
        return sum(int(str(num)[i - k:i]) and not num % int(str(num)[i - k:i]) for i in range(k, len(str(num)) + 1))
```
