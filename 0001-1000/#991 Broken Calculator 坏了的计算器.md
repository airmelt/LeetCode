# 991 Broken Calculator 坏了的计算器

__Description__:
There is a broken calculator that has the integer startValue on its display initially. In one operation, you can:

multiply the number on display by 2, or
subtract 1 from the number on display.
Given two integers startValue and target, return the minimum number of operations needed to display target on the calculator.

__Example:__

Example 1:

Input: startValue = 2, target = 3
Output: 2
Explanation: Use double operation and then decrement operation {2 -> 4 -> 3}.

Example 2:

Input: startValue = 5, target = 8
Output: 2
Explanation: Use decrement and then double {5 -> 4 -> 8}.

Example 3:

Input: startValue = 3, target = 10
Output: 3
Explanation: Use double, decrement and double {3 -> 6 -> 5 -> 10}.

__Constraints:__

1 <= x, y <= 10^9

__题目描述__:
在显示着数字的坏计算器上，我们可以执行以下两种操作：

双倍（Double）：将显示屏上的数字乘 2；
递减（Decrement）：将显示屏上的数字减 1 。
最初，计算器显示数字 X。

返回显示数字 Y 所需的最小操作数。

__示例 :__

示例 1：

输入：X = 2, Y = 3
输出：2
解释：先进行双倍运算，然后再进行递减运算 {2 -> 4 -> 3}.

示例 2：

输入：X = 5, Y = 8
输出：2
解释：先递减，再双倍 {5 -> 4 -> 8}.

示例 3：

输入：X = 3, Y = 10
输出：3
解释：先双倍，然后递减，再双倍 {3 -> 6 -> 5 -> 10}.

示例 4：

输入：X = 1024, Y = 1
输出：1023
解释：执行递减运算 1023 次

__提示:__

1 <= X <= 10^9
1 <= Y <= 10^9

__思路__:

模拟
用 target 模拟到 startValue 这个过程
如果 target <= startValue, 只能逐个递增
如果 target 是偶数, 直接减半, 因为乘法肯定比减法更快
否则 target ➕ 1
时间复杂度为 O(lgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int brokenCalc(int startValue, int target) 
    {
        return startValue >= target ? startValue - target : (!(target & 1) ? brokenCalc(startValue, target >> 1) + 1 : brokenCalc(startValue, target + 1) + 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int brokenCalc(int startValue, int target) {
        int result = 0;
        while (target > startValue) {
            if ((target & 1) == 0) target >>>= 1;
            else ++target;
            ++result;
        }
        return result + startValue - target;
    }
}
```

__Python__:

```Python
class Solution:
    def brokenCalc(self, startValue: int, target: int) -> int:
        return startValue - target if startValue >= target else self.brokenCalc(startValue, target >> 1) + 1 if not target & 1 else self.brokenCalc(startValue, target + 1) + 1
```
