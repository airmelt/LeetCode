# 2710 Remove Trailing Zeros From a String 移除字符串中的尾随零

__Description:__

Given a __positive__ integer `num` represented as a string, return _the integer_ `num` _without trailing zeros as a string_.

__Example:__

Example 1:

```text
Input: num = "51230100"
Output: "512301"
Explanation: Integer "51230100" has 2 trailing zeros, we remove them and return integer "512301".
```

Example 2:

```text
Input: num = "123"
Output: "123"
Explanation: Integer "123" has no trailing zeros, we return integer "123".
```

__Constraints:__

- `1 <= num.length <= 1000`
- `num` consists of only digits.
- `num` doesn't have any leading zeros.

__题目描述:__

给你一个用字符串表示的正整数 `num` ，请你以字符串形式返回不含尾随零的整数 `num` 。

__示例:__

示例 1：

```text
输入：num = "51230100"
输出："512301"
解释：整数 "51230100" 有 2 个尾随零，移除并返回整数 "512301" 。
```

示例 2：

```text
输入：num = "123"
输出："123"
解释：整数 "123" 不含尾随零，返回整数 "123" 。
```

__提示：__

- `1 <= num.length <= 1000`
- `num` 仅由数字 `0` 到 `9` 组成
- `num` 不含前导零

__思路:__

```text
模拟
题目保证数字为正整数
将最右边的 0 依次去掉即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string removeTrailingZeros(string num) 
    {
        num.erase(num.begin() + 1 + num.find_last_not_of('0'), num.end());
        return num;
    }
};
```

__Java__:

```Java
class Solution {
    public String removeTrailingZeros(String num) {
        return num.replaceAll("0+$", "");
    }
}
```

__Python__:

```Python
class Solution:
    def removeTrailingZeros(self, num: str) -> str:
        return num.rstrip('0')
```
