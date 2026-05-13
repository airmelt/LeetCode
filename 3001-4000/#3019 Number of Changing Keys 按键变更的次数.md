# 3019 Number of Changing Keys 按键变更的次数

__Description:__

You are given a __0-indexed__ string `s` typed by a user. Changing a key is defined as using a key different from the last used key. For example, `s = "ab"` has a change of a key while `s = "bBBb"` does not have any.

Return the number of times the user had to change the key.

__Note:__ Modifiers like `shift` or `caps lock` won't be counted in changing the key that is if a user typed the letter `'a'` and then the letter `'A'` then it will not be considered as a changing of key.

__Example:__

Example 1:

```text
Input: s = "aAbBcC"
Output: 2
Explanation: 
From s[0] = 'a' to s[1] = 'A', there is no change of key as caps lock or shift is not counted.
From s[1] = 'A' to s[2] = 'b', there is a change of key.
From s[2] = 'b' to s[3] = 'B', there is no change of key as caps lock or shift is not counted.
From s[3] = 'B' to s[4] = 'c', there is a change of key.
From s[4] = 'c' to s[5] = 'C', there is no change of key as caps lock or shift is not counted.
```

Example 2:

```text
Input: s = "AaAaAaaA"
Output: 0
Explanation: There is no change of key since only the letters 'a' and 'A' are pressed which does not require change of key.
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists of only upper case and lower case English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，该字符串由用户输入。按键变更的定义是:使用与上次使用的按键不同的键。例如 `s = "ab"` 表示按键变更一次，而 `s = "bBBb"` 不存在按键变更。

返回用户输入过程中按键变更的次数。

__注意:__`shift` 或 `caps lock` 等修饰键不计入按键变更，也就是说，如果用户先输入字母 `'a'` 然后输入字母 `'A'` ，不算作按键变更。

__示例:__

示例 1：

```text
输入：s = "aAbBcC"
输出：2
解释： 
从 s[0] = 'a' 到 s[1] = 'A'，不存在按键变更，因为不计入 caps lock 或 shift 。
从 s[1] = 'A' 到 s[2] = 'b'，按键变更。
从 s[2] = 'b' 到 s[3] = 'B'，不存在按键变更，因为不计入 caps lock 或 shift 。
从 s[3] = 'B' 到 s[4] = 'c'，按键变更。
从 s[4] = 'c' 到 s[5] = 'C'，不存在按键变更，因为不计入 caps lock 或 shift 。
```

示例 2：

```text
输入：s = "AaAaAaaA"
输出：0
解释： 不存在按键变更，因为这个过程中只按下字母 'a' 和 'A' ，不需要进行按键变更。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 仅由英文大写字母和小写字母组成。

__思路:__

```text
模拟
找到相邻元素值不相等的情况
使用 ASCII 码比较的时候只需要比较后 5 位
所以可以和 31 进行与运算来比较
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countKeyChanges(string s) 
    {
        int result = 0;
        for (int i = 1, n = s.size(); i < n; i++) result += (s[i - 1] & 31) != (s[i] & 31);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countKeyChanges(String s) {
        return (int)IntStream.range(1, s.length()).filter(i -> (s.charAt(i - 1) & 31) != (s.charAt(i) & 31)).count();
    }
}
```

__Python__:

```Python
class Solution:
    def countKeyChanges(self, s: str) -> int:
        return sum(x != y for x, y in pairwise(s.lower()))
```
