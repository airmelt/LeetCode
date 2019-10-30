__Description__:
You are given a license key represented as a string S which consists only alphanumeric character and dashes. The string is separated into N+1 groups by N dashes.

Given a number K, we would want to reformat the strings such that each group contains exactly K characters, except for the first group which could be shorter than K, but still must contain at least one character. Furthermore, there must be a dash inserted between two groups and all lowercase letters should be converted to uppercase.

Given a non-empty string S and a number K, format the string according to the rules described above.

__Example:__
Example 1:
Input: S = "5F3Z-2e-9-w", K = 4

Output: "5F3Z-2E9W"

Explanation: The string S has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.

Example 2:
Input: S = "2-5g-3-J", K = 2

Output: "2-5G-3J"

Explanation: The string S has been split into three parts, each part has 2 characters except the first part as it could be shorter as mentioned above.

__Note:__
The length of string S will not exceed 12,000, and K is a positive integer.
String S consists only of alphanumerical characters (a-z and/or A-Z and/or 0-9) and dashes(-).
String S is non-empty.

__题目描述__:
给定一个密钥字符串S，只包含字母，数字以及 '-'（破折号）。N 个 '-' 将字符串分成了 N+1 组。给定一个数字 K，重新格式化字符串，除了第一个分组以外，每个分组要包含 K 个字符，第一个分组至少要包含 1 个字符。两个分组之间用 '-'（破折号）隔开，并且将所有的小写字母转换为大写字母。

给定非空字符串 S 和数字 K，按照上面描述的规则进行格式化。

__示例 :__
示例 1：

输入：S = "5F3Z-2e-9-w", K = 4

输出："5F3Z-2E9W"

解释：字符串 S 被分成了两个部分，每部分 4 个字符；
     注意，两个额外的破折号需要删掉。

示例 2：

输入：S = "2-5g-3-J", K = 2

输出："2-5G-3J"

解释：字符串 S 被分成了 3 个部分，按照前面的规则描述，第一部分的字符可以少于给定的数量，其余部分皆为 2 个字符。
 

__提示:__

S 的长度不超过 12,000，K 为正整数
S 只包含字母数字（a-z，A-Z，0-9）以及破折号'-'
S 非空

__思路__:
从字符串的末尾开始遍历, 最后输出的时候反转字符串
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        string result;
        int num = 0;
        for (int i = S.size() - 1; i > -1; i--) {
            if (S[i] != '-') {
                if (num == K) {
                    result += "-";
                    num = 0;
                }
                result += toupper(S[i]);
                num++;
            }
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:
```
class Solution {
    public String licenseKeyFormatting(String S, int K) {
        StringBuilder sb = new StringBuilder();
        int num = 0;
        for (int i = S.length() - 1; i > -1; i--) {
            if (S.charAt(i) != '-') {
                if (num == K) {
                    sb.append("-");
                    num = 0;
                }
                sb.append((char)(S.charAt(i) >= 'a' && S.charAt(i)<='z' ? S.charAt(i) - 32 : S.charAt(i)));
                num++;    
            }
        }
        return sb.reverse().toString();
    }
}
```

__Python__:
```
class Solution:
    def licenseKeyFormatting(self, S: str, K: int) -> str:
        result, num, S = "", 0, S.upper()
        for i in range(len(S) - 1, -1, -1):
            if S[i] != '-':
                if num == K:
                    result += "-"
                    num = 0
                result += S[i]
                num += 1
        return result[::-1]          
```