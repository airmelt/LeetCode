# 1324 Print Words Vertically 竖直打印单词

__Description:__

Given a string s. Return all the words vertically in the same order in which they appear in s.
Words are returned as a list of strings, complete with spaces when is necessary. (Trailing spaces are not allowed).
Each word would be put on only one column and that in one column there will be only one word.

__Example:__

Example 1:

Input: s = "HOW ARE YOU"
Output: ["HAY","ORO","WEU"]
Explanation: Each word is printed vertically.
 "HAY"
 "ORO"
 "WEU"

Example 2:

Input: s = "TO BE OR NOT TO BE"
Output: ["TBONTB","OEROOE","   T"]
Explanation: Trailing spaces is not allowed.
"TBONTB"
"OEROOE"
"   T"

Example 3:

Input: s = "CONTEST IS COMING"
Output: ["CIC","OSO","N M","T I","E N","S G","T"]

__Constraints:__

1 <= s.length <= 200
s contains only upper case English letters.
It's guaranteed that there is only one space between 2 words.

__题目描述：__

给你一个字符串 s。请你按照单词在 s 中的出现顺序将它们全部竖直返回。
单词应该以字符串列表的形式返回，必要时用空格补位，但输出尾部的空格需要删除（不允许尾随空格）。
每个单词只能放在一列上，每一列中也只能有一个单词。

__示例：__

示例 1：

输入：s = "HOW ARE YOU"
输出：["HAY","ORO","WEU"]
解释：每个单词都应该竖直打印。
 "HAY"
 "ORO"
 "WEU"

示例 2：

输入：s = "TO BE OR NOT TO BE"
输出：["TBONTB","OEROOE","   T"]
解释：题目允许使用空格补位，但不允许输出末尾出现空格。
"TBONTB"
"OEROOE"
"   T"

示例 3：

输入：s = "CONTEST IS COMING"
输出：["CIC","OSO","N M","T I","E N","S G","T"]

__提示：__

1 <= s.length <= 200
s 仅含大写英文字母。
题目数据保证两个单词之间只有一个空格。

__思路：__

模拟
先找到所有单词的最长长度
然后按照题目意思将单词按顺序插入结果中即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<string> printVertically(string s) 
    {
        vector<string> result;
        vector<vector<char>> v(210, vector<char>(210, ' '));
        int max_size = 0, j = 0, n = s.size();
        for (int i = 0, k = 0; i < n; i++) 
        {
            if (s[i] == ' ') 
            {
                ++j;
                k = 0;
            }
            else 
            {
                v[j][k] = s[i];
                ++k;
            }
            max_size = max(max_size, k);
        }
        for (int i = 0; i < max_size; i++) 
        {
            int k = j;
            while (k >= 0 and v[k][i] == ' ') --k;
            string cur;
            for (int m = 0; m <= k; ++m) cur += v[m][i];
            result.emplace_back(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> printVertically(String s) {
        List<String> result = new ArrayList<>();
        String[] strs = s.split(" ");
        int count = 0;
        while (true) {
            StringBuilder sb = new StringBuilder(), blank = new StringBuilder();
            for (String str : strs) {
                if (count < str.length()) {
                    sb.append(blank).append(str.charAt(count));
                    blank.setLength(0);
                } else blank.append(' ');
            }
            if (sb.isEmpty()) break;
            ++count;
            result.add(sb.toString());
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def printVertically(self, s: str) -> List[str]:
        return [''.join(i).rstrip() for i in zip_longest(*s.split(), fillvalue=' ')]
```
