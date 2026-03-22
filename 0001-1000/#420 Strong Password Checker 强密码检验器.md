# 420 Strong Password Checker 强密码检验器

__Description__:
A password is considered strong if the below conditions are all met:

It has at least 6 characters and at most 20 characters.
It contains at least one lowercase letter, at least one uppercase letter, and at least one digit.
It does not contain three repeating characters in a row (i.e., "...aaa..." is weak, but "...aa...a..." is strong, assuming other conditions are met).
Given a string password, return the minimum number of steps required to make password strong. if password is already strong, return 0.

In one step, you can:

Insert one character to password,
Delete one character from password, or
Replace one character of password with another character.

__Example:__

Example 1:

Input: password = "a"
Output: 5

Example 2:

Input: password = "aA1"
Output: 3

Example 3:

Input: password = "1337C0d3"
Output: 0

__Constraints:__

1 <= password.length <= 50
password consists of letters, digits, dot '.' or exclamation mark '!'.

__题目描述__:
一个强密码应满足以下所有条件：

由至少6个，至多20个字符组成。
至少包含一个小写字母，一个大写字母，和一个数字。
同一字符不能连续出现三次 (比如 "...aaa..." 是不允许的, 但是 "...aa...a..." 是可以的)。
编写函数 strongPasswordChecker(s)，s 代表输入字符串，如果 s 已经符合强密码条件，则返回0；否则返回要将 s 修改为满足强密码条件的字符串所需要进行修改的最小步数。

插入、删除、替换任一字符都算作一次修改。

__思路__:

长度小于 6, 补充到 6的长度和缺失的种类取最大的
长度 6-20, 这时替换效率最高, 比如 'aaaaa' -> 'aabaa' > 'aaaaa' -> 'aabaaba' > 'aaaaa' -> 'aaa'
长度大于 20, 至少需要删除 n - 20个字符, 从 2中可以看出来如果是 3n + 2长度的字符, 只需要修改 n个字符就可以使得字符连续数少于 3, 所以尽量将字符转变为 3n + 2的形式, 3n的可以删除一个字符转化为 3(n - 1) + 2的形式, 3n + 1的需要删除 2个字符转化为 3(n - 1) + 2的形式
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int strongPasswordChecker(string password) 
    {
        int start = 0, end = 0, n = password.size(), modify = 0, count[]{0, 0, 0}, c = 0, len = 0, need = 0, remove = 0, num = 1, lower = 1, upper = 1;
        while (end < n) 
        {
            c = password[end];
            if (c >= '0' and c <= '9') num = 0;
            if (c >= 'a' and c <= 'z') lower = 0;
            if (c >= 'A' and c <= 'Z') upper = 0;
            while (end < n and password[end] == c) ++end;
            len = end - start;
            if (len > 2) 
            {
                modify += len / 3;
                ++count[len % 3];
            }
            start = end;
        }
        need = num + lower + upper;
        if (n < 6) return max(6 - n, need);
        if (n <= 20) return max(modify, need);
        remove = n - 20;
        n -= 20;
        if (remove < count[0]) return max(modify - remove, need) + n;
        remove -= count[0];
        modify -= count[0];
        if (remove < (count[1] << 1)) return max(modify - (remove >> 1), need) + n;
        remove -= (count[1] << 1);
        modify -= count[1];
        return max(modify - remove / 3, need) + n;
    }
};
```

__Java__:

```Java
public class Solution {
    public int strongPasswordChecker(String password) {
        boolean num = false, lower = false, upper = false;
        int start = 0, end = 0, n = password.length(), modify = 0, count[] = new int[]{0, 0, 0}, c = 0, len = 0, need = 0, remove = 0;
        while (end < n) {
            c = password.charAt(end);
            if (c >= '0' && c <= '9') num = true;
            if (c >= 'a' && c <= 'z') lower = true;
            if (c >= 'A' && c <= 'Z') upper = true;
            while (end < n && password.charAt(end) == c) ++end;
            len = end - start;
            if (len > 2) {
                modify += len / 3;
                ++count[len % 3];
            }
            start = end;
        }
        need = (num ? 0 : 1) + (lower ? 0 : 1) + (upper ? 0 : 1);
        if (n < 6) return Math.max(6 - n, need);
        if (n <= 20) return Math.max(modify, need);
        remove = n - 20;
        n -= 20;
        if (remove < count[0]) return Math.max(modify - remove, need) + n;
        remove -= count[0];
        modify -= count[0];
        if (remove < (count[1] << 1)) return Math.max(modify - (remove >> 1), need) + n;
        remove -= (count[1] << 1);
        modify -= count[1];
        return Math.max(modify - remove / 3, need) + n;
    }
}
```

__Python__:

```Python
class Solution:
    def strongPasswordChecker(self, password: str) -> int:
        start, end, n, modify, count, c, l, need, remove, number, lower, upper = 0, 0, len(password), 0, [0] * 3, '', 0, 0, 0, True, True, True
        while end < n:
            c = password[end]
            if '0' <= c <= '9':
                number = False
            if 'a' <= c <= 'z':
                lower = False
            if 'A' <= c <= 'Z':
                upper = False
            while end < n and password[end] == c:
                end += 1
            l = end - start
            if l > 2:
                modify += l // 3
                count[l % 3] += 1
            start = end;
        need = sum([number, lower, upper])
        if n < 6:
            return max(6 - n, need)
        if n <= 20:
            return max(modify, need)
        remove = n - 20
        n -= 20
        if remove < count[0]:
            return max(modify - remove, need) + n
        remove -= count[0]
        modify -= count[0]
        if remove < (count[1] << 1):
            return max(modify - (remove >> 1), need) + n
        remove -= (count[1] << 1)
        modify -= count[1];
        return max(modify - remove // 3, need) + n
```
