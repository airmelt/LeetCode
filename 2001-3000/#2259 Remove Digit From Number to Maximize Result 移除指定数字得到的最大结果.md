# 2259 Remove Digit From Number to Maximize Result 移除指定数字得到的最大结果

__Description:__

You are given a string `number` representing a __positive integer__ and a character `digit`.

Return _the resulting string after removing __exactly one occurrence__ of_ `digit` _from_ `number` _such that the value of the resulting string in __decimal__ form is __maximized___. The test cases are generated such that `digit` occurs at least once in `number`.

__Example:__

Example 1:

```text
Input: number = "123", digit = "3"
Output: "12"
Explanation: There is only one '3' in "123". After removing '3', the result is "12".
```

Example 2:

```text
Input: number = "1231", digit = "1"
Output: "231"
Explanation: We can remove the first '1' to get "231" or remove the second '1' to get "123".
Since 231 > 123, we return "231".
```

Example 3:

```text
Input: number = "551", digit = "5"
Output: "51"
Explanation: We can remove either the first or second '5' from "551".
Both result in the string "51".
```

__Constraints:__

- `2 <= number.length <= 100`
- `number` consists of digits from `'1'` to `'9'`.
- `digit` is a digit from `'1'` to `'9'`.
- `digit` occurs at least once in `number`.

__题目描述:__

给你一个表示某个正整数的字符串 `number` 和一个字符 `digit` 。

从 `number` 中 __恰好__ 移除 __一个__ 等于 `digit` 的字符后，找出并返回按 __十进制__ 表示 __最大__ 的结果字符串。生成的测试用例满足 `digit` 在 `number` 中出现至少一次。

__示例:__

示例 1：

```text
输入：number = "123", digit = "3"
输出："12"
解释："123" 中只有一个 '3' ，在移除 '3' 之后，结果为 "12" 。
```

示例 2：

```text
输入：number = "1231", digit = "1"
输出："231"
解释：可以移除第一个 '1' 得到 "231" 或者移除第二个 '1' 得到 "123" 。
由于 231 > 123 ，返回 "231" 。
```

示例 3：

```text
输入：number = "551", digit = "5"
输出："51"
解释：可以从 "551" 中移除第一个或者第二个 '5' 。
两种方案的结果都是 "51" 。
```

__提示：__

- `2 <= number.length <= 100`
- `number` 由数字 `'1'` 到 `'9'` 组成
- `digit` 是 `'1'` 到 `'9'` 中的一个数字
- `digit` 在 `number` 中出现至少一次

__思路:__

```text
1. 暴力
模拟移除每一个与 digit 相等的位置的数字, 返回最大值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 贪心
从左到右遍历
如果该位置与 digit 相等且比后面的数字小, 则移除该位置的数字
否则移除最后一个与 digit 相等的数字
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string removeDigit(string number, char digit) 
    {
        string result;
        for (int i = 0, n = number.size(); i < n; i++) if (number[i] == digit) result = max(result, number.substr(0, i) + number.substr(i + 1, n - i));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String removeDigit(String number, char digit) {
        StringBuilder sb = new StringBuilder();
        boolean flag = false;
        int idx = 0, n = number.length();
        for (int i = 0; i < n; i++) {
            if (!flag && number.charAt(i) == digit) {
                idx = i;
                if (i + 1 < n && number.charAt(i + 1) > digit) {
                    flag = true;
                    continue;
                }
            }
            sb.append(number.charAt(i));
        }
        return flag ? sb.toString() : sb.deleteCharAt(idx).toString();
    }
}
```

__Python__:

```Python
class Solution:
    def removeDigit(self, number: str, digit: str) -> str:
        return max(number[:i] + number[i + 1:] for i, c in enumerate(number) if c == digit)
```
