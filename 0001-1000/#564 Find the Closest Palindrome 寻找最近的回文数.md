# 564 Find the Closest Palindrome 寻找最近的回文数

__Description__:
Given a string n representing an integer, return the closest integer (not including itself), which is a palindrome. If there is a tie, return the smaller one.

The closest is defined as the absolute difference minimized between two integers.

__Example:__

Example 1:

Input: n = "123"
Output: "121"
Example 2:

Input: n = "1"
Output: "0"
Explanation: 0 and 2 are the closest palindromes but we return the smallest which is 0.

__Constraints:__

1 <= n.length <= 18
n consists of only digits.
n does not have leading zeros.
n is representing an integer in the range [1, 10^18 - 1].

__题目描述__:
给定一个整数 n ，你需要找到与它最近的回文数（不包括自身）。

“最近的”定义为两个整数差的绝对值最小。

__示例 :__

示例 1:

输入: "123"
输出: "121"

__注意:__

n 是由字符串表示的正整数，其长度不超过18。
如果有多个结果，返回最小的那个。

__思路__:

1. 如果 n < 10 或者 n 为 10的幂, 因为不能取自身, 取 n - 1
2. n = 11, 取 9
3. n 中所有字符均为 '9', 取 n + 2
4. 将 n 分为前后两个部分 a + b, 分别取 a - 1, a, a + 1构造回文数字
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string nearestPalindromic(string n) 
    {
        string cur = n;
        for (int i = 0, j = cur.size() - 1; i < j; i++, j--) cur[j] = cur[i];
        string pre = nearest(cur, false), next = nearest(cur, true);
        long numl = stol(n), curl = stol(cur), prel = stol(pre), nextl = stol(next);
        long d1 = abs(numl - prel), d2 = abs(numl - curl), d3 = abs(numl - nextl); 
        return numl == curl ? (d1 <= d3 ? pre : next) : (numl < curl ? (d2 < d1 ? cur : pre) : (d2 <= d3 ? cur : next));
    }
private:
    string nearest(string cur, bool flag) 
    {
        long right = cur.size() >> 1;
        long left = cur.size() - right;
        long l = flag ? stoi(cur.substr(0, (int)left)) + 1 : stoi(cur.substr(0, (int)left)) - 1;
        if (l == 0) return right == 0 ? "0" : "9";
        string ll = to_string(l);
        string rr = ll;
        reverse(rr.begin(), rr.end());
        if (right > ll.size()) rr += '9';
        return ll + rr.substr(rr.size() - (int)right);
    }
};
```

__Java__:

```Java
class Solution {
    public String nearestPalindromic(String n) {
        char[] arr = n.toCharArray();
        for (int i = 0, j = arr.length - 1; i < j; i++, j--) arr[j] = arr[i];
        String cur = String.valueOf(arr);
        String pre = nearest(cur, false), next = nearest(cur, true);
        long numl = Long.valueOf(n), curl = Long.valueOf(cur), prel = Long.valueOf(pre), nextl = Long.valueOf(next);
        long d1 = Math.abs(numl - prel), d2 = Math.abs(numl - curl), d3 = Math.abs(numl - nextl); 
        return numl == curl ? (d1 <= d3 ? pre : next) : (numl < curl ? (d2 < d1 ? cur : pre) : (d2 <= d3 ? cur : next));
    }
    
    private String nearest(String cur, boolean flag) {
        long right = cur.length() >> 1;
        long left = cur.length() - right;
        long l = flag ? Integer.valueOf(cur.substring(0, (int)left)) + 1 : Integer.valueOf(cur.substring(0, (int)left)) - 1;
        if (l == 0) return right == 0 ? "0" : "9";
        StringBuilder ll = new StringBuilder(String.valueOf(l));
        StringBuilder rr = new StringBuilder(ll).reverse();
        if (right > ll.length()) rr.append(9);
        return ll.append(rr.substring(rr.length() - (int)right)).toString();
    }
}
```

__Python__:

```Python
class Solution:
    def nearestPalindromic(self, n: str) -> str:
        if int(n) < 10 or int(n[::-1]) == 1:
            return str(int(n) - 1)
        elif n == '11':
            return '9'
        elif set(n) == {'9'}:
            return str(int(n) + 2)
        a, b = n[:(len(n) + 1) >> 1], n[(len(n) + 1) >> 1:]
        return min([c + c[len(b) - 1::-1] for c in [str(int(a) - 1), a, str(int(a) + 1)]], key=lambda x: abs(int(x) - int(n) or float('inf')))
```
