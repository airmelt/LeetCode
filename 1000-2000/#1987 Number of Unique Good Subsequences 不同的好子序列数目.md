# 1987 Number of Unique Good Subsequences 不同的好子序列数目

__Description:__

You are given a binary string `binary`. A __subsequence__ of `binary` is considered __good__ if it is __not empty__ and has __no leading zeros__ (with the exception of `"0"`).

Find the number of __unique good subsequences__ of `binary`.

- For example, if `binary = "001"`, then all the __good__ subsequences are `["0", "0", "1"]`, so the __unique__ good subsequences are `"0"` and `"1"`. Note that subsequences `"00"`, `"01"`, and `"001"` are not good because they have leading zeros.

Return _the number of __unique good subsequences__ of_ `binary`. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: binary = "001"
Output: 2
Explanation: The good subsequences of binary are ["0", "0", "1"].
The unique good subsequences are "0" and "1".
```

Example 2:

```text
Input: binary = "11"
Output: 2
Explanation: The good subsequences of binary are ["1", "1", "11"].
The unique good subsequences are "1" and "11".
```

Example 3:

```text
Input: binary = "101"
Output: 5
Explanation: The good subsequences of binary are ["1", "0", "1", "10", "11", "101"]. 
The unique good subsequences are "0", "1", "10", "11", and "101".
```

__Constraints:__

- `1 <= binary.length <= 10 ^ 5`
- `binary` consists of only `'0'`s and `'1'`s.

__题目描述:__

给你一个二进制字符串 `binary` 。 `binary` 的一个 __子序列__ 如果是 __非空__ 的且没有 _前导_ __0__ （除非数字是 `"0"` 本身），那么它就是一个 __好__ 的子序列。

请你找到 `binary` __不同好子序列__ 的数目。

- 比方说，如果 `binary = "001"` ，那么所有 __好__ 子序列为 `["0", "0", "1"]` ，所以 _不同_ 的好子序列为 `"0"` 和 `"1"` 。 注意，子序列 `"00"` ， `"01"` 和 `"001"` 不是好的，因为它们有前导 0 。

请你返回 `binary` 中 __不同好子序列__ 的数目。由于答案可能很大，请将它对 `10 ^ 9 + 7` __取余__ 后返回。

一个 子序列 指的是从原数组中删除若干个（可以一个也不删除）元素后，不改变剩余元素顺序得到的序列。

__示例:__

示例 1：

```text
输入：binary = "001"
输出：2
解释：好的二进制子序列为 ["0", "0", "1"] 。
不同的好子序列为 "0" 和 "1" 。
```

示例 2：

```text
输入：binary = "11"
输出：2
解释：好的二进制子序列为 ["1", "1", "11"] 。
不同的好子序列为 "1" 和 "11" 。
```

示例 3：

```text
输入：binary = "101"
输出：5
解释：好的二进制子序列为 ["1", "0", "1", "10", "11", "101"] 。
不同的好子序列为 "0" ，"1" ，"10" ，"11" 和 "101" 。
```

__提示：__

- `1 <= binary.length <= 10 ^ 5`
- `binary` 只含有 `'0'` 和 `'1'` 。

__思路:__

```text
动态规划
设 dp0[i] 表示以 '0' 开头的子序列的数目
设 dp1[i] 表示以 '1' 开头的子序列的数目
从后往前遍历字符串
如果当前字符为 '0'，则以 '0' 开头的子序列的数目为 dp0[i] = dp0[i + 1] + dp1[i + 1] + 1, 第 i 个字符可以添加到 '0'/'1' 开头的所有子序列的前面, 也可以单独作为一个新的 '0' 开头的好子序列, 所以加 1
dp1[i] = dp1[i + 1], 因为 '0' 不能作为 '1' 开头的子序列的前面
同理, 如果当前字符为 '1', dp1[i] = dp0[i + 1] + dp1[i + 1] + 1, dp0[i] = dp0[i + 1]
最后返回 (dp1[0] + flag) % MOD, 其中 flag 表示是否存在 '0', 如果存在 '0', 则加 1 表示 '0' 本身也是一个好子序列
注意到 dp0[i] 和 dp1[i] 只与 dp0[i + 1] 和 dp1[i + 1] 有关, 所以可以使用滚动数组进行优化
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfUniqueGoodSubsequences(string binary) 
    {
        int n = binary.size(), dp0 = 0, dp1 = 0, MOD = 1e9 + 7, flag = 0;
        for (int i = n - 1; ~i; i--) 
        {
            if (binary[i] == '0') 
            {
                flag = 1;
                dp0 = (dp0 + dp1 + 1) % MOD;
            }
            else dp1 = (dp0 + dp1 + 1) % MOD;
        }
        return (dp1 + flag) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfUniqueGoodSubsequences(String binary) {
        int n = binary.length(), dp0 = 0, dp1 = 0, MOD = 1_000_000_007, flag = 0;
        for (int i = n - 1; i > -1; i--) {
            if (binary.charAt(i) == '0') {
                flag = 1;
                dp0 = (dp0 + dp1 + 1) % MOD;
            }
            else dp1 = (dp0 + dp1 + 1) % MOD;
        }
        return (dp1 + flag) % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfUniqueGoodSubsequences(self, binary: str) -> int:
        n, dp0, dp1, MOD = len(binary), 0, 0, 10 ** 9 + 7
        for i in range(n - 1, -1, -1):
            if binary[i] == '0':
                dp0 = (dp0 + dp1 + 1) % MOD
            else:
                dp1 = (dp0 + dp1 + 1) % MOD
        return (dp1 + ('0' in binary)) % MOD
```
