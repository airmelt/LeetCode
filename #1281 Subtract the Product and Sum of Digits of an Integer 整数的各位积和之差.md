# 1281 Subtract the Product and Sum of Digits of an Integer 整数的各位积和之差

__Description__:
Given an integer number n, return the difference between the product of its digits and the sum of its digits.

__Example:__

Example 1:

Input: n = 234
Output: 15
Explanation:
Product of digits = 2 \* 3 \* 4 = 24
Sum of digits = 2 + 3 + 4 = 9
Result = 24 - 9 = 15

Example 2:

Input: n = 4421
Output: 21
Explanation:
Product of digits = 4 \* 4 \* 2 \* 1 = 32
Sum of digits = 4 + 4 + 2 + 1 = 11
Result = 32 - 11 = 21

__Constraints:__

1 <= n <= 10^5

__题目描述__:
给你一个整数 n，请你帮忙计算并返回该整数「各位数字之积」与「各位数字之和」的差。

__示例 :__

示例 1：

输入：n = 234
输出：15
解释：
各位数之积 = 2 \* 3 \* 4 = 24
各位数之和 = 2 + 3 + 4 = 9
结果 = 24 - 9 = 15

示例 2：

输入：n = 4421
输出：21
解释：
各位数之积 = 4 \* 4 \* 2 \* 1 = 32
各位数之和 = 4 + 4 + 2 + 1 = 11
结果 = 32 - 11 = 21

__提示：__

1 <= n <= 10^5

__思路__:

用取模算出乘积和和即可
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int subtractProductAndSum(int n) 
    {
        int product = 1, sum = 0;
        while (n)
        {
            product *= n % 10;
            sum += n % 10;
            n /= 10;
        }
        return product - sum;
    }
};
```

__Java__:

```Java
class Solution {
    public int subtractProductAndSum(int n) {
        int product = 1, sum = 0;
        while (n != 0) {
            product *= n % 10;
            sum += n % 10;
            n /= 10;
        }
        return product - sum;
    }
}
```

__Python__:

```Python
from functools import reduce
class Solution:
    def subtractProductAndSum(self, n: int) -> int:
        return reduce(lambda x, y: x * y, [int(i) for i in str(n)]) - sum(int(i) for i in str(n))
```
