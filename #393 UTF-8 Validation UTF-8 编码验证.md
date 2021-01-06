# 393 UTF-8 Validation UTF-8 编码验证

__Description__:
A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:

For 1-byte character, the first bit is a 0, followed by its unicode code.
For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.
This is how the UTF-8 encoding would work:

```text
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

Given an array of integers representing the data, return whether it is a valid utf-8 encoding.

__Note:__
The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

__Example:__

Example 1:

data = [197, 130, 1], which represents the octet sequence: 11000101 10000010 00000001.

Return true.
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.

Example 2:

data = [235, 140, 4], which represented the octet sequence: 11101011 10001100 00000100.

Return false.
The first 3 bits are all one's and the 4th bit is 0 means it is a 3-bytes character.
The next byte is a continuation byte which starts with 10 and that's correct.
But the second continuation byte does not start with 10, so it is invalid.

__题目描述__:
UTF-8 中的一个字符可能的长度为 1 到 4 字节，遵循以下的规则：

对于 1 字节的字符，字节的第一位设为0，后面7位为这个符号的unicode码。
对于 n 字节的字符 (n > 1)，第一个字节的前 n 位都设为1，第 n+1 位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的unicode码。
这是 UTF-8 编码的工作方式：

```text
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

给定一个表示数据的整数数组，返回它是否为有效的 utf-8 编码。

__注意:__
输入是整数数组。只有每个整数的最低 8 个有效位用来存储数据。这意味着每个整数只表示 1 字节的数据。

__示例 :__

示例 1:

data = [197, 130, 1], 表示 8 位的序列: 11000101 10000010 00000001.

返回 true 。
这是有效的 utf-8 编码，为一个2字节字符，跟着一个1字节字符。

示例 2:

data = [235, 140, 4], 表示 8 位的序列: 11101011 10001100 00000100.

返回 false 。
前 3 位都是 1 ，第 4 位为 0 表示它是一个3字节字符。
下一个字节是开头为 10 的延续字节，这是正确的。
但第二个延续字节不以 10 开头，所以是不符合规则的。

__思路__:

使用一个数 n记录接下来要出现的字节数
遍历数组, 对于当前遍历的数字
如果第 1位为 0, 说明是 1个字节, n = 0
如果前 3位为 110, 说明是 2个字节, n = 1
如果前 4位为 1110, 说明是 3个字节, n = 2
如果前 5位为 11110, 说明是 4个字节, n = 3
如果 n != 0, 说明现在的数字是前面的字节中的数字, 需要判断前两位是否为 10, n自减
最后判断 n是否为 0即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool validUtf8(vector<int>& data) 
    {
        int n = 0;
        for (auto i : data) 
        {
            if (n > 0) 
            {
                if (i >> 6 != 0b10) return false;
                --n;
            }
            else if (i >> 7 == 0) n = 0;
            else if (i >> 5 == 0b110) n = 1;
            else if (i >> 4 == 0b1110) n = 2;
            else if (i >> 3 == 0b11110) n = 3;
            else return false;
        }
        return n == 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validUtf8(int[] data) {
        int n = 0;
        for (int i : data) {
            if (n > 0) {
                if (i >> 6 != 0b10) return false;
                --n;
            }
            else if (i >> 7 == 0) n = 0;
            else if (i >> 5 == 0b110) n = 1;
            else if (i >> 4 == 0b1110) n = 2;
            else if (i >> 3 == 0b11110) n = 3;
            else return false;
        }
        return n == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def validUtf8(self, data: List[int]) -> bool:
        n = 0
        for i in data:
            if n:
                if i >> 6 != 0b10:
                    return False
                n -= 1
            elif not (i >> 7):
                n = 0
            elif i >> 5 == 0b110:
                n = 1
            elif i >> 4 == 0b1110:
                n = 2
            elif i >> 3 == 0b11110:
                n = 3
            else:
                return False
        return n == 0
```
