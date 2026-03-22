# 2165 Smallest Value of the Rearranged Number 重排数字的最小值

__Description:__

You are given an integer `num.` __Rearrange__ the digits of `num` such that its value is __minimized__ and it does not contain __any__ leading zeros.

Return the rearranged number with minimal value.

Note that the sign of the number does not change after rearranging the digits.

__Example:__

Example 1:

```text
Input: num = 310
Output: 103
Explanation: The possible arrangements for the digits of 310 are 013, 031, 103, 130, 301, 310. 
The arrangement with the smallest value that does not contain any leading zeros is 103.
```

Example 2:

```text
Input: num = -7605
Output: -7650
Explanation: Some possible arrangements for the digits of -7605 are -7650, -6705, -5076, -0567.
The arrangement with the smallest value that does not contain any leading zeros is -7650.
```

__Constraints:__

- `-10 ^ 15 <= num <= 10 ^ 15`

__题目描述:__

给你一个整数 `num` 。__重排__ `num` 中的各位数字，使其值 __最小化__ 且不含 __任何__ 前导零。

返回不含前导零且值最小的重排数字。

注意，重排各位数字后， `num` 的符号不会改变。

__示例:__

示例 1：

```text
输入：num = 310
输出：103
解释：310 中各位数字的可行排列有：013、031、103、130、301、310 。
不含任何前导零且值最小的重排数字是 103 。
```

示例 2：

```text
输入：num = -7605
输出：-7650
解释：-7605 中各位数字的部分可行排列为：-7650、-6705、-5076、-0567。
不含任何前导零且值最小的重排数字是 -7650 。
```

__提示：__

- `-10 ^ 15 <= num <= 10 ^ 15`

__思路:__

```text
排序
按照正数负数讨论
如果是正数就正序排列，如果是负数就逆序排列
如果不是负数且第一个数字是 0，就找到第一个不是 0 的数字，和第一个数字交换
时间复杂度为 O(lgNlglgN), 空间复杂度为 O(lgN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long smallestNumber(long long num) 
    {
        auto str = to_string(num);
        if (str.front() == '-' or str.front() == '0') sort(str.begin() + 1, str.end(), greater());
        else
        {
            sort(str.begin(), str.end());
            swap(*find_if(str.begin(), str.end(), [](char c){return c != '0';}), *str.begin());
        }
        return stol(str);
    }
};
```

__Java__:

```Java
class Solution {
    public long smallestNumber(long num) {
        boolean flag = num >= 0;
        int[] nums = new int[10];
        while (num != 0) {
            nums[(int)Math.abs(num % 10)]++;
            num /= 10;
        }
        long result = 0;
        if (flag) {
            for (int i = 1; i <= 9; i++) result = process(nums, i, result, flag);
            while (nums[0]-- > 0) result *= 10;
        } else {
            for (int i = 9; i >= 0; i--) result = process(nums, i, result, flag);
        }
        return flag ? result : -result;
    }


    private long process(int[] nums, int index, long num, boolean flag) {
        for (int i = 0; i < nums[index]; i++) {
            while (flag && num != 0 && nums[0]-- > 0) num *= 10;
            num = num * 10 + index;
        }
        return num;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestNumber(self, num: int) -> int:
        if not num:
            return 0
        negative, num, digits = num < 0, abs(num), sorted(int(digit) for digit in str(abs(num)))
        if negative:
            digits = digits[::-1]
        else:
            if digits[0] == 0:
                i = 1
                while not digits[i]:
                    i += 1
                digits[0], digits[i] = digits[i], digits[0]
        return -int("".join(str(digit) for digit in digits)) if negative else int("".join(str(digit) for digit in digits))
```
