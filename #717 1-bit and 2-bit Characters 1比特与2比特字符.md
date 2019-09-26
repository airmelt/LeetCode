__Description__:
We have two special characters. The first character can be represented by one bit 0. The second character can be represented by two bits (10 or 11).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.

__Example:__
Example 1:

Input: 
bits = [1, 0, 0]
Output: True
Explanation: 
The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.

Example 2:

Input: 
bits = [1, 1, 1, 0]
Output: False
Explanation: 
The only way to decode it is two-bit character and two-bit character. So the last character is NOT one-bit character.

__Note:__

1 <= len(bits) <= 1000.
bits[i] is always 0 or 1.

__题目描述__:
有两种特殊字符。第一种字符可以用一比特0来表示。第二种字符可以用两比特(10 或 11)来表示。

现给一个由若干比特组成的字符串。问最后一个字符是否必定为一个一比特字符。给定的字符串总是由0结束。

__示例 :__
示例 1:

输入: 
bits = [1, 0, 0]
输出: True
解释: 
唯一的编码方式是一个两比特字符和一个一比特字符。所以最后一个字符是一比特字符。

示例 2:

输入: 
bits = [1, 1, 1, 0]
输出: False
解释: 
唯一的编码方式是两比特字符和两比特字符。所以最后一个字符不是一比特字符。

__注意：__

1 <= len(bits) <= 1000.
bits[i] 总是0 或 1.

__思路__:
1. 正则表达式
2. 遍历数组, 如果是 1, 则步进 2, 否则步进 1, 最后要到倒数第一个位置输出 true, 否则输出 false
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int start = 0;
        while (start < bits.size() - 1) {
            if (bits[start]) start += 2;
            else start++;
        }
        return start == bits.size() - 1;
    }
};
```

__Java__:
```
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int start = 0;
        while (start < bits.length - 1)
            if (bits[start] == 0) start++;
            else start += 2;
        return start == bits.length - 1;
    }
}
```

__Python__:
```
class Solution:
    def isOneBitCharacter(self, bits: List[int]) -> bool:
        return True if re.match('^(10|11|0)*0$', ''.join(str(bit) for bit in bits)) else False
```