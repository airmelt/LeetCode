__Description__:
Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain at least three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits '0'-'9', write a function to determine if it's an additive number.

__Note:__
Numbers in the additive sequence cannot have leading zeros, so sequence 1, 2, 03 or 1, 02, 3 is invalid.

__Example:__
Example 1:

Input: "112358"
Output: true
Explanation: The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8

Example 2:

Input: "199100199"
Output: true
Explanation: The additive sequence is: 1, 99, 100, 199. 
             1 + 99 = 100, 99 + 100 = 199

__Constraints:__

num consists only of digits '0'-'9'.
1 <= num.length <= 35

__Follow up:__
How would you handle overflow for very large input integers?

__题目描述__:
累加数是一个字符串，组成它的数字可以形成累加序列。

一个有效的累加序列必须至少包含 3 个数。除了最开始的两个数以外，字符串中的其他数都等于它之前两个数相加的和。

给定一个只包含数字 '0'-'9' 的字符串，编写一个算法来判断给定输入是否是累加数。

__说明:__
累加序列里的数不会以 0 开头，所以不会出现 1, 2, 03 或者 1, 02, 3 的情况。

__示例 :__
示例 1:

输入: "112358"
输出: true 
解释: 累加序列为: 1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8

示例 2:

输入: "199100199"
输出: true 
解释: 累加序列为: 1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199

__进阶：__
你如何处理一个溢出的过大的整数输入?

__思路__:
dfs ➕ 剪枝
将字符串分成 3个部分, 第一个部分从 1开始(保证非空)最多到一半字符串位置, 第二个字符串从第一个部分的后 1个开始(保证非空)到结尾, 只要找到一条合适路径就返回 true
剪枝操作:
1. 判断前导 0
2. 判断两个子字符串之和是否超出 num的范围
3. 检查和是否在 num中
4. 如果到结尾立即返回 true
否则继续调用 dfs搜索下一个子字符串
时间复杂度O(n ^ 3), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    bool isAdditiveNumber(string num) 
    {
        for (int i = 1; i <= num.size() / 2; i++) for (int j = i + 1; j < num.size(); j++) if (dfs(0, i, j, num)) return true;
        return false;
    }
private:
    bool dfs(int i, int j, int k, string& num)
    {
        if ((num[i] == '0' and j - i != 1) or (num[j] == '0' and k - j != 1)) return false;
        string s1 = num.substr(i, j - i), s2 = num.substr(j, k - j), sum = add(s1, s2);
        if (sum.size() + k - 1 >= num.size()) return false;
        for (int p = k; p < k + sum.size(); p++) if (num[p] != sum[p - k]) return false;
        if (sum.size() + k - 1 == num.size() - 1) return true;
        return dfs(j, k, k + sum.size(), num);
    }
    
    string add(string& a,string& b)
    {
        int n1 = a.size() - 1, n2 = b.size() - 1, carry = 0;
        string result;
        while (n1 >= 0 or n2 >= 0 or carry > 0)
        {
            int t1 = n1 >= 0 ? a[n1--] - '0' : 0, t2 = n2 >= 0 ? b[n2--] - '0' : 0;
            result += (t1 + t2 + carry) % 10 + '0';
            carry = (t1 + t2 + carry) / 10;
        }
        reverse(result.begin(),result.end());
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean isAdditiveNumber(String num) {
        for (int i = 1; i < num.length() / 2 + 1; i++) for (int j = i + 1; j < num.length(); j++) if (dfs(0, i, j, num)) return true;
        return false;
    }
    
    private String add(String a, String b) {
        int n1 = a.length() - 1, n2 = b.length() - 1, c = 0;
        StringBuilder sb = new StringBuilder();
        while (n1 >= 0 || n2 >= 0 || c > 0) {
            int t1 = n1 >= 0 ? a.charAt(n1--) - '0' : 0, t2 = n2 >= 0 ? b.charAt(n2--) - '0' : 0;
            sb.append((t1 + t2 + c) % 10);
            c = (t1 + t2 + c) / 10;
        }
        return sb.reverse().toString();
    }
    
    private boolean dfs(int i, int j, int k, String num) {
        if ((num.charAt(i) == '0' && j - i != 1) || (num.charAt(j) == '0' && k - j != 1)) return false;
        String s1 = num.substring(i, j), s2 = num.substring(j, k), s = add(s1, s2);
        if (s.length() + k - 1 >= num.length()) return false;
        for (int p = k; p < k + s.length(); p++) if (num.charAt(p) != s.charAt(p - k)) return false;
        if (s.length() + k - 1 == num.length() - 1) return true;
        return dfs(j, k, k + s.length(), num);
    }
}
```

__Python__:
```Python
class Solution:
    def isAdditiveNumber(self, num: str) -> bool:
        def dfs(i: int, j: int, k: int, num: str) -> bool:
            if (num[i] == '0' and j - i > 1) or (num[j] == '0' and k - j > 1):
                return False
            s1, s2, s = num[i:j], num[j:k], str(int(num[i:j]) + int(num[j:k]))
            if len(s) + k - 1 >= len(num) or any(s[p - k] != num[p] for p in range(k, k + len(s))):
                return False
            if len(s) + k - 1 == len(num) - 1:
                return True
            return dfs(j, k, k + len(s), num)
        return any(dfs(0, i, j, num) for i in range(1, (len(num) >> 1) + 1) for j in range(i + 1, len(num)))
```