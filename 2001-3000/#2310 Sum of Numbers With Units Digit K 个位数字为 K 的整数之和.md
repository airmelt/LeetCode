# 2310 Sum of Numbers With Units Digit K 个位数字为 K 的整数之和

__Description:__

Given two integers `num` and `k`, consider a set of positive integers with the following properties:

- The units digit of each integer is `k`.
- The sum of the integers is `num`.

Return _the __minimum__ possible size of such a set, or_ `-1` _if no such set exists._

Note:

- The set can contain multiple instances of the same integer, and the sum of an empty set is considered `0`.
- The __units digit__ of a number is the rightmost digit of the number.

__Example:__

Example 1:

```text
Input: num = 58, k = 9
Output: 2
Explanation:
One valid set is [9,49], as the sum is 58 and each integer has a units digit of 9.
Another valid set is [19,39].
It can be shown that 2 is the minimum possible size of a valid set.
```

Example 2:

```text
Input: num = 37, k = 2
Output: -1
Explanation: It is not possible to obtain a sum of 37 using only integers that have a units digit of 2.
```

Example 3:

```text
Input: num = 0, k = 7
Output: 0
Explanation: The sum of an empty set is considered 0.
```

__Constraints:__

- `0 <= num <= 3000`
- `0 <= k <= 9`

__题目描述:__

给你两个整数 `num` 和 `k` ，考虑具有以下属性的正整数多重集:

- 每个整数个位数字都是 `k` 。
- 所有整数之和是 `num` 。

返回该多重集的最小大小，如果不存在这样的多重集，返回 `-1` 。

注意：

- 多重集与集合类似，但多重集可以包含多个同一整数，空多重集的和为 `0` 。
- __个位数字__ 是数字最右边的数位。

__示例:__

示例 1：

```text
输入：num = 58, k = 9
输出：2
解释：
多重集 [9,49] 满足题目条件，和为 58 且每个整数的个位数字是 9 。
另一个满足条件的多重集是 [19,39] 。
可以证明 2 是满足题目条件的多重集的最小长度。
```

示例 2：

```text
输入：num = 37, k = 2
输出：-1
解释：个位数字为 2 的整数无法相加得到 37 。
```

示例 3：

```text
输入：num = 0, k = 7
输出：0
解释：空多重集的和为 0 。
```

__提示：__

- `0 <= num <= 3000`
- `0 <= k <= 9`

__思路:__

```text
数学
先特判 num 为 0 的情况
遍历 1 到 10, 判断 num - k * i 是否能被 10 整除
如果能被 10 整除, 返回 i
否则返回 -1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumNumbers(int num, int k) 
    {
        if (!num) return num;
        for (int i = 1; i <= 10 and num - k * i >= 0; i++) if (!((num - k * i) % 10)) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumNumbers(int num, int k) {
        if (num == 0) return num;
        for (int i = 1; i <= 10 && num - k * i >= 0; i++) if ((num - k * i) % 10 == 0) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumNumbers(self, num: int, k: int) -> int:
        return num if not num else -1 if not k and num % 10 else 1 if not k else next((n for n in range(1, min(num // k + 1, 11)) if (not (num - n * k) % 10)), -1)
```
