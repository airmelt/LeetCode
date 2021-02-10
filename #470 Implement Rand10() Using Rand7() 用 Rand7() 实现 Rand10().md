# 470 Implement Rand10() Using Rand7() 用 Rand7() 实现 Rand10()

__Description__:
Given the API rand7() that generates a uniform random integer in the range [1, 7], write a function rand10() that generates a uniform random integer in the range [1, 10]. You can only call the API rand7(), and you shouldn't call any other API. Please do not use a language's built-in random API.

Each test case will have one internal argument n, the number of times that your implemented function rand10() will be called while testing. Note that this is not an argument passed to rand10().

__Follow up:__

What is the expected value for the number of calls to rand7() function?
Could you minimize the number of calls to rand7()?

__Example:__

Example 1:

Input: n = 1
Output: [2]

Example 2:

Input: n = 2
Output: [2,8]

Example 3:

Input: n = 3
Output: [3,8,10]

__Constraints:__

1 <= n <= 10^5

__题目描述__:
已有方法 rand7 可生成 1 到 7 范围内的均匀随机整数，试写一个方法 rand10 生成 1 到 10 范围内的均匀随机整数。

不要使用系统的 Math.random() 方法。

__示例 :__

示例 1:

输入: 1
输出: [7]

示例 2:

输入: 2
输出: [8,4]

示例 3:

输入: 3
输出: [8,1,10]

__提示:__

rand7 已定义。
传入参数: n 表示 rand10 的调用次数。

__进阶:__

rand7()调用次数的 期望值 是多少 ?
你能否尽量少调用 rand7() ?

__思路__:

用乘法或者加法构造均匀分布的 1-10的数
比如 7 \* 7 = 49, a = rand7(), b = rand7(), 则 b等概率的可以是1, 2, 3, 4, 5, 6, 7
那么 (b - 1) \* 7等概率的为 (0, 7, 14, 21, 28, 35, 42), 再加上 a, 就可以组合出 1-49中的任何一个, 去掉大于 40的部分, 剩下的 40个数等概率出现, 这样就可以等概率取到 10个数中的一个
每次调用 2次 rand7(), 每次调用成功的概率为 40 / 49, 调用的期望值为 2 + 2 \* 9 / 49 + 2 \* (9 / 49) ^ 2 + ... = 2 \* 1 / (1 - 9 / 49) = 80 / 49
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

class Solution 
{
public:
    int rand10() 
    {
        int result = INT_MAX;
        while (result > 40) result = (rand7() - 1) * 7 + rand7();
        return result % 10 + 1;
    }
};
```

__Java__:

```Java
/**
 * The rand7() API is already defined in the parent class SolBase.
 * public int rand7();
 * @return a random integer in the range 1 to 7
 */
class Solution extends SolBase {
    public int rand10() {
        int result = Integer.MAX_VALUE;
        while (result > 40) result = (rand7() - 1) * 7 + rand7();
        return result % 10 + 1;
    }
}
```

__Python__:

```Python
# The rand7() API is already defined for you.
# def rand7():
# @return a random integer in the range 1 to 7

class Solution:
    def rand10(self):
        """
        :rtype: int
        """
        result = float('inf')
        while result > 40:
            result = (rand7() - 1) * 7 + rand7()
        return result % 10 + 1
```
