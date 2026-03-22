# 1012 Numbers With Repeated Digits 至少有 1 位重复的数字

__Description__:
Given an integer n, return the number of positive integers in the range [1, n] that have at least one repeated digit.

__Example:__

Example 1:

Input: n = 20
Output: 1
Explanation: The only positive number (<= 20) with at least 1 repeated digit is 11.

Example 2:

Input: n = 100
Output: 10
Explanation: The positive numbers (<= 100) with atleast 1 repeated digit are 11, 22, 33, 44, 55, 66, 77, 88, 99, and 100.

Example 3:

Input: n = 1000
Output: 262

__Constraints:__

1 <= n <= 10^9

__题目描述__:
给定正整数 n，返回在 [1, n] 范围内具有 至少 1 位 重复数字的正整数的个数。

__示例 :__

示例 1：

输入：n = 20
输出：1
解释：具有至少 1 位重复数字的正数（<= 20）只有 11 。

示例 2：

输入：n = 100
输出：10
解释：具有至少 1 位重复数字的正数（<= 100）有 11，22，33，44，55，66，77，88，99 和 100 。

示例 3：

输入：n = 1000
输出：262

__提示:__

1 <= n <= 10^9

__思路__:

数学
以 4869 为例
按照高位是否为 0 分成两种情况
如果高位有 0:
0XXX -> 9 \* A(9, 2)
00XX -> 9 \* A(9, 1)
000X -> 9 \* A(9, 0)
其中 A(m, n) = m! / (m - n)!, 可以预先计算 m! 备用, 注意到最多为 10 位, 计算 10 以内的阶乘就够
如果高位不含 0:
1-3XXX -> 3 \* A(9, 3)
4(0-8)XX -> 9 \* A(8, 2)
48(0-6)X -> 7 \* A(7, 1)
4869 -> 9
注意在这里面需要去掉重复的
时间复杂度为 O(lgn \* lgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numDupDigitsAtMostN(int n) 
    {
        fact = vector<int>(10, 1);
        for (int i = 1; i < 10; i++) fact[i] *= fact[i - 1] * i;
        return n - helper(n);
    }
private:
    vector<int> fact;
    
    int helper(int n) 
    {
        string digits = to_string(n);
        vector<int> used(10, 0);
        int k = digits.size(), total = 0;
        for (int i = 1; i < k; i++) total += 9 * A(9, i - 1);
        for (int i = k - 1; i >= 0; i--) 
        {
            int num = digits[k - 1 - i] - '0';
            for (int j = i == k - 1 ? 1 : 0; j < num; j++) 
            {
                if (used[j] != 0) continue;
                total += A(10 - (k - i), i);
            }
            if (++used[num] > 1) break;
            if (!i) ++total;
        }
        return total;
    }

    int A(int m, int n) 
    {
        return fact[m] / fact[m - n];
    }
};
```

__Java__:

```Java
class Solution {
    private int[] fact;
    public int numDupDigitsAtMostN(int n) {
        fact = new int[10];
        Arrays.fill(fact, 1);
        for (int i = 1; i < 10; i++) fact[i] *= fact[i - 1] * i;
        return n - helper(n);
    }

    private int helper(int n) {
        List<Integer> digits = new ArrayList<>();
        while (n > 0) {
            digits.add(n % 10);
            n /= 10;
        }
        int k = digits.size(), total = 0, used[] = new int[10];
        for (int i = 1; i < k; i++) total += 9 * A(9, i - 1);
        for (int i = k - 1; i >= 0; i--) {
            int num = digits.get(i);
            for (int j = i == k - 1 ? 1 : 0; j < num; j++) {
                if (used[j] != 0) continue;
                total += A(10 - (k - i), i);
            }
            if (++used[num] > 1) break;
            if (i == 0) ++total;
        }
        return total;
    }

    private int A(int m, int n) {
        return fact[m] / fact[m - n];
    }
}
```

__Python__:

```Python
class Solution:
    def numDupDigitsAtMostN(self, n: int) -> int:
        m, cur, d = len((s := str(n))), 0, {}
        for i in range(1, m + 1):
            cur += 9 * factorial(9) / factorial(9 - i + 1)
        for i in range(m):
            a = int(s[i])
            for j in range(a + 1, 10):
                if j not in d:
                    cur -= factorial(9 - len(d)) / factorial(9 - len(d) - m + 1 + i)
            if a in d:
                break
            d[a] = 1
        return int(n - cur)
```
