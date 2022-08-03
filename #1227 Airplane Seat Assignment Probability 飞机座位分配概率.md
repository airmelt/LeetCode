# 1227 Airplane Seat Assignment Probability 飞机座位分配概率

__Description:__

n passengers board an airplane with exactly n seats. The first passenger has lost the ticket and picks a seat randomly. But after that, the rest of the passengers will:

Take their own seat if it is still available, and
Pick other seats randomly when they find their seat occupied
Return the probability that the nth person gets his own seat.

__Example:__

Example 1:

Input: n = 1
Output: 1.00000
Explanation: The first person can only get the first seat.

Example 2:

Input: n = 2
Output: 0.50000
Explanation: The second person has a probability of 0.5 to get the second seat (when first person gets the first seat).

__Constraints:__

1 <= n <= 10^5

__题目描述：__

有 n 位乘客即将登机，飞机正好有 n 个座位。第一位乘客的票丢了，他随便选了一个座位坐下。

剩下的乘客将会：

如果他们自己的座位还空着，就坐到自己的座位上，

当他们自己的座位被占用时，随机选择其他座位
第 n 位乘客坐在自己的座位上的概率是多少？

__示例：__

示例 1：

输入：n = 1
输出：1.00000
解释：第一个人只会坐在自己的位置上。

示例 2：

输入: n = 2
输出: 0.50000
解释：在第一个人选好座位坐下后，第二个人坐在自己的座位上的概率是 0.5。

__提示：__

1 <= n <= 10^5

__思路：__

数学
当只有 1 个人时只能坐自己的位置, 概率为 1.0
当超过 1 个人时, 第一个人和另外一个人都坐在自己位置上的概率为 0.5, 其实就是要么第一个人坐在自己位置上, 要么他选择了一个别人的位置, 而除了他选择的位置, 其余位置都是按顺序坐的
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    double nthPersonGetsNthSeat(int n) 
    {
        return n == 1 ? 1.0 : 0.5;
    }
};
```

__Java__:

```Java
class Solution {
    public double nthPersonGetsNthSeat(int n) {
        return n == 1 ? 1.0 : 0.5;
    }
}
```

__Python__:

```Python
class Solution:
    def nthPersonGetsNthSeat(self, n: int) -> float:
        return 1.0 if n == 1 else 0.5
```
