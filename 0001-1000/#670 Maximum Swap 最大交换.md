# 670 Maximum Swap 最大交换

__Description__:
You are given an integer num. You can swap two digits at most once to get the maximum valued number.

Return the maximum valued number you can get.

__Example:__

Example 1:

Input: num = 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.

Example 2:

Input: num = 9973
Output: 9973
Explanation: No swap.

__Constraints:__

0 <= num <= 10^8

__题目描述__:
给定一个非负整数，你至多可以交换一次数字中的任意两位。返回你能得到的最大值。

__示例 :__

示例 1 :

输入: 2736
输出: 7236
解释: 交换数字2和数字7。

示例 2 :

输入: 9973
输出: 9973
解释: 不需要交换。

__注意:__

给定数字的范围是 [0, 10^8]

__思路__:

贪心法

1. 转化为字符串之后排序获得最大排列
从前往后查找到第一个不与最大值一致的下标
从后往前查找到第一个与当前最大值下标对应的值
将两者交换即可
时间复杂度 O(nlgn), 空间复杂度 O(n)
2. 记录每个数字最后一次出现的下标
从左往右遍历到当前位置对应的右边的最大数字并交换, 然后直接返回
没找到的情况下循环结束后返回, 这时没发生交换
时间复杂度 O(n), 空间复杂度 O(1), 额外空间只需要常数(10)大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maximumSwap(int num) 
    {
        string s = to_string(num);
        int n = s.size();
        vector<int> last(10, 0);
        for (int i = 0; i < n; i++) last[s[i] - '0'] = i;
        for (int i = 0; i < n - 1; i++)
        {
            for (int d = 9; d > s[i] - '0'; d--)
            {
                if (last[d] > i)
                {
                    swap(s[i], s[last[d]]);
                    return stoi(s);
                }
            }
        }
        return num;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumSwap(int num) {
        char[] s = String.valueOf(num).toCharArray();
        int n = s.length, last[] = new int[10];
        for (int i = 0; i < n; i++) last[s[i] - '0'] = i;
        for (int i = 0; i < n - 1; i++) {
            for (int d = 9; d > s[i] - '0'; d--) {
                if (last[d] > i) {
                    swap(s, i, last[d]);
                    return Integer.parseInt(new String(s));
                }
            }
        }
        return num;
    }
    
    private void swap(char[] s, int i, int j) {
        s[i] ^= s[j];
        s[j] ^= s[i];
        s[i] ^= s[j];
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSwap(self, num: int) -> int:
        num = list(str(num))
        max_num, n = sorted(num, reverse=True), len(num)
        for i in range(n):
            if num[i] != max_num[i]:
                num[i], num[n - num[::-1].index(max_num[i]) - 1] = max_num[i], num[i]
                break
        return int(''.join(num))
```
