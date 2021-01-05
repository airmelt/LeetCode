# 443 String Compression 压缩字符串

__Description__:
Given an array of characters, compress it in-place.

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a character (not int) of length 1.

After you are done modifying the input array in-place, return the new length of the array.

__Follow up:__
Could you solve it using only O(1) extra space?

__Example:__

Example 1:

Input:
["a","a","b","b","c","c","c"]

Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".

Example 2:

Input:
["a"]

Output:
Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:
Nothing is replaced.

Example 3:

Input:
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

Output:
Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.

__Note:__

All characters have an ASCII value in [35, 126].
1 <= len(chars) <= 1000.

__题目描述__:
给定一组字符，使用原地算法将其压缩。

压缩后的长度必须始终小于或等于原数组长度。

数组的每个元素应该是长度为1 的字符（不是 int 整数类型）。

在完成原地修改输入数组后，返回数组的新长度。

__进阶：__
你能否仅使用O(1) 空间解决问题？

__示例：__

示例 1：

输入：
["a","a","b","b","c","c","c"]

输出：
返回6，输入数组的前6个字符应该是：["a","2","b","2","c","3"]

说明：
"aa"被"a2"替代。"bb"被"b2"替代。"ccc"被"c3"替代。

示例 2：

输入：
["a"]

输出：
返回1，输入数组的前1个字符应该是：["a"]

说明：
没有任何字符串被替代。

示例 3：

输入：
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

输出：
返回4，输入数组的前4个字符应该是：["a","b","1","2"]。

说明：
由于字符"a"不重复，所以不会被压缩。"bbbbbbbbbbbb"被“b12”替代。
注意每个数字在数组中都有它自己的位置。

__注意：__

所有字符都有一个ASCII值在[35, 126]区间内。
1 <= len(chars) <= 1000。

__思路__:

使用一个长度指针记录返回结果, 并记录重复的值的长度
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int compress(vector<char>& chars) 
    {
        int i = 0, j = 0, l = chars.size();
        while (i < l and j < l) 
        {
            chars[i++] = chars[j];
            int temp = j;
            while (j < l and chars[i - 1] == chars[j]) j++;
            if (j - temp > 1) 
            {
                for (char c : to_string(j - temp)) chars[i++] = c;
            }
        }
        return i;
    }
};
```

__Java__:

```Java
class Solution {
    public int compress(char[] chars) {
        int i = 0, j = 0, l = chars.length;
        while (i < l && j < l) {
            chars[i++] = chars[j];
            int temp = j;
            while (j < l && chars[i - 1] == chars[j]) j++;
            if (j - temp > 1) {
                for (char c : String.valueOf(j - temp).toCharArray()) chars[i++] = c;
            }
        }
        return i;
    }
}
```

__Python__:

```Python
class Solution:
    def compress(self, chars: List[str]) -> int:
        i, j, l = 0, 0, len(chars)
        while i < l and j < l:
            chars[i] = chars[j]
            i += 1
            temp = j
            while j < l and chars[i - 1] == chars[j]:
                j += 1
            if j - temp <= 1 :
                continue
            for c in str(j - temp):
                chars[i] = c
                i += 1
        return i
```
