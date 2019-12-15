__Description__:
Given a valid (IPv4) IP address, return a defanged version of that IP address.

A defanged IP address replaces every period "." with "[.]".

__Example:__
Example 1:

Input: address = "1.1.1.1"
Output: "1[.]1[.]1[.]1"

Example 2:

Input: address = "255.100.50.0"
Output: "255[.]100[.]50[.]0"
 
__Constraints:__

The given address is a valid IPv4 address.

__题目描述__:
给你一个有效的 IPv4 地址 address，返回这个 IP 地址的无效化版本。

所谓无效化 IP 地址，其实就是用 "[.]" 代替了每个 "."。

__示例 :__
示例 1：

输入：address = "1.1.1.1"
输出："1[.]1[.]1[.]1"

示例 2：

输入：address = "255.100.50.0"
输出："255[.]100[.]50[.]0"
 
__提示：__

给出的 address 是一个有效的 IPv4 地址

__思路__:
遍历字符串, 按照题目要求把 “.”替换成 “[.]”即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    string defangIPaddr(string address) 
    {
        for (int i = address.size() - 1; i > -1; --i) if (address[i] == '.') address.replace(i, 1, "[.]");
        return address;
    }
};
```

__Java__:
```Java
class Solution {
    public String defangIPaddr(String address) {
        return address.replaceAll("\\.", "[.]");
    }
}
```

__Python__:
```Python
class Solution:
    def defangIPaddr(self, address: str) -> str:
        return address.replace(r'.', '[.]')
```