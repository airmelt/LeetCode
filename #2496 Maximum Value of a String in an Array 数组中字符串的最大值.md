# 2496 Maximum Value of a String in an Array 数组中字符串的最大值

__Description:__

The value of an alphanumeric string can be defined as:

- The __numeric__ representation of the string in base `10`, if it comprises of digits __only__.
- The __length__ of the string, otherwise.

Given an array `strs` of alphanumeric strings, return _the __maximum value__ of any string in_ `strs`.

__Example:__

Example 1:

```text
Input: strs = ["alic3","bob","3","4","00000"]
Output: 5
Explanation: 
```

- "alic3" consists of both letters and digits, so its value is its length, i.e. 5.
- "bob" consists only of letters, so its value is also its length, i.e. 3.
- "3" consists only of digits, so its value is its numeric equivalent, i.e. 3.
- "4" also consists only of digits, so its value is 4.
- "00000" consists only of digits, so its value is 0.

Hence, the maximum value is 5, of "alic3".

Example 2:

```text
Input: strs = ["1","01","001","0001"]
Output: 1
Explanation: 
Each string in the array has value 1. Hence, we return 1.
```

__Constraints:__

- `1 <= strs.length <= 100`
- `1 <= strs[i].length <= 9`
- `strs[i]` consists of only lowercase English letters and digits.

__题目描述:__

一个由字母和数字组成的字符串的 值 定义如下：

- 如果字符串 __只__ 包含数字，那么值为该字符串在 `10` 进制下的所表示的数字。
- 否则，值为字符串的 __长度__ 。

给你一个字符串数组 `strs` ，每个字符串都只由字母和数字组成，请你返回 `strs` 中字符串的 __最大值__ 。

__示例:__

示例 1：

```text
输入：strs = ["alic3","bob","3","4","00000"]
输出：5
解释：
```

- "alic3" 包含字母和数字，所以值为长度 5 。
- "bob" 只包含字母，所以值为长度 3 。
- "3" 只包含数字，所以值为 3 。
- "4" 只包含数字，所以值为 4 。
- "00000" 只包含数字，所以值为 0 。

所以最大的值为 5 ，是字符串 "alic3" 的值。

示例 2：

```text
输入：strs = ["1","01","001","0001"]
输出：1
解释：
数组中所有字符串的值都是 1 ，所以我们返回 1 。
```

__提示：__

- `1 <= strs.length <= 100`
- `1 <= strs[i].length <= 9`
- `strs[i]` 只包含小写英文字母和数字。

__思路:__

```text
1. 暴力法
遍历字符串数组, 判断每个字符串是否是数字
如果全部是数字, 则将其转换为数字
如果不是, 则取其长度
返回最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 正则表达式
使用正则表达式判断字符串是否是数字
如果是数字, 则将其转换为数字
如果不是, 则取其长度
返回最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumValue(vector<string> strs) 
    {
        int result = 0;
        for (const auto& str: strs) result = max(result, all_of(str.begin(), str.end(), ::isdigit) ? stoi(str) : (int)str.size());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumValue(String[] strs) {
        int result = 0;
        for (String str : strs) result = str.matches("\\d+") ? Math.max(result, Integer.parseInt(str)) : Math.max(result, str.length());
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumValue(self, strs: List[str]) -> int:
        return max(int(i) if i.isdigit() else len(i) for i in strs)
```
