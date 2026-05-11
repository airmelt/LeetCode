# 3016 Minimum Number of Pushes to Type Word II 输入单词需要的最少按键次数 II

__Description:__

You are given a string `word` containing lowercase English letters.

Telephone keypads have keys mapped with __distinct__ collections of lowercase English letters, which can be used to form words by pushing them. For example, the key `2` is mapped with `["a","b","c"]`, we need to push the key one time to type `"a"`, two times to type `"b"`, and three times to type `"c"` _._

It is allowed to remap the keys numbered `2` to `9` to __distinct__ collections of letters. The keys can be remapped to __any__ amount of letters, but each letter __must__ be mapped to __exactly__ one key. You need to find the __minimum__ number of times the keys will be pushed to type the string `word`.

Return _the __minimum__ number of pushes needed to type_ `word` _after remapping the keys_.

An example mapping of letters to keys on a telephone keypad is given below. Note that `1`, `*`, `#`, and `0` do __not__ map to any letters.

![3016-1](https://assets.leetcode.com/uploads/2023/12/26/keypaddesc.png)

__Example:__

Example 1:

![3016-2](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e1.png)

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

![3016-3](https://assets.leetcode.com/uploads/2024/08/20/edited.png)

```text
Input: word = "xyzxyzxyzxyz"
Output: 12
Explanation: The remapped keypad given in the image provides the minimum cost.
"x" -> one push on key 2
"y" -> one push on key 3
"z" -> one push on key 4
Total cost is 1 * 4 + 1 * 4 + 1 * 4 = 12
It can be shown that no other mapping can provide a lower cost.
Note that the key 9 is not mapped to any letter: it is not necessary to map letters to every key, but to map all the letters.
```

Example 3:

![3016-4](https://assets.leetcode.com/uploads/2023/12/27/keypadv2.png)

```text
Input: word = "aabbccddeeffgghhiiiiii"
Output: 24
Explanation: The remapped keypad given in the image provides the minimum cost.
"a" -> one push on key 2
"b" -> one push on key 3
"c" -> one push on key 4
"d" -> one push on key 5
"e" -> one push on key 6
"f" -> one push on key 7
"g" -> one push on key 8
"h" -> two pushes on key 9
"i" -> one push on key 9
Total cost is 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 2 * 2 + 6 * 1 = 24.
It can be shown that no other mapping can provide a lower cost.
```

__Constraints:__

- `1 <= word.length <= 10 ^ 5`
- `word` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `word`，由小写英文字母组成。

电话键盘上的按键与 __不同__ 小写英文字母集合相映射，可以通过按压按键来组成单词。例如，按键 `2` 对应 `["a","b","c"]`，我们需要按一次键来输入 `"a"`，按两次键来输入 `"b"`，按三次键来输入 `"c"`_。_

现在允许你将编号为 `2` 到 `9` 的按键重新映射到 __不同__ 字母集合。每个按键可以映射到 __任意数量__ 的字母，但每个字母 __必须__ __恰好__ 映射到 __一个__ 按键上。你需要找到输入字符串 `word` 所需的 __最少__ 按键次数。

返回重新映射按键后输入 `word` 所需的 __最少__ 按键次数。

__示例:__

下面给出了一种电话键盘上字母到按键的映射作为示例。注意 `1`， `*`， `#` 和 `0` __不__ 对应任何字母。

![3016-5](https://assets.leetcode.com/uploads/2023/12/26/keypaddesc.png)

示例 1：

![3016-6](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e1.png)

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

![3016-7](https://assets.leetcode.com/uploads/2023/12/26/keypadv2e2.png)

```text
输入：word = "xyzxyzxyzxyz"
输出：12
解释：图片中给出的重新映射方案的输入成本最小。
"x" -> 在按键 2 上按一次
"y" -> 在按键 3 上按一次
"z" -> 在按键 4 上按一次
总成本为 1 * 4 + 1 * 4 + 1 * 4 = 12 。
可以证明不存在其他成本更低的映射方案。
注意按键 9 没有映射到任何字母：不必让每个按键都存在与之映射的字母，但是每个字母都必须映射到按键上。
```

示例 3：

![3016-8](https://assets.leetcode.com/uploads/2023/12/27/keypadv2.png)

```text
输入：word = "aabbccddeeffgghhiiiiii"
输出：24
解释：图片中给出的重新映射方案的输入成本最小。
"a" -> 在按键 2 上按一次
"b" -> 在按键 3 上按一次
"c" -> 在按键 4 上按一次
"d" -> 在按键 5 上按一次
"e" -> 在按键 6 上按一次
"f" -> 在按键 7 上按一次
"g" -> 在按键 8 上按一次
"h" -> 在按键 9 上按两次
"i" -> 在按键 9 上按一次
总成本为 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 2 * 2 + 6 * 1 = 24 。
可以证明不存在其他成本更低的映射方案。
```

__提示：__

- `1 <= word.length <= 10 ^ 5`
- `word` 仅由小写英文字母组成。

__思路:__

```text
贪心
统计字符个数
由大到小排序
将出现最多的字符安排在前面
每出现 8 次就需要多加一次输入
所以统计字符出现次数和顺序除以 8 加 1 的乘积即可
时间复杂度为 O(N), 空间复杂度为 O(1), 实际上需要字符数的空间, 本题为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumPushes(string word) 
    {
        int result = 0;
        vector<int> cnt(26);
        for (const auto& c : word) ++cnt[c - 'a'];
        sort(cnt.begin(), cnt.end());
        for (int i = 0; i < 26; i++) result += cnt[25 - i] * ((i >> 3) + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumPushes(String word) {
        int result = 0, cnt[] = new int[26];
        for (var c : word.toCharArray()) ++cnt[c - 'a'];
        Arrays.sort(cnt);
        for (int i = 0; i < 26; i++) result += cnt[25 - i] * ((i >> 3) + 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumPushes(self, word: str) -> int:
        return sum(c * ((i >> 3) + 1) for i, c in enumerate(a)) if (a := sorted(Counter(word).values(), reverse=True)) else 0
```
