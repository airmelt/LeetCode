# 1362 Closest Divisors 最接近的因数

__Description:__

Given an integer num, find the closest two integers in absolute difference whose product equals num + 1 or num + 2.

Return the two integers in any order.

__Example:__

Example 1:

Input: num = 8
Output: [3,3]
Explanation: For num + 1 = 9, the closest divisors are 3 & 3, for num + 2 = 10, the closest divisors are 2 & 5, hence 3 & 3 is chosen.

Example 2:

Input: num = 123
Output: [5,25]

Example 3:

Input: num = 999
Output: [40,25]

__Constraints:__

1 <= num <= 10^9

__题目描述：__

给你一个整数 num，请你找出同时满足下面全部要求的两个整数：

两数乘积等于  num + 1 或 num + 2
以绝对差进行度量，两数大小最接近
你可以按任意顺序返回这两个整数。

__示例：__

示例 1：

输入：num = 8
输出：[3,3]
解释：对于 num + 1 = 9，最接近的两个因数是 3 & 3；对于 num + 2 = 10, 最接近的两个因数是 2 & 5，因此返回 3 & 3 。

示例 2：

输入：num = 123
输出：[5,25]

示例 3：

输入：num = 999
输出：[40,25]

__提示：__

1 <= num <= 10^9

__思路：__

数学
两个最近的因数在 n ^ 1 / 2 两侧
从 n ^ 1 / 2 开始向下搜索直到找到 n 的因子
时间复杂度为 O(n ^ 1 / 2), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<int> closestDivisors(int num)
    {
        vector<int> result1 = helper(num + 1), result2 = helper(num + 2);
        return abs(result1.front() - result1.back()) < abs(result2.front() - result2.back()) ? result1 : result2;
    }
private:
    vector<int> helper(int num) 
    {
        vector<int> result(2);
        for (int i = (int)sqrt(num); i > 0; i--) 
        {
            if (!(num % i)) 
            {
                result.front() = i;
                result.back() = num / i;
                break;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] closestDivisors(int num) {
        int[] result1 = helper(num + 1), result2 = helper(num + 2);
        return Math.abs(result1[0] - result1[1]) < Math.abs(result2[0] - result2[1]) ? result1 : result2;
    }
    
    private int[] helper(int num) {
        int result[] = new int[2];
        for (int i = (int)Math.sqrt(num); i > 0; i--) {
            if (num % i == 0) {
                result[0] = i;
                result[1] = num / i;
                break;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def closestDivisors(self, num: int) -> List[int]:
        def helper(num: int) -> List[int]:
            for x in range(int(num ** 0.5), 0, -1):
                if not num % x:
                    return [x, num // x]
        return result1 if abs((result1 := helper(num + 1))[0] - result1[1]) < abs((result2 := helper(num + 2))[0] - result2[1]) else result2
```
