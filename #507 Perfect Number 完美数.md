__Description__:
We define the Perfect Number is a positive integer that is equal to the sum of all its positive divisors except itself.

Now, given an integer n, write a function that returns true when it is a perfect number and false when it is not.

__Example:__
Input: 28
Output: True
Explanation: 28 = 1 + 2 + 4 + 7 + 14

__Note:__
The input number n will not exceed 100,000,000. (1e8)

__题目描述__:
对于一个 正整数，如果它和除了它自身以外的所有正因子之和相等，我们称它为“完美数”。

给定一个 正整数 n， 如果他是完美数，返回 True，否则返回 False

__示例：__

输入: 28
输出: True
解释: 28 = 1 + 2 + 4 + 7 + 14

__注意:__
输入的数字 n 不会超过 100,000,000. (1e8)

__思路__:
1. 最快方法是算出几个完美数, 判断相等就行
时间复杂度O(1), 空间复杂度O(1)
2. 计算出所有正因子相加即可, 边界是 sqrt(num), 需要判断 num是否是完全平方数
时间复杂度O(n ^ 1/2), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool checkPerfectNumber(int num) {
        if (num < 0) return false;
        int sum = 1, s = sqrt(num);
        if (s * s == num) sum += s--;
        while (s > 1) {
            if (!(num % s)) sum += num / s + s;
            s--;
        }
        return sum == num;
    }
};
```

__Java__:
```
class Solution {
    public boolean checkPerfectNumber(int num) {
        int sum = 1;
        int i = 1;
        double sqrt = Math.sqrt(num);
        while (++i < sqrt) if (num % i == 0) sum += i + num / i;
        if (i * i == num) sum += i;
        return sum == num && num != 1;
    }
}
```

__Python__:
```
class Solution:
    def checkPerfectNumber(self, num: int) -> bool:
        return num in [6, 28, 496, 8128, 33550336]
```
