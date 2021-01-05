# 949 Largest Time for Given Digits 给定数字能组成的最大时间

__Description__:
Given an array of 4 digits, return the largest 24 hour time that can be made.

The smallest 24 hour time is 00:00, and the largest is 23:59.  Starting from 00:00, a time is larger if more time has elapsed since midnight.

Return the answer as a string of length 5.  If no valid time can be made, return an empty string.

__Example:__

Example 1:

Input: [1,2,3,4]
Output: "23:41"

Example 2:

Input: [5,5,5,5]
Output: ""

__Note:__

A.length == 4
0 <= A[i] <= 9

__题目描述__:
给定一个由 4 位数字组成的数组，返回可以设置的符合 24 小时制的最大时间。

最小的 24 小时制时间是 00:00，而最大的是 23:59。从 00:00 （午夜）开始算起，过得越久，时间越大。

以长度为 5 的字符串返回答案。如果不能确定有效时间，则返回空字符串。

__示例 :__

示例 1：

输入：[1,2,3,4]
输出："23:41"
示例 2：

输入：[5,5,5,5]
输出：""

__提示：__

A.length == 4
0 <= A[i] <= 9

__思路__:

1. 检查每一个组合, 取最大时间即可
2. 排序之后用排列组合找到满足时间格式的输出即可
时间复杂度O(1), 空间复杂度O(1), 最多检查 24(4!)组结果

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string largestTimeFromDigits(vector<int>& A) 
    {
        sort(A.begin(), A.end(), greater<int>());
        do 
        {
            if (A[0] * 10 + A[1] < 24 and A[2] * 10 + A[3] < 60) return to_string(A[0]) + to_string(A[1]) + ":" + to_string(A[2]) + to_string(A[3]);
        } while (next_permutation(A.begin(), A.end(), greater<int>()));
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String largestTimeFromDigits(int[] A) {
        int result = -1;
        for (int i = 0; i < 4; i++) {
            for (int j = 0; j < 4; j++) {
                if (j != i) {
                    for (int k = 0; k < 4; k++) {
                        if (k != i && k != j) {
                            int l = 6 - i - j - k;
                            int hour = 10 * A[i] + A[j], minute = 10 * A[k] + A[l];
                            if (hour < 24 && minute < 60) result = Math.max(result, 100 * hour + minute);
                        }
                    }
                }
            }
        }
        return result >= 0 ? String.format("%02d:%02d", result / 100, result % 100) : "";
    }
}
```

__Python__:

```Python
class Solution:
    def largestTimeFromDigits(self, A: List[int]) -> str:
        A.sort(reverse=True)
        p = itertools.permutations(A)
        for i, j, k, t in p:
            if 10 * i + j < 24 and k * 10 + t < 60:
                return str(i) + str(j) + ":" + str(k) + str(t)
        return ""
```
