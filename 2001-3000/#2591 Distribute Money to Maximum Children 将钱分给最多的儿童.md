# 2591 Distribute Money to Maximum Children 将钱分给最多的儿童

__Description:__

You are given an integer `money` denoting the amount of money (in dollars) that you have and another integer `children` denoting the number of children that you must distribute the money to.

You have to distribute the money according to the following rules:

- All money must be distributed.
- Everyone must receive at least `1` dollar.
- Nobody receives `4` dollars.

Return _the __maximum__ number of children who may receive __exactly___ `8` _dollars if you distribute the money according to the aforementioned rules_. If there is no way to distribute the money, return `-1`.

__Example:__

Example 1:

```text
Input: money = 20, children = 3
Output: 1
Explanation: 
The maximum number of children with 8 dollars will be 1. One of the ways to distribute the money is:
```

- 8 dollars to the first child.
- 9 dollars to the second child.
- 3 dollars to the third child.

It can be proven that no distribution exists such that number of children getting 8 dollars is greater than 1.

Example 2:

```text
Input: money = 16, children = 2
Output: 2
Explanation: Each child can be given 8 dollars.
```

__Constraints:__

- `1 <= money <= 200`
- `2 <= children <= 30`

__题目描述:__

给你一个整数 `money` ，表示你总共有的钱数（单位为美元）和另一个整数 `children` ，表示你要将钱分配给多少个儿童。

你需要按照如下规则分配：

- 所有的钱都必须被分配。
- 每个儿童至少获得 `1` 美元。
- 没有人获得 `4` 美元。

请你按照上述规则分配金钱，并返回 __最多__ 有多少个儿童获得 __恰好__ `8` 美元。如果没有任何分配方案，返回 `-1` 。

__示例:__

示例 1：

```text
输入：money = 20, children = 3
输出：1
解释：
最多获得 8 美元的儿童数为 1 。一种分配方案为：
```

- 给第一个儿童分配 8 美元。
- 给第二个儿童分配 9 美元。
- 给第三个儿童分配 3 美元。

没有分配方案能让获得 8 美元的儿童数超过 1 。

示例 2：

```text
输入：money = 16, children = 2
输出：2
解释：每个儿童都可以获得 8 美元。
```

__提示：__

- `1 <= money <= 200`
- `2 <= children <= 30`

__思路:__

```text
模拟
按照规则每个人都必须分到钱, 所以 children > money 时直接返回 -1
如果每个人刚好分到 8 美元, 则直接返回 children
如果其他人都分到 8 美元, 但最后一个人分到 4 美元, 则返回 children - 2
否则, 如果钱足够多, 那么可以给除了最后一个人每人 8 美元
或者钱不够多的情况下, 先给每个人 1 美元, 剩下的钱分给其他人, 最多可以分到 (money - children) / 7 个人
返回 min(children - 1, (money - children) / 7)
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int distMoney(int money, int children) 
    {
        return children > money ? -1 : (money == (children << 3) ? children : (money == (children << 3) - 4 ? children - 2 : min(children - 1, (money - children) / 7)));
    }
};
```

__Java__:

```Java
class Solution {
    public int distMoney(int money, int children) {
        return children > money ? -1 : (money == (children << 3) ? children : (money == (children << 3) - 4 ? children - 2 : Math.min(children - 1, (money - children) / 7)));
    }
}
```

__Python__:

```Python
class Solution:
    def distMoney(self, money: int, children: int) -> int:
        return -1 if children > money else children if money == (children << 3) else children - 2 if money == (children << 3) - 4 else min((money - children) // 7, children - 1)
```
