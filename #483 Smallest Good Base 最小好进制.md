# 483 Smallest Good Base 最小好进制

__Description__:
For an integer n, we call k>=2 a good base of n, if all digits of n base k are 1.

Now given a string representing n, you should return the smallest good base of n in string format.

__Example:__

Example 1:

Input: "13"
Output: "3"
Explanation: 13 base 3 is 111.

Example 2:

Input: "4681"
Output: "8"
Explanation: 4681 base 8 is 11111.

Example 3:

Input: "1000000000000000000"
Output: "999999999999999999"
Explanation: 1000000000000000000 base 999999999999999999 is 11.

__Note:__

The range of n is [3, 10^18].
The string representing n is always valid and will not have leading zeros.

__题目描述__:
对于给定的整数 n, 如果n的k（k>=2）进制数的所有数位全为1，则称 k（k>=2）是 n 的一个好进制。

以字符串的形式给出 n, 以字符串的形式返回 n 的最小好进制。

__示例 :__

示例 1：

输入："13"
输出："3"
解释：13 的 3 进制是 111。

示例 2：

输入："4681"
输出："8"
解释：4681 的 8 进制是 11111。

示例 3：

输入："1000000000000000000"
输出："999999999999999999"
解释：1000000000000000000 的 999999999999999999 进制是 11。

__提示:__

n的取值范围是 [3, 10^18]。
输入总是有效且没有前导 0。

__思路__:

假设这个数为 k进制, 一共长为 m
n = k ^ (m - 1) + k ^ (m - 2) + ... + k + 1 = k ^ m - 1 / (k - 1)
当这个数为 2进制时, m能取到最大值, 最小值为 2
可以用二分法加快搜索
时间复杂度O(nlglgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string smallestGoodBase(string n) 
    {
        long num = stol(n);
        for (int m = (int)(log(num + 1) / log(2)); m >= 2; m--) 
        {
            long left = 2, right = (long)pow(num, 1.0 / (m - 1)) + 1;
            while (left < right) 
            {
                long mid = left + ((right - left) >> 1), sum = 0;
                for (int j = 0; j < m; j++) sum = sum * mid + 1;
                if (sum == num) return to_string(mid);
                else if (sum < num) left = mid + 1;
                else right = mid;
            }
        }
        return to_string(num - 1);
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestGoodBase(String n) {
        long num = Long.parseLong(n);
        for (int m = (int)(Math.log(num + 1) / Math.log(2)); m >= 2; m--) {
            long left = 2, right = (long)Math.pow(num, 1.0 / (m - 1)) + 1;
            while (left < right) {
                long mid = left + ((right - left) >> 1), sum = 0;
                for (int j = 0; j < m; j++) sum = sum * mid + 1;
                if (sum == num) return String.valueOf(mid);
                else if (sum < num) left = mid + 1;
                else right = mid;
            }
        }
        return String.valueOf(num - 1);
    }
}
```

__Python__:

```Python
class Solution:
    def smallestGoodBase(self, n: str) -> str:
        num = int(n)
        for m in range(num.bit_length(), 2, -1):
            x = int(num ** (1 / (m - 1)))
            if num == (x ** m - 1) // (x - 1):
                return str(x)
        return str(num - 1)
```
