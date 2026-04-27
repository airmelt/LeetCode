# 2844 Minimum Operations to Make a Special Number 生成特殊数字的最少操作

__Description:__

You are given a __0-indexed__ string `num` representing a non-negative integer.

In one operation, you can pick any digit of `num` and delete it. Note that if you delete all the digits of `num`, `num` becomes `0`.

Return _the __minimum number of operations__ required to make_ `num` _special_.

An integer `x` is considered __special__ if it is divisible by `25`.

__Example:__

Example 1:

```text
Input: num = "2245047"
Output: 2
Explanation: Delete digits num[5] and num[6]. The resulting number is "22450" which is special since it is divisible by 25.
It can be shown that 2 is the minimum number of operations required to get a special number.
```

Example 2:

```text
Input: num = "2908305"
Output: 3
Explanation: Delete digits num[3], num[4], and num[6]. The resulting number is "2900" which is special since it is divisible by 25.
It can be shown that 3 is the minimum number of operations required to get a special number.
```

Example 3:

```text
Input: num = "10"
Output: 1
Explanation: Delete digit num[0]. The resulting number is "0" which is special since it is divisible by 25.
It can be shown that 1 is the minimum number of operations required to get a special number.
```

__Constraints:__

- `1 <= num.length <= 100`
- `num` only consists of digits `'0'` through `'9'`.
- `num` does not contain any leading zeros.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `num` ，表示一个非负整数。

在一次操作中，您可以选择 `num` 的任意一位数字并将其删除。请注意，如果你删除 `num` 中的所有数字，则 `num` 变为 `0`。

返回最少需要多少次操作可以使 `num` 变成特殊数字。

如果整数 `x` 能被 `25` 整除，则该整数 `x` 被认为是特殊数字。

__示例:__

示例 1：

```text
输入：num = "2245047"
输出：2
解释：删除数字 num[5] 和 num[6] ，得到数字 "22450" ，可以被 25 整除。
可以证明要使数字变成特殊数字，最少需要删除 2 位数字。
```

示例 2：

```text
输入：num = "2908305"
输出：3
解释：删除 num[3]、num[4] 和 num[6] ，得到数字 "2900" ，可以被 25 整除。
可以证明要使数字变成特殊数字，最少需要删除 3 位数字。
```

示例 3：

```text
输入：num = "10"
输出：1
解释：删除 num[0] ，得到数字 "0" ，可以被 25 整除。
可以证明要使数字变成特殊数字，最少需要删除 1 位数字。
```

__提示：__

- `1 <= num.length <= 100`
- `num` 仅由数字 `'0'` 到 `'9'` 组成
- `num` 不含任何前导零

__思路:__

```text
1. 逆序遍历
由于能够除尽 25 的只有
00/25/50/75 四种
从后往前遍历
如果出现了这四种组合就停止遍历返回长度
总共需要遍历 4 次
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 状态机
逆序遍历
如果出现过 0/5
将对应的 flag 设置为 true
接下来检查在 flag 为 true 的情况下是否出现
00/25/50/75 四种组合中的一种
即 0 出现后, 检查是当前位为 0 或 5
5 出现后, 检查当前位是否为 2 或 7
只需要遍历 1 次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperations(string num) 
    {
        int n = num.size(), zero = 0, five = 0;
        for (int i = n - 1; i > -1; i--) 
        {
            if ((zero and (num[i] == '0' or num[i] == '5')) or (five and (num[i] == '2' or num[i] == '7'))) return n - i - 2;
            zero = zero or (num[i] == '0');
            five = five or (num[i] == '5');
        }
        return n - zero;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperations(String num) {
        int n = num.length();
        boolean zero = false, five = false;
        for (int i = n - 1; i > -1; i--) {
            char c = num.charAt(i);
            if ((zero && (c == '0' || c == '5')) || (five && (c == '2' || c == '7'))) return n - i - 2;
            zero = zero || (c == '0');
            five = five || (c == '5');
        }
        return zero ? n - 1 : n;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperations(self, num: str) -> int:
        return 0 if not (n := len(num := num[::-1])) or not (f := lambda s: n if (a := num.find(s[1])) == -1 else n if (b := num.find(s[0], a + 1)) == -1 else b - 1) else min([n - ('0' in num)] + [f(sub) for sub in ("00", "25", "50", "75")])
```
