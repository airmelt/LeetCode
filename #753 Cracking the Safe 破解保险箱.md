# 753 Cracking the Safe 破解保险箱

__Description__:
There is a box protected by a password. The password is a sequence of n digits where each digit can be in the range [0, k - 1].

While entering a password, the last n digits entered will automatically be matched against the correct password.

For example, assuming the correct password is "345", if you type "012345", the box will open because the correct password matches the suffix of the entered password.
Return any password of minimum length that is guaranteed to open the box at some point of entering it.

__Example:__

Example 1:

Input: n = 1, k = 2
Output: "10"
Explanation: "01" will be accepted too.

Example 2:

Input: n = 2, k = 2
Output: "01100"
Explanation: "01100", "10011", "11001" will be accepted too.

__Constraints:__

1 <= n <= 4
1 <= k <= 10
1 <= k^n <= 4096

__题目描述__:
有一个需要密码才能打开的保险箱。密码是 n 位数, 密码的每一位是 k 位序列 0, 1, ..., k-1 中的一个 。

你可以随意输入密码，保险箱会自动记住最后 n 位输入，如果匹配，则能够打开保险箱。

举个例子，假设密码是 "345"，你可以输入 "012345" 来打开它，只是你输入了 6 个字符.

请返回一个能打开保险箱的最短字符串。

__示例 :__

示例1:

输入: n = 1, k = 2
输出: "01"
说明: "10"也可以打开保险箱。

示例2:

输入: n = 2, k = 2
输出: "00110"
说明: "01100", "10011", "11001" 也能打开保险箱。

__提示:__

n 的范围是 [1, 4]。
k 的范围是 [1, 10]。
k^n 最大可能为 4096。

__思路__:

DFS
题目的含义是求出最短的字符串包括长度为 n 的所有的密码的 k 排列组合
比如 n = 2, k = 2, 排列方式有 00, 01, 10, 11 最短的字符串为 00110
从 '0' \* (n - 1) 出发找遍所有 k 排列组合的路径即可
时间复杂度为 O(n \* k ^ n), 空间复杂度为 O(n \* k ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string crackSafe(int n, int k) 
    {
        string result(n - 1, '0');
        int p = pow(k, n);
        unordered_map<string, int> m;
        for (int i = 0; i < p; i++) result.push_back('0' + k - ++m[result.substr(i, n - 1)]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String crackSafe(int n, int k) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n - 1; i++) sb.append('0');
        int p = (int)Math.pow(k, n);
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < p; i++) {
            sb.append((char)('0' + k - map.getOrDefault(sb.toString().substring(i, n + i - 1), 1)));
            map.put(sb.substring(i, n + i - 1), map.getOrDefault(sb.toString().substring(i, n + i - 1), 1) + 1);
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def crackSafe(self, n: int, k: int) -> str:
        if n == 1: 
            return ''.join(str(i) for i in range(k))
        d, s = {}, '0' * (n - 1)
        for i in range(k ** n):
            t = s[1 - n:]
            d[t] = d.get(t, k) - 1
            s += str(d[t])
        return s
```
