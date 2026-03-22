# 754 Reach a Number 到达终点数字

__Description__:
You are standing at position 0 on an infinite number line. There is a goal at position target.

On each move, you can either go left or right. During the n-th move (starting from 1), you take n steps.

Return the minimum number of steps required to reach the destination.

__Example:__

Example 1:

Input: target = 3
Output: 2
Explanation:
On the first move we step from 0 to 1.
On the second step we step from 1 to 3.

Example 2:

Input: target = 2
Output: 3
Explanation:
On the first move we step from 0 to 1.
On the second move we step  from 1 to -1.
On the third move we step from -1 to 2.

__Note:__

target will be a non-zero integer in the range [-10^9, 10^9].

__题目描述__:

在一根无限长的数轴上，你站在0的位置。终点在target的位置。

每次你可以选择向左或向右移动。第 n 次移动（从 1 开始），可以走 n 步。

返回到达终点需要的最小移动次数。

__示例 :__

示例 1:

输入: target = 3
输出: 2
解释:
第一次移动，从 0 到 1 。
第二次移动，从 1 到 3 。

示例 2:

输入: target = 2
输出: 3
解释:
第一次移动，从 0 到 1 。
第二次移动，从 1 到 -1 。
第三次移动，从 -1 到 2 。

__注意：__

target是在[-10^9, 10^9]范围中的非零整数。

__思路__:

> 首先 target的正负性不影响结果, 比如 target = 3
3 = 1 + 2; -3 = -1 + (-2)
只要把每一步取相反数即可, 所以直接可以令 target = abs(target)
令 sum = 1 + 2 + 3 + ...
当 sum < target时, 要一直往右走, 直到 sum > target, 不然一定到不了 target
比如 target = 4. sum = 1 + 2 + 3 = 6, 6 - 4 = 2
这个时候如果sum - target是偶数, 那么只要把 sum中的一项替换成负数即可
比如 target = -1 + 2 + 3 = 4
如果这个时候sum - target是奇数, 那么还需要看 sum的最后一项
>> 如果最后一项是奇数, 要多走两步
比如 target = 5, sum = 1 + 2 + 3 = 6, 6 - 5 = 1
target = 1 + 2 + 3 + 4 - 5 = 5
如果最后一项是偶数, 只要多走一步
比如 target = 9, sum = 1 + 2 + 3 + 4 = 10, 10 - 9 = 1
target = 1 + 2 - 3 + 4 + 5 = 9

sum = (1 + 2 + 3 + ... + i) = (1 + i) \* i / 2
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int reachNumber(int target) 
    {
        int s = 0, i = 1;
        for (target = abs(target); s < target or (s - target) % 2; i++) s += i;
        return --i;
    }
};
```

__Java__:

```Java
class Solution {
    public int reachNumber(int target) {
        int s = 0, i = 1;
        target = Math.abs(target);
        while (s < target || (s - target) % 2 != 0) s += i++;
        return --i;
    }
}
```

__Python__:

```Python
class Solution:
    def reachNumber(self, target: int) -> int:
        s, i, target = 0, 1, abs(target)
        while s < target or (s - target) % 2:
            s += i
            i += 1
        return i - 1
```
