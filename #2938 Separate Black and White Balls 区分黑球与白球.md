# 2938 Separate Black and White Balls 区分黑球与白球

__Description:__

There are `n` balls on a table, each ball has a color black or white.

You are given a __0-indexed__ binary string `s` of length `n`, where `1` and `0` represent black and white balls, respectively.

In each step, you can choose two adjacent balls and swap them.

Return the minimum number of steps to group all the black balls to the right and all the white balls to the left.

__Example:__

Example 1:

```text
Input: s = "101"
Output: 1
Explanation: We can group all the black balls to the right in the following way:
```

- Swap s[0] and s[1], s = "011".

Initially, 1s are not grouped together, requiring at least 1 step to group them to the right.

Example 2:

```text
Input: s = "100"
Output: 2
Explanation: We can group all the black balls to the right in the following way:
```

- Swap s[0] and s[1], s = "010".
- Swap s[1] and s[2], s = "001".

It can be proven that the minimum number of steps needed is 2.

Example 3:

```text
Input: s = "0111"
Output: 0
Explanation: All the black balls are already grouped to the right.
```

__Constraints:__

- `1 <= n == s.length <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

桌子上有 `n` 个球，每个球的颜色不是黑色，就是白色。

给你一个长度为 `n` 、下标从 __0__ 开始的二进制字符串 `s`，其中 `1` 和 `0` 分别代表黑色和白色的球。

在每一步中，你可以选择两个相邻的球并交换它们。

返回「将所有黑色球都移到右侧，所有白色球都移到左侧所需的 最小步数」。

__示例:__

示例 1：

```text
输入：s = "101"
输出：1
解释：我们可以按以下方式将所有黑色球移到右侧：
```

- 交换 s[0] 和 s[1]，s = "011"。

最开始，1 没有都在右侧，需要至少 1 步将其移到右侧。

示例 2：

```text
输入：s = "100"
输出：2
解释：我们可以按以下方式将所有黑色球移到右侧：
```

- 交换 s[0] 和 s[1]，s = "010"。
- 交换 s[1] 和 s[2]，s = "001"。

可以证明所需的最小步数为 2 。

示例 3：

```text
输入：s = "0111"
输出：0
解释：所有黑色球都已经在右侧。
```

__提示：__

- `1 <= n == s.length <= 10 ^ 5`
- `s[i]` 不是 `'0'`，就是 `'1'`。

__思路:__

```text
1. 模拟
需要将每一个 0 左边的 1 都移动到这个 0 的右边去
用一个变量记录 1 出现的次数
如果碰到 0, 前面所有 1 都需要移动
结果累加上 1 出现的次数
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 数学
对于所有的 0 最后都要回到最左边
比如对于 "101011"
最后会变成 "001111"
第一个 0 移动的距离为 1 - 0
第二个 0 移动的距离为 3 - 1 (当前一共有几个零)
所以第一部分为所有 0 出现的位置之和
第二部分为 0 + 1 + 2 + ... + s.count('0')
为等差数列求和
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumSteps(string s) 
    {
        long long result = 0LL, one = 0LL;
        for (const auto& c : s) 
        {
            if (c == '1') ++one;
            else result += one;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumSteps(String s) {
        long result = 0L, one = 0L;
        for (var c : s.toCharArray()) {
            if (c == '1') ++one;
            else result += one;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSteps(self, s: str) -> int:
        return sum(i for i, c in enumerate(s) if c == '0') - ((zero := s.count('0')) * (zero - 1) >> 1)
```
