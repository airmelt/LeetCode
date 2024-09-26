# 2264 Largest 3-Same-Digit Number in String 字符串中最大的 3 位相同数字

__Description:__

You are given a string `num` representing a large integer. An integer is __good__ if it meets the following conditions:

- It is a __substring__ of `num` with length `3`.
- It consists of only one unique digit.

Return _the __maximum good__ integer as a __string__ or an empty string_ `""` _if no such integer exists_.

Note:

- A __substring__ is a contiguous sequence of characters within a string.
- There may be __leading zeroes__ in `num` or a good integer.

__Example:__

Example 1:

```text
Input: num = "6777133339"
Output: "777"
Explanation: There are two distinct good integers: "777" and "333".
"777" is the largest, so we return "777".
```

Example 2:

```text
Input: num = "2300019"
Output: "000"
Explanation: "000" is the only good integer.
```

Example 3:

```text
Input: num = "42352338"
Output: ""
Explanation: No substring of length 3 consists of only one unique digit. Therefore, there are no good integers.
```

__Constraints:__

- `3 <= num.length <= 1000`
- `num` only consists of digits.

__题目描述:__

给你一个字符串 `num` ，表示一个大整数。如果一个整数满足下述所有条件，则认为该整数是一个 __优质整数__ :

- 该整数是 `num` 的一个长度为 `3` 的 __子字符串__ 。
- 该整数由唯一一个数字重复 `3` 次组成。

以字符串形式返回 __最大的优质整数__ 。如果不存在满足要求的整数，则返回一个空字符串 `""` 。

注意：

- __子字符串__ 是字符串中的一个连续字符序列。
- `num` 或优质整数中可能存在 __前导零__ 。

__示例:__

示例 1：

```text
输入：num = "6777133339"
输出："777"
解释：num 中存在两个优质整数："777" 和 "333" 。
"777" 是最大的那个，所以返回 "777" 。
```

示例 2：

```text
输入：num = "2300019"
输出："000"
解释："000" 是唯一一个优质整数。
```

示例 3：

```text
输入：num = "42352338"
输出：""
解释：不存在长度为 3 且仅由一个唯一数字组成的整数。因此，不存在优质整数。
```

__提示：__

- `3 <= num.length <= 1000`
- `num` 仅由数字（ `0` - `9`）组成

__思路:__

```text
模拟
从 '999' 开始检查是否在 num 中
如果存在则返回
否则返回空字符串
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string largestGoodInteger(string num) 
    {
        string result;
        for (int i = 2, n = num.size(); i < n; i++) if (num[i] == num[i - 1] and num[i - 1] == num[i - 2]) result = max(result, num.substr(i - 2, 3));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String largestGoodInteger(String num) {
        String[] arr = { "999", "888", "777", "666", "555", "444", "333", "222", "111", "000" };
        for (String s : arr) if (num.indexOf(s) >= 0) return s;
        return "";
    }
}
```

__Python__:

```Python
class Solution:
    def largestGoodInteger(self, num: str) -> str:
        return next(filter(lambda n: n in num, ['999', '888', '777', '666', '555', '444', '333', '222', '111', '000']), '')
```
