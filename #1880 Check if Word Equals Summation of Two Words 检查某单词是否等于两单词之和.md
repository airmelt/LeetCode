# 1880 Check if Word Equals Summation of Two Words 检查某单词是否等于两单词之和

__Description:__

The __letter value__ of a letter is its position in the alphabet __starting from 0__ (i.e. `'a' -> 0`, `'b' -> 1`, `'c' -> 2`, etc.).

The __numerical value__ of some string of lowercase English letters `s` is the __concatenation__ of the __letter values__ of each letter in `s`, which is then __converted__ into an integer.

- For example, if `s = "acb"`, we concatenate each letter's letter value, resulting in `"021"`. After converting it, we get `21`.

You are given three strings `firstWord`, `secondWord`, and `targetWord`, each consisting of lowercase English letters `'a'` through `'j'` __inclusive__.

Return `true` _if the __summation__ of the __numerical values__ of_ `firstWord` _and_ `secondWord` _equals the __numerical value__ of_ `targetWord`_, or_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: firstWord = "acb", secondWord = "cba", targetWord = "cdb"
Output: true
Explanation:
The numerical value of firstWord is "acb" -> "021" -> 21.
The numerical value of secondWord is "cba" -> "210" -> 210.
The numerical value of targetWord is "cdb" -> "231" -> 231.
We return true because 21 + 210 == 231.
```

Example 2:

```text
Input: firstWord = "aaa", secondWord = "a", targetWord = "aab"
Output: false
Explanation: 
The numerical value of firstWord is "aaa" -> "000" -> 0.
The numerical value of secondWord is "a" -> "0" -> 0.
The numerical value of targetWord is "aab" -> "001" -> 1.
We return false because 0 + 0 != 1.
```

Example 3:

```text
Input: firstWord = "aaa", secondWord = "a", targetWord = "aaaa"
Output: true
Explanation: 
The numerical value of firstWord is "aaa" -> "000" -> 0.
The numerical value of secondWord is "a" -> "0" -> 0.
The numerical value of targetWord is "aaaa" -> "0000" -> 0.
We return true because 0 + 0 == 0.
```

__Constraints:__

- `1 <= firstWord.length,` `secondWord.length,` `targetWord.length <= 8`
- `firstWord`, `secondWord`, and `targetWord` consist of lowercase English letters from `'a'` to `'j'` __inclusive__.

__题目描述:__

字母的 __字母值__ 取决于字母在字母表中的位置，__从 0 开始__ 计数。即， `'a' -> 0`、 `'b' -> 1`、 `'c' -> 2`，以此类推。

对某个由小写字母组成的字符串 `s` 而言，其 __数值__ 就等于将 `s` 中每个字母的 __字母值__ 按顺序 __连接__ 并 __转换__ 成对应整数。

- 例如， `s = "acb"` ，依次连接每个字母的字母值可以得到 `"021"` ，转换为整数得到 `21` 。

给你三个字符串 `firstWord`、 `secondWord` 和 `targetWord` ，每个字符串都由从 `'a'` 到 `'j'` （__含__ `'a'` 和 `'j'` ）的小写英文字母组成。

如果 `firstWord` 和 `secondWord` 的 __数值之和__ 等于 `targetWord` 的数值，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：firstWord = "acb", secondWord = "cba", targetWord = "cdb"
输出：true
解释：
firstWord 的数值为 "acb" -> "021" -> 21
secondWord 的数值为 "cba" -> "210" -> 210
targetWord 的数值为 "cdb" -> "231" -> 231
由于 21 + 210 == 231 ，返回 true
```

示例 2：

```text
输入：firstWord = "aaa", secondWord = "a", targetWord = "aab"
输出：false
解释：
firstWord 的数值为 "aaa" -> "000" -> 0
secondWord 的数值为 "a" -> "0" -> 0
targetWord 的数值为 "aab" -> "001" -> 1
由于 0 + 0 != 1 ，返回 false
```

示例 3：

```text
输入：firstWord = "aaa", secondWord = "a", targetWord = "aaaa"
输出：true
解释：
firstWord 的数值为 "aaa" -> "000" -> 0
secondWord 的数值为 "a" -> "0" -> 0
targetWord 的数值为 "aaaa" -> "0000" -> 0
由于 0 + 0 == 0 ，返回 true
```

__提示：__

- `1 <= firstWord.length,` `secondWord.length,` `targetWord.length <= 8`
- `firstWord`、 `secondWord` 和 `targetWord` 仅由从 `'a'` 到 `'j'` （__含__ `'a'` 和 `'j'` ）的小写英文字母组成。

__思路:__

```text
模拟
简单将字符转换成对应数字即可
由于字符的范围为 'a' - 'j'
所以对应的数字大小为 [0, 9]
又由于长度不大于 8
所以直接转换成数字不会超过范围
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isSumEqual(string firstWord, string secondWord, string targetWord, int (*func)(const string&) = [](const string& s){return accumulate(s.begin(), s.end(), 0, [](const int& sum, const int& next){return sum * 10 + next - 'a';});}) 
    {
        return func(firstWord) + func(secondWord) == func(targetWord);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isSumEqual(String firstWord, String secondWord, String targetWord) {
        return Integer.valueOf(firstWord.replace('a', '0').replace('b', '1').replace('c', '2').replace('d', '3').replace('e', '4').replace('f', '5').replace('g', '6').replace('h', '7').replace('i', '8').replace('j', '9')) + Integer.valueOf(secondWord.replace('a', '0').replace('b', '1').replace('c', '2').replace('d', '3').replace('e', '4').replace('f', '5').replace('g', '6').replace('h', '7').replace('i', '8').replace('j', '9')) == Integer.valueOf(targetWord.replace('a', '0').replace('b', '1').replace('c', '2').replace('d', '3').replace('e', '4').replace('f', '5').replace('g', '6').replace('h', '7').replace('i', '8').replace('j', '9'));

    }
}
```

__Python__:

```Python
class Solution:
    def isSumEqual(self, firstWord: str, secondWord: str, targetWord: str) -> bool:
        return (f := lambda word: int(''.join([str(ord(w) - ord('a')) for w in word]))) and f(firstWord) + f(secondWord) == f(targetWord)
```
