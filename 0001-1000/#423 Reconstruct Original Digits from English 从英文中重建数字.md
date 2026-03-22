# 423 Reconstruct Original Digits from English 从英文中重建数字

__Description__:
Given a non-empty string containing an out-of-order English representation of digits 0-9, output the digits in ascending order.

__Note:__
Input contains only lowercase English letters.
Input is guaranteed to be valid and can be transformed to its original digits. That means invalid inputs such as "abc" or "zerone" are not permitted.
Input length is less than 50,000.

__Example:__

Example 1:
Input: "owoztneoer"

Output: "012"

Example 2:
Input: "fviefuro"

Output: "45"

__题目描述__:
给定一个非空字符串，其中包含字母顺序打乱的英文单词表示的数字0-9。按升序输出原始的数字。

__注意:__

输入只包含小写英文字母。
输入保证合法并可以转换为原始的数字，这意味着像 "abc" 或 "zerone" 的输入是不允许的。
输入字符串的长度小于 50,000。

__示例 :__

示例 1:

输入: "owoztneoer"

输出: "012" (zeroonetwo)

示例 2:

输入: "fviefuro"

输出: "45" (fourfive)

__思路__:

首先找到各自的特殊的字符
比如 0对应 z, 2对应 w, 4对应 u, 6对应 x, 8对应 g
然后看各自的英文对应的数量找等量关系, 比如 7(seven) = count('s') - count('x') = count('s') - 6(six)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string originalDigits(string s) 
    {
        vector<int> count(26), nums(10);
        string result;
        for (auto c : s) ++count[c - 'a'];
        nums[0] = count['z' - 'a'];
        nums[2] = count['w' - 'a'];
        nums[4] = count['u' - 'a'];
        nums[6] = count['x' - 'a'];
        nums[8] = count['g' - 'a'];
        nums[1] = count['o' - 'a'] - nums[0] - nums[2] - nums[4];
        nums[3] = count['h' - 'a'] - nums[8];
        nums[5] = count['f' - 'a'] - nums[4];
        nums[7] = count['s' - 'a'] - nums[6];
        nums[9] = count['i' - 'a'] - nums[8] - nums[6] - nums[5];
        for (int i = 0; i < 10; i++) while (nums[i]-- > 0) result += to_string(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String originalDigits(String s) {
        int count[] = new int[26], nums[] = new int[10];
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) ++count[c - 'a'];
        nums[0] = count['z' - 'a'];
        nums[2] = count['w' - 'a'];
        nums[4] = count['u' - 'a'];
        nums[6] = count['x' - 'a'];
        nums[8] = count['g' - 'a'];
        nums[1] = count['o' - 'a'] - nums[0] - nums[2] - nums[4];
        nums[3] = count['h' - 'a'] - nums[8];
        nums[5] = count['f' - 'a'] - nums[4];
        nums[7] = count['s' - 'a'] - nums[6];
        nums[9] = count['i' - 'a'] - nums[8] - nums[6] - nums[5];
        for (int i = 0; i < 10; i++) while (nums[i]-- > 0) sb.append(i);
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def originalDigits(self, s: str) -> str:
        return "".join((str(i) * n for i, n in enumerate([s.count('z'), s.count('o') - s.count('z') - s.count('w') - s.count('u'), s.count('w'), s.count('h') - s.count('g'), s.count('u'), s.count('f') - s.count('u'), s.count('x'), s.count('s') - s.count('x'), s.count('g'), s.count('i') - s.count('g') - s.count('x') - s.count('f') + s.count('u')])))
```
