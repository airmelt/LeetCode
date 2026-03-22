# 672 Bulb Switcher II 灯泡开关 Ⅱ

__Description__:
There is a room with n bulbs labeled from 1 to n that all are turned on initially, and four buttons on the wall. Each of the four buttons has a different functionality where:

Button 1: Flips the status of all the bulbs.
Button 2: Flips the status of all the bulbs with even labels (i.e., 2, 4, ...).
Button 3: Flips the status of all the bulbs with odd labels (i.e., 1, 3, ...).
Button 4: Flips the status of all the bulbs with a label j = 3k + 1 where k = 0, 1, 2, ... (i.e., 1, 4, 7, 10, ...).
You will press one of the four mentioned buttons exactly presses times.

Given the two integers n and presses, return the number of different statuses after pressing the four buttons exactly presses times.

__Example:__

Example 1:

Input: n = 1, presses = 1
Output: 2
Explanation: Status can be: [on], [off].

Example 2:

Input: n = 2, presses = 1
Output: 3
Explanation: Status can be: [on, off], [off, on], [off, off].

Example 3:

Input: n = 3, presses = 1
Output: 4
Explanation: Status can be: [off, on, off], [on, off, on], [off, off, off], [off, on, on].

__Constraints:__

1 <= n <= 1000
0 <= presses <= 1000

__题目描述__:
现有一个房间，墙上挂有 n 只已经打开的灯泡和 4 个按钮。在进行了 m 次未知操作后，你需要返回这 n 只灯泡可能有多少种不同的状态。

假设这 n 只灯泡被编号为 [1, 2, 3 ..., n]，这 4 个按钮的功能如下：

将所有灯泡的状态反转（即开变为关，关变为开）
将编号为偶数的灯泡的状态反转
将编号为奇数的灯泡的状态反转
将编号为 3k+1 的灯泡的状态反转（k = 0, 1, 2, ...)

__示例 :__

示例 1:

输入: n = 1, m = 1.
输出: 2
说明: 状态为: [开], [关]

示例 2:

输入: n = 2, m = 1.
输出: 3
说明: 状态为: [开, 关], [关, 开], [关, 关]

示例 3:

输入: n = 3, m = 1.
输出: 4
说明: 状态为: [关, 开, 关], [开, 关, 开], [关, 关, 关], [关, 开, 开].

__注意:__
n 和 m 都属于 [0, 1000].

__思路__:

穷举
虽然 presses, n 看起来区间有 [0, 1000] 那么大, 实际上对每个操作来说 2 次操作等于没操作, 所以操作只需要 1 次或者 0 次, 并且操作顺序对灯泡是否开关没影响
这样 ∑presses 可以在 0-15(0b0000-0b1111) 内取值来观察灯泡的开关情况
另外, 由于操作 1234 的操作灯泡下标的最小公倍数为 6, 所以操作 x 号灯泡必然会和操作 x + 6 号灯泡等效
所以区间可以缩小为 0 < n < 7, -1 < ∑presses < 16, 在这个区间内搜索即可
时间复杂度 O(1), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int flipLights(int n, int presses) 
    {
        return vector<vector<int>> { {1, 1, 1}, {2, 3, 4}, {2, 4, 7}, {2, 4, 8}}[min(presses, 3)][min(n - 1, 2)];
    }
};
```

__Java__:

```Java
class Solution {
    public int flipLights(int n, int presses) {
        return n == 0 ? 0 : presses == 0 ? 1 : n == 1 ? 2 : n == 2 && presses == 1 ? 3 : n == 2 && presses != 1 || presses == 1 ? 4 : presses == 2 ? 7 : 8;
    }
}
```

__Python__:

```Python
class Solution:
    def flipLights(self, n: int, presses: int) -> int:
        return [[1, 1, 1], [2, 3, 4], [2, 4, 7], [2, 4, 8]][min(presses, 3)][min(n - 1, 2)]
```
