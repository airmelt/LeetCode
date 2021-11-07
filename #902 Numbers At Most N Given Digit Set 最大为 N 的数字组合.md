# 902 Numbers At Most N Given Digit Set 最大为 N 的数字组合

__Description__:
Given an array of digits which is sorted in non-decreasing order. You can write numbers using each digits[i] as many times as we want. For example, if digits = ['1','3','5'], we may write numbers such as '13', '551', and '1351315'.

Return the number of positive integers that can be generated that are less than or equal to a given integer n.

__Example:__

Example 1:

Input: digits = ["1","3","5","7"], n = 100
Output: 20
Explanation:
The 20 numbers that can be written are:
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.

Example 2:

Input: digits = ["1","4","9"], n = 1000000000
Output: 29523
Explanation:
We can write 3 one digit numbers, 9 two digit numbers, 27 three digit numbers,
81 four digit numbers, 243 five digit numbers, 729 six digit numbers,
2187 seven digit numbers, 6561 eight digit numbers, and 19683 nine digit numbers.
In total, this is 29523 integers that can be written using the digits array.

Example 3:

Input: digits = ["7"], n = 8
Output: 1

__Constraints:__

1 <= digits.length <= 9
digits[i].length == 1
digits[i] is a digit from '1' to '9'.
All the values in digits are unique.
digits is sorted in non-decreasing order.
1 <= n <= 10^9

__题目描述__:
我们有一组排序的数字 D，它是  {'1','2','3','4','5','6','7','8','9'} 的非空子集。（请注意，'0' 不包括在内。）

现在，我们用这些数字进行组合写数字，想用多少次就用多少次。例如 D = {'1','3','5'}，我们可以写出像 '13', '551', '1351315' 这样的数字。

返回可以用 D 中的数字写出的小于或等于 N 的正整数的数目。

__示例 :__

示例 1：

输入：D = ["1","3","5","7"], N = 100
输出：20
解释：
可写出的 20 个数字是：
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.

示例 2：

输入：D = ["1","4","9"], N = 1000000000
输出：29523
解释：
我们可以写 3 个一位数字，9 个两位数字，27 个三位数字，
81 个四位数字，243 个五位数字，729 个六位数字，
2187 个七位数字，6561 个八位数字和 19683 个九位数字。
总共，可以使用D中的数字写出 29523 个整数。

__提示:__

D 是按排序顺序的数字 '1'-'9' 的子集。
1 <= N <= 10^9

__思路__:

数位 dp
令 s 为 n 的字符串形式, k = len(s), m = len(digits)
设 dp[i] 表示从低位开始能表示成小于 s[k - i:] 的个数
比如 1234, dp[1] 表示小于 4 的组合个数
对于每一个 digits 中的元素 cur
如果 s 当前的元素大于 cur, dp[i + 1] += pow(m, i), 即后面每一位都可以加入
如果 s 当前的元素等于 cur, dp[i + 1] += dp[i], 即当前位等于前一位的组合数
否则 dp[i + 1] += 0, 此时不可能能组成小于 n 的数字
dp[k] 最后需要加上所有 pow(m, i), 0 < i < k, 因为最高位总能加上所有的低位组合
时间复杂度为 O(lgn), 空间复杂度为 O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) 
    {
        string s = to_string(n);
        int m = digits.size(), k = s.size();
        vector<int> dp(k + 1), nums(m);
        dp.front() = 1;
        for (int i = 0; i < m; i++) nums[i] = stoi(digits[i]);
        for (int i = 0, cur = s[k - i - 1] - '0'; i < k; i++, cur = s[k - (i < k ? i : k - 1) - 1] - '0') for (const auto& num : nums) dp[i + 1] += cur < num ? 0 : (cur == num ? dp[i] : pow(m, i));
        for (int i = 1; i < k; i++) dp[k] += pow(m, i);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        String s = String.valueOf(n);
        int m = digits.length, k = s.length(), dp[] = new int[k + 1], nums[] = new int[m];
        dp[0] = 1;
        for (int i = 0; i < m; i++) nums[i] = Integer.valueOf(digits[i]);
        for (int i = 0, cur = s.charAt(k - i - 1) - '0'; i < k; i++, cur = s.charAt(k - (i < k ? i : k - 1) - 1) - '0') for (int num : nums) dp[i + 1] += cur < num ? 0 : (cur == num ? dp[i] : (int)Math.pow(m, i));
        for (int i = 1; i < k; i++) dp[k] += (int)Math.pow(m, i);
        return dp[k];
    }
}
```

__Python__:

```Python
class Solution:
    def atMostNGivenDigitSet(self, digits: List[str], n: int) -> int:
        dp, m = [1] + [0] * (k := len((s := str(n)))), len(digits)
        for i, cur in enumerate(s[::-1]):
            for num in digits:
                dp[i + 1] += 0 if cur < num else dp[i] if cur == num else m ** i
        for i in range(1, k):
            dp[k] += m ** i
        return dp[-1]
```
