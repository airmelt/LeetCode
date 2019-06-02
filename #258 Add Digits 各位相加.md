__Description__:
Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

**Example :**
Input: 38
Output: 2

__Explanation:__
 The process is like: 3 + 8 = 11, 1 + 1 = 2.
             Since 2 has only one digit, return it.

__Follow up__:
Could you do it without any loop/recursion in O(1) runtime?

__题目描述__:
给定一个非负整数 num，反复将各个位上的数字相加，直到结果为一位数。

**示例 :**
输入: 38
输出: 2

__解释:__
各位相加的过程为：3 + 8 = 11, 1 + 1 = 2。
由于 2 是一位数，所以返回 2。

__进阶:__
你可以不使用循环或者递归，且在 O(1) 时间复杂度内解决这个问题吗？

__思路__:
可以输入几个数, 找规律, 最后输出的答案应该是 % 9的结果
注意边界条件
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int addDigits(int num) {
        if (!num) return 0;
        return num % 9 == 0 ? 9 : num % 9;
    }
};
```

__Java__:
```
class Solution {
    public int addDigits(int num) {
        while (num > 9) {
            int temp = 0;
            while (num > 0) {
                temp += num % 10;
                num /= 10;
            }
            num = temp;
        }
        return num;
    }
}
```

__Python__:
```
class Solution:
    def addDigits(self, num: int) -> int:
        if num and num % 9 == 0:
            return 9
        return num % 9
```
