# 2443 Sum of Number and Its Reverse 反转之后的数字和

__Description:__

Given a __non-negative__ integer `num`, return `true` _if_ `num` _can be expressed as the sum of any __non-negative__ integer and its reverse, or_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: num = 443
Output: true
Explanation: 172 + 271 = 443 so we return true.
```

Example 2:

```text
Input: num = 63
Output: false
Explanation: 63 cannot be expressed as the sum of a non-negative integer and its reverse so we return false.
```

Example 3:

```text
Input: num = 181
Output: true
Explanation: 140 + 041 = 181 so we return true. Note that when a number is reversed, there may be leading zeros.
```

__Constraints:__

- `0 <= num <= 10 ^ 5`

__题目描述:__

给你一个 __非负__ 整数 `num` 。如果存在某个 __非负__ 整数 `k` 满足 `k + reverse(k) = num` ，则返回 `true` ；否则，返回 `false` 。

`reverse(k)` 表示 `k` 反转每个数位后得到的数字。

__示例:__

示例 1：

```text
输入：num = 443
输出：true
解释：172 + 271 = 443 ，所以返回 true 。
```

示例 2：

```text
输入：num = 63
输出：false
解释：63 不能表示为非负整数及其反转后数字之和，返回 false 。
```

示例 3：

```text
输入：num = 181
输出：true
解释：140 + 041 = 181 ，所以返回 true 。注意，反转后的数字可能包含前导零。
```

__提示：__

- `0 <= num <= 10 ^ 5`

__思路:__

```text
1. 暴力法
遍历所有不大于 num 的数
检查其反转后的数的和是否等于 num
时间复杂度为 O(NlogN), 空间复杂度为 O(1), 每次得到反转的数需要 O(logN) 的时间复杂度
2. 分类讨论
如果 ace...fdb 这样的一个数
第一步必须是 a - b <= 1, a - b == 1 时是因为有进位
比如 172 + 271 = 443
如果 a == b, 那么 c == d
否则 a == b + 1, 那么 d 也需要借位, 如果此时导致 j < 0, 还需要进一步借位
下一步判断 e, f, 如果 f == 9, 有可能是借位导致的, 那么如果 e == 9, c == 1 则不可能因为两个数字相加最大为 18
如果 e == 0, 看作 10 继续判断
指针如果相遇, 判断 num[i] == num[j], 如果 i == j, 判断奇偶性是否相同
对于 1 开头的数字需要额外判断是否有前导 0
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool sumOfNumberAndReverse(int num) 
    {
        string s = to_string(num);
        function<bool(int, int, int, int)> dfs = [&](int i, int j, int pre, int suf) 
        {
            if (i < j) 
            {
                for (int m = 0, sum = pre * 10 + (s[i] - '0') - m; m <= 1; m++, sum--) if (sum >= int(i == 0) and sum <= 18 and (sum + suf) % 10 == s[j] - '0') return dfs(i + 1, j - 1, m, (sum + suf) / 10);
                return false;
            }
            return (i == j and (s[i] & 1) == suf) or (i > j and pre == suf);
        };
        return dfs(0, s.size() - 1, 0, 0) or (s[0] == '1' and dfs(1, s.size() - 1, 1, 0));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean sumOfNumberAndReverse(int num) {
        for (int i = 0; i <= num; i++) {
            int rev = 0, x = i;
            while (x > 0) {
                rev = rev * 10 + x % 10;
                x /= 10;
            }
            if (i + rev == num) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfNumberAndReverse(self, num: int) -> bool:
        return any(i + int(str(i)[::-1]) == num for i in range(num + 1))
```
