# 777 Swap Adjacent in LR String 在LR字符串中交换相邻字符

__Description__:
In a string composed of 'L', 'R', and 'X' characters, like "RXXLRXRXL", a move consists of either replacing one occurrence of "XL" with "LX", or replacing one occurrence of "RX" with "XR". Given the starting string start and the ending string end, return True if and only if there exists a sequence of moves to transform one string to the other.

__Example:__

Example 1:

Input: start = "RXXLRXRXL", end = "XRLXXRRLX"
Output: true
Explanation: We can transform start to end following these steps:

```text
RXXLRXRXL ->
XRXLRXRXL ->
XRLXRXRXL ->
XRLXXRRXL ->
XRLXXRRLX
```

Example 2:

Input: start = "X", end = "L"
Output: false

Example 3:

Input: start = "LLR", end = "RRL"
Output: false

Example 4:

Input: start = "XL", end = "LX"
Output: true

Example 5:

Input: start = "XLLR", end = "LXLX"
Output: false

__Constraints:__

1 <= start.length <= 10^4
start.length == end.length
Both start and end will only consist of characters in 'L', 'R', and 'X'.

__题目描述__:
在一个由 'L' , 'R' 和 'X' 三个字符组成的字符串（例如"RXXLRXRXL"）中进行移动操作。一次移动操作指用一个"LX"替换一个"XL"，或者用一个"XR"替换一个"RX"。现给定起始字符串start和结束字符串end，请编写代码，当且仅当存在一系列移动操作使得start可以转换成end时， 返回True。

__示例 :__

输入: start = "RXXLRXRXL", end = "XRLXXRRLX"
输出: True
解释:
我们可以通过以下几步将start转换成end:

```text
RXXLRXRXL ->
XRXLRXRXL ->
XRLXRXRXL ->
XRLXXRRXL ->
XRLXXRRLX
```

__提示:__

1 <= len(start) = len(end) <= 10000。
start和end中的字符串仅限于'L', 'R'和'X'。

__思路__:

双指针
将 'L' 看成左转的人, 'R' 看成右转的人, 'X' 看成空位
题意即, 将所有人聚集, 两个队列是否完全相等
找到第一个不为 'X' 的人, 如果两个队列该位置不想等, 直接返回 false, 因为 'L' 和 'R' 不能交换
如果 start[i] == 'L' and i < j, 直接返回 false, 因为 'L' 只能往左移动, 如果 i < j, start[i] 永远也不能移动到 end[j] 对应的位置上
start[j] == 'R' and i > j 同理
注意边界条件
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canTransform(string start, string end) 
    {
        for (int i = 0, j = 0, n = start.size(); i < n or j < n; i++, j++) 
        {
            while (i < n and start[i] == 'X') ++i;
            while (j < n and end[j] == 'X') ++j;
            if ((i < n) ^ (j < n)) return false;
            if (i < n and ((start[i] != end[j]) or (start[i] == 'R' && i > j) or (start[i] == 'L' and i < j))) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canTransform(String start, String end) {
        for (int i = 0, j = 0, n = start.length(); i < n || j < n; i++, j++) {
            while (i < n && start.charAt(i) == 'X') ++i;
            while (j < n && end.charAt(j) == 'X') ++j;
            if ((i < n) ^ (j < n)) return false;
            if (i < n && ((start.charAt(i) != end.charAt(j)) || (start.charAt(i) == 'R' && i > j) || (start.charAt(i) == 'L' && i < j))) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canTransform(self, start: str, end: str) -> bool:
        if (s := start.replace('X', '')) != end.replace('X', ''):
            return False
        start, end = [i for i, c in enumerate(start) if c != 'X'], [i for i, c in enumerate(end) if c != 'X']
        return not any((c == 'R' and start[i] > end[i]) or (c == 'L' and start[i] < end[i]) for i, c in enumerate(s))
```
