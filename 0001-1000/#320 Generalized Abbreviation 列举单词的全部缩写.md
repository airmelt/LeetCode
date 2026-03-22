# 320 Generalized Abbreviation 列举单词的全部缩写

__Description:__

A word's generalized abbreviation can be constructed by taking any number of non-overlapping and non-adjacent substrings and replacing them with their respective lengths.

- For example, `"abcde"` can be abbreviated into:

  - `"a3e"` ( `"bcd"` turned into `"3"`)
  - `"1bcd1"` ( `"a"` and `"e"` both turned into `"1"`)
  - `"5"` ( `"abcde"` turned into `"5"`)
  - `"abcde"` (no substrings replaced)

- However, these abbreviations are __invalid__

  - `"23"` ( `"ab"` turned into `"2"` and `"cde"` turned into `"3"`) is invalid as the substrings chosen are adjacent.
  - `"22de"` ( `"ab"` turned into `"2"` and `"bc"` turned into `"2"`) is invalid as the substring chosen overlap.

Given a string `word`, return _a list of all the possible __generalized abbreviations__ of_ `word`. Return the answer in __any order__.

__Example:__

Example 1:

```text
Input: word = "word"
Output: ["4","3d","2r1","2rd","1o2","1o1d","1or1","1ord","w3","w2d","w1r1","w1rd","wo2","wo1d","wor1","word"]
```

Example 2:

```text
Input: word = "a"
Output: ["1","a"]
```

__Constraints:__

- `1 <= word.length <= 15`
- `word` consists of only lowercase English letters.

__题目描述:__

单词的 广义缩写词 可以通过下述步骤构造：先取任意数量的 不重叠、不相邻 的子字符串，再用它们各自的长度进行替换。

- 例如， `"abcde"` 可以缩写为:

  - `"a3e"`（ `"bcd"` 变为 `"3"` ）
  - `"1bcd1"`（ `"a"` 和 `"e"` 都变为 `"1"`）
  - `"5"` ( `"abcde"` 变为 `"5"`)
  - `"abcde"` (没有子字符串被代替)

- 然而，这些缩写是 __无效的__:

  - `"23"`（ `"ab"` 变为 `"2"` ， `"cde"` 变为 `"3"` ）是无效的，因为被选择的字符串是相邻的
  - `"22de"` ( `"ab"` 变为 `"2"` ， `"bc"` 变为 `"2"`) 是无效的，因为被选择的字符串是重叠的

给你一个字符串 `word` ，返回 _一个由_ `word` 的_所有可能 __广义缩写词__ 组成的列表_ 。按 __任意顺序__ 返回答案。

__示例:__

示例 1：

```text
输入：word = "word"
输出：["4","3d","2r1","2rd","1o2","1o1d","1or1","1ord","w3","w2d","w1r1","w1rd","wo2","wo1d","wor1","word"]
```

示例 2：

```text
输入：word = "a"
输出：["1","a"]
```

__提示：__

- `1 <= word.length <= 15`
- `word` 仅由小写英文字母组成

__思路:__

```text
1. 回溯
每次回溯
要么选择连续的字符缩写
要么重新开始新的一段字符进行缩写
时间复杂度为 O(N * 2 ^ N), 空间复杂度为 O(N)
2. 位运算
比如 abcd 可以转化为 0000
abc1 -> 0001
ab2  -> 0011
即把连续的 1 转化为缩写
每次计算连续的 1 的长度转化为缩写
时间复杂度为 O(N * 2 ^ N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> generateAbbreviations(string word) 
    {
        int n = word.size(), end = 1 << n;
        vector<string> result(end);
        for (int mask = 0, cur = 0; mask < end; mask++, cur = 0) 
        {
            while (cur < n) 
            {
                if (!(mask & (1 << cur))) result[mask].push_back(word[cur++]);
                else 
                {
                    int pos = cur;
                    while (mask & (1 << pos)) ++pos;
                    result[mask].append(to_string(pos - cur));
                    cur = pos;
                }
            }
        }        
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private List<String> result = new ArrayList<>();
    private String word;
    
    public List<String> generateAbbreviations(String word) {
        this.word = word;
        dfs(0, "", 0);
        return result;
    }
    
    private void dfs(int start, String s, int cur) {
        if (start == word.length()) {
            result.add(s + (cur != 0 ? cur : ""));
            return;
        }
        dfs(start + 1, s, cur + 1);
        dfs(start + 1, s + (cur != 0 ? cur : "") + word.charAt(start), 0);
    }
}
```

__Python__:

```Python
class Solution:
    def generateAbbreviations(self, word: str) -> List[str]:
        return ["".join(str(len([*g])) if type(x) is int else x * len([*g]) for x, g in groupby([[1, word[i]][(mask >> i) & 1] for i in range(len(word))]))  for mask in range(1 << len(word))]
```
