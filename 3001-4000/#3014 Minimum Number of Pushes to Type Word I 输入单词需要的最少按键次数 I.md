# 3014 Minimum Number of Pushes to Type Word I 输入单词需要的最少按键次数 I

__Description:__

You are given a string `word` containing __distinct__ lowercase English letters.

Telephone keypads have keys mapped with __distinct__ collections of lowercase English letters, which can be used to form words by pushing them. For example, the key `2` is mapped with `["a","b","c"]`, we need to push the key one time to type `"a"`, two times to type `"b"`, and three times to type `"c"` _._

It is allowed to remap the keys numbered `2` to `9` to __distinct__ collections of letters. The keys can be remapped to __any__ amount of letters, but each letter __must__ be mapped to __exactly__ one key. You need to find the __minimum__ number of times the keys will be pushed to type the string `word`.

Return _the __minimum__ number of pushes needed to type_ `word` _after remapping the keys_.

An example mapping of letters to keys on a telephone keypad is given below. Note that `1`, `*`, `#`, and `0` do __not__ map to any letters.

![3014-1](https://assets.leetcode.com/uploads/2023/12/26/keypaddesc.png)

__Example:__

Example 1:

![3014-2](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e1.png)

```text
Input: word = "abcde"
Output: 5
Explanation: The remapped keypad given in the image provides the minimum cost.
"a" -> one push on key 2
"b" -> one push on key 3
"c" -> one push on key 4
"d" -> one push on key 5
"e" -> one push on key 6
Total cost is 1 + 1 + 1 + 1 + 1 = 5.
It can be shown that no other mapping can provide a lower cost.
```

Example 2:

![3014-3](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e2.png)

```text
Input: word = "xycdefghij"
Output: 12
Explanation: The remapped keypad given in the image provides the minimum cost.
"x" -> one push on key 2
"y" -> two pushes on key 2
"c" -> one push on key 3
"d" -> two pushes on key 3
"e" -> one push on key 4
"f" -> one push on key 5
"g" -> one push on key 6
"h" -> one push on key 7
"i" -> one push on key 8
"j" -> one push on key 9
Total cost is 1 + 2 + 1 + 2 + 1 + 1 + 1 + 1 + 1 + 1 = 12.
It can be shown that no other mapping can provide a lower cost.
```

__Constraints:__

- `1 <= word.length <= 26`
- `word` consists of lowercase English letters.
- All letters in `word` are distinct.

__题目描述:__

给你一个字符串 `word`，由 __不同__ 小写英文字母组成。

电话键盘上的按键与 __不同__ 小写英文字母集合相映射，可以通过按压按键来组成单词。例如，按键 `2` 对应 `["a","b","c"]`，我们需要按一次键来输入 `"a"`，按两次键来输入 `"b"`，按三次键来输入 `"c"`_。_

现在允许你将编号为 `2` 到 `9` 的按键重新映射到 __不同__ 字母集合。每个按键可以映射到 __任意数量__ 的字母，但每个字母 __必须__ __恰好__ 映射到 __一个__ 按键上。你需要找到输入字符串 `word` 所需的 __最少__ 按键次数。

返回重新映射按键后输入 `word` 所需的 __最少__ 按键次数。

__示例:__

下面给出了一种电话键盘上字母到按键的映射作为示例。注意 `1`， `*`， `#` 和 `0` __不__ 对应任何字母。

![3014-4](https://assets.leetcode.com/uploads/2023/12/26/keypaddesc.png)

示例 1：

![3014-5](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e1.png)

```text
输入：word = "abcde"
输出：5
解释：图片中给出的重新映射方案的输入成本最小。
"a" -> 在按键 2 上按一次
"b" -> 在按键 3 上按一次
"c" -> 在按键 4 上按一次
"d" -> 在按键 5 上按一次
"e" -> 在按键 6 上按一次
总成本为 1 + 1 + 1 + 1 + 1 = 5 。
可以证明不存在其他成本更低的映射方案。
```

示例 2：

![3014-6](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e2.png)

```text
输入：word = "xycdefghij"
输出：12
解释：图片中给出的重新映射方案的输入成本最小。
"x" -> 在按键 2 上按一次
"y" -> 在按键 2 上按两次
"c" -> 在按键 3 上按一次
"d" -> 在按键 3 上按两次
"e" -> 在按键 4 上按一次
"f" -> 在按键 5 上按一次
"g" -> 在按键 6 上按一次
"h" -> 在按键 7 上按一次
"i" -> 在按键 8 上按一次
"j" -> 在按键 9 上按一次
总成本为 1 + 2 + 1 + 2 + 1 + 1 + 1 + 1 + 1 + 1 = 12 。
可以证明不存在其他成本更低的映射方案。
```

__提示：__

- `1 <= word.length <= 26`
- `word` 仅由小写英文字母组成。
- `word` 中的所有字母互不相同。

__思路:__

```text
贪心
先将所有字母都映射到 2-9
不够的字母再依次映射
所以如果字符串的长度小于 9
一次按键就能搞定
其余情况依次递推即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumPushes(string word) 
    {
        return word.size() < 9 ?  word.size() : (word.size() < 17 ? 8 + ((word.size() - 8) << 1) : (word.size() < 25 ? 24 + ((word.size() - 16) * 3) : 48 + ((word.size() - 24) << 2)));
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumPushes(String word) {
        return word.length() < 9 ?  word.length() : (word.length() < 17 ? 8 + ((word.length() - 8) << 1) : (word.length() < 25 ? 24 + ((word.length() - 16) * 3) : 48 + ((word.length() - 24) << 2)));
    }
}
```

__Python__:

```Python
class Solution:
    def minimumPushes(self, word: str) -> int:
        return len(word) if len(word) < 9 else 8 + ((len(word) - 8) << 1) if len(word) < 17 else 24 + ((len(word) - 16) * 3) if len(word) < 25 else 48 + ((len(word) - 24) << 2)
```
