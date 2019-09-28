__Description__:
Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

__Example:__

Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]

__Note:__

S will be a string with length between 1 and 12.
S will consist only of letters or digits.

__题目描述__:
给定一个字符串S，通过将字符串S中的每个字母转变大小写，我们可以获得一个新的字符串。返回所有可能得到的字符串集合。

__示例 :__

输入: S = "a1b2"
输出: ["a1b2", "a1B2", "A1b2", "A1B2"]

输入: S = "3z4"
输出: ["3z4", "3Z4"]

输入: S = "12345"
输出: ["12345"]

__注意：__

S 的长度不超过12。
S 仅由数字和字母组成。

__思路__:
回溯法, 大小写转换可以使用 s[i] ^= (1 << 5), 实际上是大小写的 ascii码刚好相差 32
时间复杂度O(2 ^ n), 空间复杂度O(1), n表示字符串的长度

__代码__:
__C++__:
```
class Solution {
public:
    vector<string> letterCasePermutation(string S) {
        vector<string> result;
        traceback(result, S, 0);
        return result;
    }
private:
    void traceback(vector<string> &result, string s, int len) {
        if (len == s.size()) {
            result.push_back(s);
            return;
        }
        traceback(result, s, len + 1);
        if (s[len] > '9') {
            s[len] ^= (1 << 5);
            traceback(result, s, len + 1);
        }
    }
};
```

__Java__:
```
class Solution {
    public List<String> letterCasePermutation(String S) {
        List<String> result = new ArrayList<>();
        traceback(result, S.toCharArray(), 0);
        return result;
    }
    
    private void traceback(List<String> result, char[] s, int len) {
        if (len == s.length) {
            result.add(String.valueOf(s));
            return;
        }
        traceback(result, s, len + 1);
        if (s[len] > '9') {
            s[len] ^= (1 << 5);
            traceback(result, s, len + 1);
        }
    }
}
```

__Python__:
```
class Solution:
    def letterCasePermutation(self, S: str) -> List[str]:
        if not S:
            return [S]
        rest = self.letterCasePermutation(S[1:])
        if S[0].isalpha():
            return [S[0].lower() + s for s in rest] + [S[0].upper() + s for s in rest]
        return [S[0] + s for s in rest]
```