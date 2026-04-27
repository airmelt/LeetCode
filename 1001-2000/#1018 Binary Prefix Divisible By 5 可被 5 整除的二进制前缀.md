# 1018 Binary Prefix Divisible By 5 可被 5 整除的二进制前缀

__Description__:
Given an array A of 0s and 1s, consider N_i: the i-th subarray from A[0] to A[i] interpreted as a binary number (from most-significant-bit to least-significant-bit.)

Return a list of booleans answer, where answer[i] is true if and only if N_i is divisible by 5.

__Example:__

Example 1:

Input: [0,1,1]
Output: [true,false,false]
Explanation:
The input numbers in binary are 0, 01, 011; which are 0, 1, and 3 in base-10.  Only the first number is divisible by 5, so answer[0] is true.

Example 2:

Input: [1,1,1]
Output: [false,false,false]

Example 3:

Input: [0,1,1,1,1,1]
Output: [true,false,false,false,true,false]

Example 4:

Input: [1,1,1,0,1]
Output: [false,false,false,false,false]

__Note:__

1 <= A.length <= 30000
A[i] is 0 or 1

__题目描述__:
给定由若干 0 和 1 组成的数组 A。我们定义 N_i：从 A[0] 到 A[i] 的第 i 个子数组被解释为一个二进制数（从最高有效位到最低有效位）。

返回布尔值列表 answer，只有当 N_i 可以被 5 整除时，答案 answer[i] 为 true，否则为 false。

__示例 :__

示例 1：

输入：[0,1,1]
输出：[true,false,false]
解释：
输入数字为 0, 01, 011；也就是十进制中的 0, 1, 3 。只有第一个数可以被 5 整除，因此 answer[0] 为真。

示例 2：

输入：[1,1,1]
输出：[false,false,false]

示例 3：

输入：[0,1,1,1,1,1]
输出：[true,false,false,false,true,false]

示例 4：

输入：[1,1,1,0,1]
输出：[false,false,false,false,false]

__提示：__

1 <= A.length <= 30000
A[i] 为 0 或 1

__思路__:

不能将数组 A转化成 int, 因为数组 A长度 30000超出了 int的范围(32)
注意到能整除 5的只有个位为 5或者 0的数, 只需要统计个位数(% 10)即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<bool> prefixesDivBy5(vector<int>& A) 
    {
        vector<bool> result(A.size(), false);
        int i = 0, count = 0;
        for (auto a : A)
        {
            count <<= 1;
            count += a;
            count %= 10;
            if (!(count % 5)) result[i] = true;
            i++;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Boolean> prefixesDivBy5(int[] A) {
        List<Boolean> result = new ArrayList<>(A.length);
        int count = 0;
        for (int a : A) {
            count <<= 1;
            count += a;
            count %= 10;
            result.add(count % 5 == 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def prefixesDivBy5(self, A: List[int]) -> List[bool]:
        result, count = [], 0
        for a in A:
            count = ((count << 1) + a) % 10
            result.append(count % 5 == 0)
        return result
```
