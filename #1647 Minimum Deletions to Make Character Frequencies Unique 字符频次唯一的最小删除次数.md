# 1647 Minimum Deletions to Make Character Frequencies Unique 字符频次唯一的最小删除次数

__Description:__

A string `s` is called __good__ if there are no two different characters in `s` that have the same __frequency__.

Given a string `s`, return _the __minimum__ number of characters you need to delete to make_ `s` ___good__._

The __frequency__ of a character in a string is the number of times it appears in the string. For example, in the string `"aab"`, the __frequency__ of `'a'` is `2`, while the __frequency__ of `'b'` is `1`.

__Example:__

Example 1:

```text
Input:  s = "aab"
Output:  0
Explanation:  `s` is already good.
```

Example 2:

```text
Input: s = "aaabbbcc"
Output: 2
Explanation: You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".
```

Example 3:

```text
Input: s = "ceabaacb"
Output: 2
Explanation: You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` contains only lowercase English letters.

__题目描述:__

如果字符串 `s` 中 __不存在__ 两个不同字符 __频次__ 相同的情况，就称 `s` 是 __优质字符串__ 。

给你一个字符串 `s`，返回使 `s` 成为 __优质字符串__ 需要删除的 __最小__ 字符数。

字符串中字符的 __频次__ 是该字符在字符串中的出现次数。例如，在字符串 `"aab"` 中， `'a'` 的频次是 `2`，而 `'b'` 的频次是 `1` 。

__示例:__

示例 1：

```text
输入: s = "aab"
输出: 0
解释: `s` 已经是优质字符串。
```

示例 2：

```text
输入：s = "aaabbbcc"
输出：2
解释：可以删除两个 'b' , 得到优质字符串 "aaabcc" 。
另一种方式是删除一个 'b' 和一个 'c' ，得到优质字符串 "aaabbc" 。
```

示例 3：

```text
输入：s = "ceabaacb"
输出：2
解释：可以删除两个 'c' 得到优质字符串 "eabaab" 。
注意，只需要关注结果字符串中仍然存在的字符。（即，频次为 0 的字符会忽略不计。）
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 仅含小写英文字母

__思路:__

```text
计数
使用数组或者哈希表计数
将数组出现频次由大到小排序
如果较小的值没有减到 0 而且还比较大值大或者相等就自减
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minDeletions(string s) 
    {
        vector<int> count(26);
        int result = 0;
        for (const auto& c : s) ++count[c - 'a'];
        sort(count.rbegin(), count.rend());
        for (int i = 1; i < 26; i++) while (count[i] and count[i - 1] <= count[i]) 
        {
            ++result;
            --count[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minDeletions(String s) {
        int count[] = new int[26], result = 0, n = s.length();
        for (char c : s.toCharArray()) ++count[c - 'a'];
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < 26; i++) while (count[i]-- != 0 && !set.add(count[i] + 1)) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minDeletions(self, s: str) -> int:
        n, result = len(s := sorted(list(Counter(s).values()), reverse=True)), 0 
        for i in range(1, n):
            if s[i] >= s[i - 1]:
                if s[i - 1] > 0:
                    result += s[i] - s[i - 1] + 1
                    s[i] = s[i - 1] - 1
                else:
                    result += s[i]
                    s[i] = 0
        return result
```
