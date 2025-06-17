# 2566 Maximum Difference by Remapping a Digit 替换一个数字后的最大差值

__Description:__

You are given an integer `num`. You know that Bob will sneakily __remap__ one of the `10` possible digits ( `0` to `9`) to another digit.

Return _the difference between the maximum and minimum values Bob can make by remapping __exactly__ __one__ digit in_ `num`.

Notes:

- When Bob remaps a digit d1 to another digit d2, Bob replaces all occurrences of `d1` in `num` with `d2`.
- Bob can remap a digit to itself, in which case `num` does not change.
- Bob can remap different digits for obtaining minimum and maximum values respectively.
- The resulting number after remapping can contain leading zeroes.

__Example:__

Example 1:

```text
Input: num = 11891
Output: 99009
Explanation: 
To achieve the maximum value, Bob can remap the digit 1 to the digit 9 to yield 99899.
To achieve the minimum value, Bob can remap the digit 1 to the digit 0, yielding 890.
The difference between these two numbers is 99009.
```

Example 2:

```text
Input: num = 90
Output: 99
Explanation:
The maximum value that can be returned by the function is 99 (if 0 is replaced by 9) and the minimum value that can be returned by the function is 0 (if 9 is replaced by 0).
Thus, we return 99.
```

__Constraints:__

- `1 <= num <= 10 ^ 8`

__题目描述:__

给你一个整数 `num` 。你知道 Danny Mittal 会偷偷将 `0` 到 `9` 中的一个数字 __替换__ 成另一个数字。

请你返回将 `num` 中 __恰好一个__ 数字进行替换后，得到的最大值和最小值的差为多少。

注意：

- 当 Danny 将一个数字 `d1` 替换成另一个数字 `d2` 时，Danny 需要将 `nums` 中所有 `d1` 都替换成 `d2` 。
- Danny 可以将一个数字替换成它自己，也就是说 `num` 可以不变。
- Danny 可以将数字分别替换成两个不同的数字分别得到最大值和最小值。
- 替换后得到的数字可以包含前导 0 。
- Danny Mittal 获得周赛 326 前 10 名，让我们恭喜他。

__示例:__

示例 1：

```text
输入：num = 11891
输出：99009
解释：
为了得到最大值，我们将数字 1 替换成数字 9 ，得到 99899 。
为了得到最小值，我们将数字 1 替换成数字 0 ，得到 890 。
两个数字的差值为 99009 。
```

示例 2：

```text
输入：num = 90
输出：99
解释：
可以得到的最大值是 99（将 0 替换成 9），最小值是 0（将 9 替换成 0）。
所以我们得到 99 。
```

__提示：__

- `1 <= num <= 10 ^ 8`

__思路:__

```text
贪心
将第一个字符替换为 0 可以获得最小的值
将第一个不是 9 的字符替换为 9 可以获得最大的值
返回两者的差
时间复杂度为 O(logN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMaxDifference(int num) 
    {
        string s = to_string(num);
        int mx = num;
        for (const auto& c : s) 
        {
            if (c != '9') 
            {
                string tmp = s;
                ranges::replace(tmp, c, '9');
                mx = stoi(tmp);
                break;
            }
        }
        char s0 = s[0];
        ranges::replace(s, s0, '0');
        return mx - stoi(s);
    }
};
```

__Java__:

```Java
class Solution {
    public int minMaxDifference(int num) {
        int maxValue = num;
        for (char c : String.valueOf(num).toCharArray()) {
            if (c != '9') {
                maxValue = Integer.parseInt(String.valueOf(num).replace(c, '9'));
                break;
            }
        }
        return maxValue - Integer.parseInt(String.valueOf(num).replace(String.valueOf(num).charAt(0), '0'));
    }
}
```

__Python__:

```Python
class Solution:
    def minMaxDifference(self, num: int) -> int:
        return max(int(str(num).replace(str(i), '9')) for i in range(9)) - int(str(num).replace(str(num)[0], '0')) 
```
