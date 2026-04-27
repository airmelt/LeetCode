# 1419 Minimum Number of Frogs Croaking 数青蛙

__Description:__

You are given the string `croakOfFrogs`, which represents a combination of the string `"croak"` from different frogs, that is, multiple frogs can croak at the same time, so multiple `"croak"` are mixed.

Return the minimum number of different frogs to finish all the croaks in the given string.

A valid `"croak"` means a frog is printing five letters `'c'`, `'r'`, `'o'`, `'a'`, and `'k'` __sequentially__. The frogs have to print all five letters to finish a croak. If the given string is not a combination of a valid `"croak"` return `-1`.

__Example:__

Example 1:

```text
Input: croakOfFrogs = "croakcroak"
Output: 1 
Explanation: One frog yelling "croak" twice.
```

Example 2:

```text
Input: croakOfFrogs = "crcoakroak"
Output: 2 
Explanation: The minimum number of frogs is two. 
The first frog could yell "crcoakroak".
The second frog could yell later "crcoakroak".
```

Example 3:

```text
Input: croakOfFrogs = "croakcrook"
Output: -1
Explanation: The given string is an invalid combination of "croak" from different frogs.
```

__Constraints:__

- `1 <= croakOfFrogs.length <= 10 ^ 5`
- `croakOfFrogs` is either `'c'`, `'r'`, `'o'`, `'a'`, or `'k'`.

__题目描述:__

给你一个字符串 `croakOfFrogs`，它表示不同青蛙发出的蛙鸣声（字符串 `"croak"` ）的组合。由于同一时间可以有多只青蛙呱呱作响，所以 `croakOfFrogs` 中会混合多个 `“croak”` _。_

请你返回模拟字符串中所有蛙鸣所需不同青蛙的最少数目。

要想发出蛙鸣 "croak"，青蛙必须 __依序__ 输出 `‘c’, ’r’, ’o’, ’a’, ’k’` 这 5 个字母。如果没有输出全部五个字母，那么它就不会发出声音。如果字符串 `croakOfFrogs` 不是由若干有效的 "croak" 字符混合而成，请返回 `-1` 。

__示例:__

示例 1：

```text
输入：croakOfFrogs = "croakcroak"
输出：1 
解释：一只青蛙 “呱呱” 两次
```

示例 2：

```text
输入：croakOfFrogs = "crcoakroak"
输出：2 
解释：最少需要两只青蛙，“呱呱” 声用黑体标注
第一只青蛙 "crcoakroak"
第二只青蛙 "crcoakroak"
```

示例 3：

```text
输入：croakOfFrogs = "croakcrook"
输出：-1
解释：给出的字符串不是 "croak" 的有效组合。
```

__提示：__

- `1 <= croakOfFrogs.length <= 10 ^ 5`
- 字符串中的字符只有 `'c'`, `'r'`, `'o'`, `'a'` 或者 `'k'`

__思路:__

```text
模拟
分别记录 5 种字符的出现次数
在遍历字符串时, "croak" 后出现的字符的数目不能大于前面出现的字符
可以保证 c <= r <= o <= a <= k, 所以遍历时只需要检查相邻的字符数目
最后需要检查所有字符是否都匹配完成 
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minNumberOfFrogs(string croakOfFrogs) 
    {
        int c = 0, r = 0, o = 0, a = 0, k = 0, result = 0, impossible = -1;
        for (const auto& i: croakOfFrogs) 
        {
            switch (i) 
            {
                case 'c':
                    result = max(result, ++c);
                    break;
                case 'r':
                    if (c < ++r) return impossible;
                    break;
                case 'o':
                    if (r < ++o) return impossible;
                    break;
                case 'a':
                    if (o < ++a) return impossible;
                    break;
                case 'k':
                    if (a < ++k) return impossible; 
                    --c;
                    --r;
                    --o;
                    --a;
                    --k;
                    break;
            }
        }
        return (!c and !r and !o and !a and !k) ? result : impossible;
    }
};
```

__Java__:

```Java
class Solution {
    public int minNumberOfFrogs(String croakOfFrogs) {
        int c = 0, r = 0, o = 0, a = 0, k = 0, result = 0, impossible = -1;
        for (char i: croakOfFrogs.toCharArray()) {
            switch (i) {
                case 'c':
                    result = Math.max(result, ++c);
                    break;
                case 'r':
                    if (c < ++r) return impossible;
                    break;
                case 'o':
                    if (r < ++o) return impossible;
                    break;
                case 'a':
                    if (o < ++a) return impossible;
                    break;
                case 'k':
                    if (a < ++k) return impossible; 
                    --c;
                    --r;
                    --o;
                    --a;
                    --k;
                    break;
            }
        }
        return (c == 0 && r == 0 && o == 0 && a == 0 && k == 0) ? result : impossible;
    }
}
```

__Python__:

```Python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        c, result, d = Counter(croakOfFrogs), 0, defaultdict(int)
        if len(set(c.values())) != 1:
            return -1
        for i in range(len(croakOfFrogs)):
            d[croakOfFrogs[i]] += 1
            if not d['c'] >= d['r'] >= d['o'] >= d['a'] >= d['k']:
                return -1
            if all(val > 0 for val in list(d.values())):
                for c in 'croak':
                    d[c] -= 1
            else:
                result = max(result, d['c'])
        return result
```
