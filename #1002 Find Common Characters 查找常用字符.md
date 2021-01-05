# 1002 Find Common Characters 查找常用字符

__Description__:
Given an array A of strings made only from lowercase letters, return a list of all characters that show up in all strings within the list (including duplicates).  For example, if a character occurs 3 times in all strings but not 4 times, you need to include that character three times in the final answer.

You may return the answer in any order.

__Example:__

Example 1:

Input: ["bella","label","roller"]
Output: ["e","l","l"]

Example 2:

Input: ["cool","lock","cook"]
Output: ["c","o"]

__Note:__

1 <= A.length <= 100
1 <= A[i].length <= 100
A[i][j] is a lowercase letter

__题目描述__:
给定仅有小写字母组成的字符串数组 A，返回列表中的每个字符串中都显示的全部字符（包括重复字符）组成的列表。例如，如果一个字符在每个字符串中出现 3 次，但不是 4 次，则需要在最终答案中包含该字符 3 次。

你可以按任意顺序返回答案。

__示例 :__

示例 1：

输入：["bella","label","roller"]
输出：["e","l","l"]

示例 2：

输入：["cool","lock","cook"]
输出：["c","o"]

__提示：__

1 <= A.length <= 100
1 <= A[i].length <= 100
A[i][j] 是小写字母

__思路__:

用一个长度为 26的数组记录下各个字符出现的次数, 输出重复出现的最小的出现次数的字符即可
时间复杂度O(mn), 空间复杂度O(1), m为 A数组的字符串长度, n为 A数组的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> commonChars(vector<string>& A) 
    {
        vector<string> result;
        int count[26]{0};
        for (auto c : A[0]) ++count[c - 'a'];
        for (int i = 1; i < A.size(); i++) 
        {
            int temp[26]{0};
            for (auto c : A[i]) ++temp[c - 'a'];
            for (int j = 0; j < 26; j++) count[j] = min(count[j], temp[j]);
        }
        for (int i = 0; i < 26; i++) if (count[i] > 0) for (int j = 0; j < count[i]; j++) result.push_back(string (1, 'a' + i));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> commonChars(String[] A) {
        List<String> result = new ArrayList<>();
        int count[] = new int[26];
        for (char c : A[0].toCharArray()) count[c - 'a']++;
        for (int i = 1; i < A.length; i++) {
            int temp[] = new int[26];
            for (char c : A[i].toCharArray()) temp[c - 'a']++; 
            for (int j = 0; j < 26; j++) count[j] = Math.min(count[j], temp[j]);
        }
        for (int i = 0; i < 26; i++) if (count[i] > 0) for (int j = 0; j < count[i]; j++) result.add(((char)('a' + i) + ""));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def commonChars(self, A: List[str]) -> List[str]:
        return [c for c in set(A[0]) for i in range(min(s.count(c) for s in A))]
```
