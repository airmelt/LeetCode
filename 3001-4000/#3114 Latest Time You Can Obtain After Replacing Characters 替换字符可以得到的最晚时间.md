# 3114 Latest Time You Can Obtain After Replacing Characters 替换字符可以得到的最晚时间

__Description:__

You are given a string `s` representing a 12-hour format time where some of the digits (possibly none) are replaced with a `"?"`.

12-hour times are formatted as `"HH:MM"`, where `HH` is between `00` and `11`, and `MM` is between `00` and `59`. The earliest 12-hour time is `00:00`, and the latest is `11:59`.

You have to replace __all__ the `"?"` characters in `s` with digits such that the time we obtain by the resulting string is a __valid__ 12-hour format time and is the __latest__ possible.

Return the resulting string.

__Example:__

Example 1:

```text
Input: s = "1?:?4"

Output: "11:54"
```

__Explanation:__ The latest 12-hour format time we can achieve by replacing `"?"` characters is `"11:54"`.

Example 2:

```text
Input: s = "0?:5?"

Output: "09:59"
```

__Explanation:__ The latest 12-hour format time we can achieve by replacing `"?"` characters is `"09:59"`.

__Constraints:__

- `s.length == 5`
- `s[2]` is equal to the character `":"`.
- All characters except `s[2]` are digits or `"?"` characters.
- The input is generated such that there is __at least__ one time between `"00:00"` and `"11:59"` that you can obtain after replacing the `"?"` characters.

__题目描述:__

给你一个字符串 `s`，表示一个 12 小时制的时间格式，其中一些数字（可能没有）被 `"?"` 替换。

12 小时制时间格式为 `"HH:MM"` ，其中 `HH` 的取值范围为 `00` 至 `11`， `MM` 的取值范围为 `00` 至 `59`。最早的时间为 `00:00`，最晚的时间为 `11:59`。

你需要将 `s` 中的 __所有__ `"?"` 字符替换为数字，使得结果字符串代表的时间是一个 __有效__ 的 12 小时制时间，并且是可能的 __最晚__ 时间。

返回结果字符串。

__示例:__

示例 1：

```text
输入： s = "1?:?4"

输出： "11:54"
```

__解释:__ 通过替换 `"?"` 字符，可以得到的最晚12小时制时间是 `"11:54"`。

示例 2：

```text
输入： s = "0?:5?"

输出： "09:59"
```

__解释:__ 通过替换 `"?"` 字符，可以得到的最晚12小时制时间是 `"09:59"`。

__提示：__

- `s.length == 5`
- `s[2]` 是字符 `":"`
- 除 `s[2]` 外，其他字符都是数字或 `"?"`
- 输入保证在替换 `"?"` 字符后至少存在一个介于 `"00:00"` 和 `"11:59"` 之间的时间。

__思路:__

```text
模拟
逐个遍历
首位如果可以修改, 那么检查第二位是 '?' 或者小于 '2' 的时候才能填入 '1', 否则只能填入 '0'
第二位需要第一位为 '?' 或者为 '1' 时才能为 '1', 否则可以填入 '9'
第三位固定为 ':'
第四位可以为 '5'
末位可以为 '9'
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string findLatestTime(string s) 
    {
        return string(1, s[0] != '?' ? s[0] : (s[1] < '2' or s[1] == '?' ? '1' : '0')) + string(1, s[1] != '?' ? s[1] : (s[0] == '1' or s[0] == '?' ? '1' : '9')) + ":" + string(1, s[3] != '?' ? s[3] : '5') + string(1, s[4] != '?' ? s[4] : '9');
    }
};
```

__Java__:

```Java
class Solution {
    public String findLatestTime(String s) {
        return String.valueOf(s.charAt(0) != '?' ? s.charAt(0) : (s.charAt(1) < '2' || s.charAt(1) == '?' ? '1' : '0')) + String.valueOf(s.charAt(1) != '?' ? s.charAt(1) : (s.charAt(0) == '1' || s.charAt(0) == '?' ? '1' : '9')) + ":" + String.valueOf(s.charAt(3) != '?' ? s.charAt(3) : '5') + String.valueOf(s.charAt(4) != '?' ? s.charAt(4) : '9');
    }
}
```

__Python__:

```Python
class Solution:
    def findLatestTime(self, time: str) -> str:
        return (time[0] if time[0] != '?' else '1' if time[1] < '2' or time[1] == '?' else '0') + (time[1] if time[1] != '?' else '1' if time[0] == '1' or time[0] == '?' else '9') + ':' + (time[3] if time[3] != '?' else '5') + (time[4] if time[4] != '?' else '9')
```
