# 2716 Minimize String Length 最小化字符串长度

__Description:__

Given a string `s`, you have two types of operation:

Your task is to __minimize__ the length of `s` by performing the above operations zero or more times.

Return an integer denoting the length of the minimized string.

__Example:__

Example 1:

```text
Input: s = "aaabc"

Output: 3

Explanation:
```

1. Operation 2: we choose `i = 1` so `c` is 'a', then we remove `s[2]` as it is closest 'a' character to the right of `s[1]`.
`s` becomes "aabc" after this.
2. Operation 1: we choose `i = 1` so `c` is 'a', then we remove `s[0]` as it is closest 'a' character to the left of `s[1]`.
`s` becomes "abc" after this.

Example 2:

```text
Input: s = "cbbd"

Output: 3

Explanation:
```

1. Operation 1: we choose `i = 2` so `c` is 'b', then we remove `s[1]` as it is closest 'b' character to the left of `s[1]`.
`s` becomes "cbd" after this.

Example 3:

```text
Input: s = "baadccab"

Output: 4

Explanation:
```

1. Operation 1: we choose `i = 6` so `c` is 'a', then we remove `s[2]` as it is closest 'a' character to the left of `s[6]`.
`s` becomes "badccab" after this.
2. Operation 2: we choose `i = 0` so `c` is 'b', then we remove `s[6]` as it is closest 'b' character to the right of `s[0]`.
`s` becomes "badcca" fter this.
3. Operation 2: we choose `i = 3` so `c` is 'c', then we remove `s[4]` as it is closest 'c' character to the right of `s[3]`.
`s` becomes "badca" after this.
4. Operation 1: we choose `i = 4` so `c` is 'a', then we remove `s[1]` as it is closest 'a' character to the left of `s[4]`.
`s` becomes "bdca" after this.

__Constraints:__

- `1 <= s.length <= 100`
- `s` contains only lowercase English letters

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，重复执行下述操作 __任意__ 次:

- 在字符串中选出一个下标 `i` ，并使 `c` 为字符串下标 `i` 处的字符。并在 `i` __左侧__（如果有）和 __右侧__（如果有）各 __删除__ 一个距离 `i` __最近__ 的字符 `c` 。

请你通过执行上述操作任意次，使 `s` 的长度 __最小化__ 。

返回一个表示 最小化 字符串的长度的整数。

__示例:__

示例 1：

```text
输入：s = "aaabc"
输出：3
解释：在这个示例中，s 等于 "aaabc" 。我们可以选择位于下标 1 处的字符 'a' 开始。接着删除下标 1 左侧最近的那个 'a'（位于下标 0）以及下标 1 右侧最近的那个 'a'（位于下标 2）。执行操作后，字符串变为 "abc" 。继续对字符串执行任何操作都不会改变其长度。因此，最小化字符串的长度是 3 。
```

示例 2：

```text
输入：s = "cbbd"
输出：3
解释：我们可以选择位于下标 1 处的字符 'b' 开始。下标 1 左侧不存在字符 'b' ，但右侧存在一个字符 'b'（位于下标 2），所以会删除位于下标 2 的字符 'b' 。执行操作后，字符串变为 "cbd" 。继续对字符串执行任何操作都不会改变其长度。因此，最小化字符串的长度是 3 。
```

示例 3：

```text
输入：s = "dddaaa"
输出：2
解释：我们可以选择位于下标 1 处的字符 'd' 开始。接着删除下标 1 左侧最近的那个 'd'（位于下标 0）以及下标 1 右侧最近的那个 'd'（位于下标 2）。执行操作后，字符串变为 "daaa" 。继续对新字符串执行操作，可以选择位于下标 2 的字符 'a' 。接着删除下标 2 左侧最近的那个 'a'（位于下标 1）以及下标 2 右侧最近的那个 'a'（位于下标 3）。执行操作后，字符串变为 "da" 。继续对字符串执行任何操作都不会改变其长度。因此，最小化字符串的长度是 2 。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 仅由小写英文字母组成

__思路:__

```text
模拟
其实就是求 s 中不同字符的个数
用哈希表或者位运算即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizedStringLength(string s) 
    {
        return unordered_set(s.begin(), s.end()).size();
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    int minimizedStringLength(string s) 
    {
        return unordered_set(s.begin(), s.end()).size();
    }
};
```

__Python__:

```Python
class Solution 
{
public:
    int minimizedStringLength(string s) 
    {
        return unordered_set(s.begin(), s.end()).size();
    }
};
```
