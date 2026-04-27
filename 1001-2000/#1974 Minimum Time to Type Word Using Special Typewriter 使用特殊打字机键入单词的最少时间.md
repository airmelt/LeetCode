# 1974 Minimum Time to Type Word Using Special Typewriter 使用特殊打字机键入单词的最少时间

__Description:__

There is a special typewriter with lowercase English letters `'a'` to `'z'` arranged in a __circle__ with a __pointer__. A character can __only__ be typed if the pointer is pointing to that character. The pointer is __initially__ pointing to the character `'a'`.

![1974-1](https://assets.leetcode.com/uploads/2021/07/31/chart.jpg)

Each second, you may perform one of the following operations:

- Move the pointer one character __counterclockwise__ or __clockwise__.
- Type the character the pointer is __currently__ on.

Given a string `word`, return the __minimum__ number of seconds to type out the characters in `word`.

__Example:__

Example 1:

```text
Input: word = "abc"
Output: 5
Explanation: 
The characters are printed as follows:
```

- Type the character 'a' in 1 second since the pointer is initially on 'a'.
- Move the pointer clockwise to 'b' in 1 second.
- Type the character 'b' in 1 second.
- Move the pointer clockwise to 'c' in 1 second.
- Type the character 'c' in 1 second.

Example 2:

```text
Input: word = "bza"
Output: 7
Explanation:
The characters are printed as follows:
```

- Move the pointer clockwise to 'b' in 1 second.
- Type the character 'b' in 1 second.
- Move the pointer counterclockwise to 'z' in 2 seconds.
- Type the character 'z' in 1 second.
- Move the pointer clockwise to 'a' in 1 second.
- Type the character 'a' in 1 second.

Example 3:

```text
Input: word = "zjpc"
Output: 34
Explanation:
The characters are printed as follows:
```

- Move the pointer counterclockwise to 'z' in 1 second.
- Type the character 'z' in 1 second.
- Move the pointer clockwise to 'j' in 10 seconds.
- Type the character 'j' in 1 second.
- Move the pointer clockwise to 'p' in 6 seconds.
- Type the character 'p' in 1 second.
- Move the pointer counterclockwise to 'c' in 13 seconds.
- Type the character 'c' in 1 second.

__Constraints:__

- `1 <= word.length <= 100`
- `word` consists of lowercase English letters.

__题目描述:__

有一个特殊打字机，它由一个 __圆盘__ 和一个 __指针__ 组成， 圆盘上标有小写英文字母 `'a'` 到 `'z'`。__只有__ 当指针指向某个字母时，它才能被键入。指针 __初始时__ 指向字符 `'a'` 。

![1974-2](https://assets.leetcode.com/uploads/2021/07/31/chart.jpg)

每一秒钟，你可以执行以下操作之一：

- 将指针 __顺时针__ 或者 _逆时针_ 移动一个字符。
- 键入指针 __当前__ 指向的字符。

给你一个字符串 `word` ，请你返回键入 `word` 所表示单词的 _最少_ 秒数 。

__示例:__

示例 1：

```text
输入：word = "abc"
输出：5
解释：
单词按如下操作键入：
```

- 花 1 秒键入字符 'a' in 1 ，因为指针初始指向 'a' ，故不需移动指针。
- 花 1 秒将指针顺时针移到 'b' 。
- 花 1 秒键入字符 'b' 。
- 花 1 秒将指针顺时针移到 'c' 。
- 花 1 秒键入字符 'c' 。

示例 2：

```text
输入：word = "bza"
输出：7
解释：
单词按如下操作键入：
```

- 花 1 秒将指针顺时针移到 'b' 。
- 花 1 秒键入字符 'b' 。
- 花 2 秒将指针逆时针移到 'z' 。
- 花 1 秒键入字符 'z' 。
- 花 1 秒将指针顺时针移到 'a' 。
- 花 1 秒键入字符 'a' 。

示例 3：

```text
输入：word = "zjpc"
输出：34
解释：
单词按如下操作键入：
```

- 花 1 秒将指针逆时针移到 'z' 。
- 花 1 秒键入字符 'z' 。
- 花 10 秒将指针顺时针移到 'j' 。
- 花 1 秒键入字符 'j' 。
- 花 6 秒将指针顺时针移到 'p' 。
- 花 1 秒键入字符 'p' 。
- 花 13 秒将指针逆时针移到 'c' 。
- 花 1 秒键入字符 'c' 。

__提示：__

- `1 <= word.length <= 100`
- `word` 只包含小写英文字母。

__思路:__

```text
模拟
从 'a' 开始, 每次移动指针到下一个字符, 然后计算移动的时间
分别从顺时针和逆时针两个方向计算移动的时间, 取最小值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minTimeToType(string word) 
    {
        int pre = 'a', result = 0, n = word.length();
        for (int i = 0; i < n; i++) 
        {
            result += min(abs(word[i] - pre), 26 - abs(word[i] - pre)) + 1;
            pre = word[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minTimeToType(String word) {
        int pre = 'a', result = 0, n = word.length();
        for (int i = 0; i < n; i++) {
            result += Math.min(Math.abs(word.charAt(i) - pre), 26 - Math.abs(word.charAt(i) - pre)) + 1;
            pre = word.charAt(i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minTimeToType(self, word: str) -> int:
        return sum(min(abs(ord(cur) - ord(pre)), 26 - abs(ord(pre) - ord(cur))) + 1 for pre, cur in pairwise('a' + word))
```
