# 2169 Count Operations to Obtain Zero 得到 0 的操作数

__Description:__

You are given two __non-negative__ integers `num1` and `num2`.

In one __operation__, if `num1 >= num2`, you must subtract `num2` from `num1`, otherwise subtract `num1` from `num2`.

- For example, if `num1 = 5` and `num2 = 4`, subtract `num2` from `num1`, thus obtaining `num1 = 1` and `num2 = 4`. However, if `num1 = 4` and `num2 = 5`, after one operation, `num1 = 4` and `num2 = 1`.

Return _the __number of operations__ required to make either_ `num1 = 0` _or_ `num2 = 0`.

__Example:__

Example 1:

```text
Input: num1 = 2, num2 = 3
Output: 3
Explanation: 
```

- Operation 1: num1 = 2, num2 = 3. Since num1 < num2, we subtract num1 from num2 and get num1 = 2, num2 = 3 - 2 = 1.
- Operation 2: num1 = 2, num2 = 1. Since num1 > num2, we subtract num2 from num1.
- Operation 3: num1 = 1, num2 = 1. Since num1 == num2, we subtract num2 from num1.

Now num1 = 0 and num2 = 1. Since num1 == 0, we do not need to perform any further operations.
So the total number of operations required is 3.

Example 2:

```text
Input: num1 = 10, num2 = 10
Output: 1
Explanation: 
```

- Operation 1: num1 = 10, num2 = 10. Since num1 == num2, we subtract num2 from num1 and get num1 = 10 - 10 = 0.

Now num1 = 0 and num2 = 10. Since num1 == 0, we are done.
So the total number of operations required is 1.

__Constraints:__

- `0 <= num1, num2 <= 10 ^ 5`

__题目描述:__

给你两个 __非负__ 整数 `num1` 和 `num2` 。

每一步 __操作__ 中，如果 `num1 >= num2` ，你必须用 `num1` 减 `num2` ；否则，你必须用 `num2` 减 `num1` 。

- 例如， `num1 = 5` 且 `num2 = 4` ，应该用 `num1` 减 `num2` ，因此，得到 `num1 = 1` 和 `num2 = 4` 。然而，如果 `num1 = 4`且 `num2 = 5` ，一步操作后，得到 `num1 = 4` 和 `num2 = 1` 。

返回使 `num1 = 0` 或 `num2 = 0` 的 __操作数__ 。

__示例:__

示例 1：

```text
输入：num1 = 2, num2 = 3
输出：3
解释：
```

- 操作 1 ：num1 = 2 ，num2 = 3 。由于 num1 < num2 ，num2 减 num1 得到 num1 = 2 ，num2 = 3 - 2 = 1 。
- 操作 2 ：num1 = 2 ，num2 = 1 。由于 num1 > num2 ，num1 减 num2 。
- 操作 3 ：num1 = 1 ，num2 = 1 。由于 num1 == num2 ，num1 减 num2 。

此时 num1 = 0 ，num2 = 1 。由于 num1 == 0 ，不需要再执行任何操作。
所以总操作数是 3 。

示例 2：

```text
输入：num1 = 10, num2 = 10
输出：1
解释：
```

- 操作 1 ：num1 = 10 ，num2 = 10 。由于 num1 == num2 ，num1 减 num2 得到 num1 = 10 - 10 = 0 。

此时 num1 = 0 ，num2 = 10 。由于 num1 == 0 ，不需要再执行任何操作。
所以总操作数是 1 。

__提示：__

- `0 <= num1, num2 <= 10 ^ 5`

__思路:__

```text
数学
辗转相除法
每次累加 num2 / num1 的商
然后令 num1 = num2 % num1, num2 = num1
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countOperations(int num1, int num2) 
    {
        int result = 0, t = 0;
        while (num1) 
        {
            result += num2 / num1;
            t = num1;
            num1 = num2 % num1;
            num2 = t;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countOperations(int num1, int num2) {
        int result = 0, t = 0;
        while (num1 > 0) {
            result += num2 / num1;
            t = num1;
            num1 = num2 % num1;
            num2 = t;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countOperations(self, num1: int, num2: int) -> int:
        return 0 if not num1 or not num2 else self.countOperations(abs(num1 - num2), min(num1, num2)) + 1
```
