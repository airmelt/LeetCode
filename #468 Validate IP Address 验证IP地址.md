# 468 Validate IP Address 验证IP地址

__Description__:
Given a string IP, return "IPv4" if IP is a valid IPv4 address, "IPv6" if IP is a valid IPv6 address or "Neither" if IP is not a correct IP of any type.

A valid IPv4 address is an IP in the form "x1.x2.x3.x4" where 0 <= xi <= 255 and xi cannot contain leading zeros. For example, "192.168.1.1" and "192.168.1.0" are valid IPv4 addresses but "192.168.01.1", while "192.168.1.00" and "192.168@1.1" are invalid IPv4 addresses.

A valid IPv6 address is an IP in the form "x1:x2:x3:x4:x5:x6:x7:x8" where:

1 <= xi.length <= 4
xi is a hexadecimal string which may contain digits, lower-case English letter ('a' to 'f') and upper-case English letters ('A' to 'F').
Leading zeros are allowed in xi.
For example, "2001:0db8:85a3:0000:0000:8a2e:0370:7334" and "2001:db8:85a3:0:0:8A2E:0370:7334" are valid IPv6 addresses, while "2001:0db8:85a3::8A2E:037j:7334" and "02001:0db8:85a3:0000:0000:8a2e:0370:7334" are invalid IPv6 addresses.

__Example:__

Example 1:

Input: IP = "172.16.254.1"
Output: "IPv4"
Explanation: This is a valid IPv4 address, return "IPv4".

Example 2:

Input: IP = "2001:0db8:85a3:0:0:8A2E:0370:7334"
Output: "IPv6"
Explanation: This is a valid IPv6 address, return "IPv6".

Example 3:

Input: IP = "256.256.256.256"
Output: "Neither"
Explanation: This is neither a IPv4 address nor a IPv6 address.

Example 4:

Input: IP = "2001:0db8:85a3:0:0:8A2E:0370:7334:"
Output: "Neither"

Example 5:

Input: IP = "1e1.4.5.6"
Output: "Neither"

__Constraints:__

IP consists only of English letters, digits and the characters '.' and ':'.

__题目描述__:
编写一个函数来验证输入的字符串是否是有效的 IPv4 或 IPv6 地址。

如果是有效的 IPv4 地址，返回 "IPv4" ；
如果是有效的 IPv6 地址，返回 "IPv6" ；
如果不是上述类型的 IP 地址，返回 "Neither" 。
IPv4 地址由十进制数和点来表示，每个地址包含 4 个十进制数，其范围为 0 - 255， 用(".")分割。比如，172.16.254.1；

同时，IPv4 地址内的数不会以 0 开头。比如，地址 172.16.254.01 是不合法的。

IPv6 地址由 8 组 16 进制的数字来表示，每组表示 16 比特。这些组数字通过 (":")分割。比如,  2001:0db8:85a3:0000:0000:8a2e:0370:7334 是一个有效的地址。而且，我们可以加入一些以 0 开头的数字，字母可以使用大写，也可以是小写。所以， 2001:db8:85a3:0:0:8A2E:0370:7334 也是一个有效的 IPv6 address地址 (即，忽略 0 开头，忽略大小写)。

然而，我们不能因为某个组的值为 0，而使用一个空的组，以至于出现 (::) 的情况。 比如， 2001:0db8:85a3::8A2E:0370:7334 是无效的 IPv6 地址。

同时，在 IPv6 地址中，多余的 0 也是不被允许的。比如， 02001:0db8:85a3:0000:0000:8a2e:0370:7334 是无效的。

__示例 :__

示例 1：

输入：IP = "172.16.254.1"
输出："IPv4"
解释：有效的 IPv4 地址，返回 "IPv4"

示例 2：

输入：IP = "2001:0db8:85a3:0:0:8A2E:0370:7334"
输出："IPv6"
解释：有效的 IPv6 地址，返回 "IPv6"

示例 3：

输入：IP = "256.256.256.256"
输出："Neither"
解释：既不是 IPv4 地址，又不是 IPv6 地址

示例 4：

输入：IP = "2001:0db8:85a3:0:0:8A2E:0370:7334:"
输出："Neither"

示例 5：

输入：IP = "1e1.4.5.6"
输出："Neither"

__提示：__

IP 仅由英文字母，数字，字符 '.' 和 ':' 组成。

__思路__:

1. 正则表达式
2. 先按点和冒号区分开 ipv4和 ipv6, 然后判断每一个字符串是否在区间内
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string validIPAddress(string IP)
    {
        regex ipv4(R"(^((\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])\.){3}(\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])$)");
        if (regex_match(IP, ipv4)) return "IPv4";
        regex ipv6(R"(^([\da-fA-F]{1,4}:){7}[\da-fA-F]{1,4}$)");
        if (regex_match(IP, ipv6)) return "IPv6";
        return "Neither";
    }
};
```

__Java__:

```Java
class Solution {
    private String validateIPv4(String IP) {
        String[] nums = IP.split("\\.", -1);
        for (String x : nums) {
            if (x.length() == 0 || x.length() > 3) return "Neither";
            if (x.charAt(0) == '0' && x.length() != 1) return "Neither";
            for (char ch : x.toCharArray()) if (! Character.isDigit(ch)) return "Neither";
            if (Integer.parseInt(x) > 255) return "Neither";
        }
        return "IPv4";
    }

    private String validateIPv6(String IP) {
        String[] nums = IP.split(":", -1);
        String hexdigits = "0123456789abcdefABCDEF";
        for (String x : nums) {
            if (x.length() == 0 || x.length() > 4) return "Neither";
            for (Character ch : x.toCharArray()) if (hexdigits.indexOf(ch) == -1) return "Neither";
        }
        return "IPv6";
    }

    public String validIPAddress(String IP) {
        return (IP.chars().filter(ch -> ch == '.').count() == 3) ? validateIPv4(IP) : ((IP.chars().filter(ch -> ch == ':').count() == 7) ? validateIPv6(IP) : "Neither");
    }
}
```

__Python__:

```Python
class Solution:
    def validIPAddress(self, IP: str) -> str:
        return "IPv4" if re.compile(r'^(' + r'([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])' + r'\.){3}' + r'([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])' + r'$').match(IP) else "IPv6" if re.compile(r'^(' + r'([0-9a-fA-F]{1,4})' + r'\:){7}' + r'([0-9a-fA-F]{1,4})' + r'$').match(IP) else "Neither"
```
