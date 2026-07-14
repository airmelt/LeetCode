# 3100 Water Bottles II 换水问题 II

__Description:__

You are given two integers `numBottles` and `numExchange`.

`numBottles` represents the number of full water bottles that you initially have. In one operation, you can perform one of the following operations:

- Drink any number of full water bottles turning them into empty bottles.
- Exchange `numExchange` empty bottles with one full water bottle. Then, increase `numExchange` by one.

Note that you cannot exchange multiple batches of empty bottles for the same value of `numExchange`. For example, if `numBottles == 3` and `numExchange == 1`, you cannot exchange `3` empty water bottles for `3` full bottles.

Return the maximum number of water bottles you can drink.

__Example:__

Example 1:

![3100-1](https://assets.leetcode.com/uploads/2024/01/28/exampleone1.png)

```text
Input: numBottles = 13, numExchange = 6
Output: 15
Explanation: The table above shows the number of full water bottles, empty water bottles, the value of numExchange, and the number of bottles drunk.
```

Example 2:

![3100-2](https://assets.leetcode.com/uploads/2024/01/28/example231.png)

```text
Input: numBottles = 10, numExchange = 3
Output: 13
Explanation: The table above shows the number of full water bottles, empty water bottles, the value of numExchange, and the number of bottles drunk.
```

__Constraints:__

- `1 <= numBottles <= 100`
- `1 <= numExchange <= 100`

__题目描述:__

给你两个整数 `numBottles` 和 `numExchange` 。

`numBottles` 代表你最初拥有的满水瓶数量。在一次操作中，你可以执行以下操作之一:

- 喝掉任意数量的满水瓶，使它们变成空水瓶。
- 用 `numExchange` 个空水瓶交换一个满水瓶。然后，将 `numExchange` 的值增加 1 。

注意，你不能使用相同的 `numExchange` 值交换多批空水瓶。例如，如果 `numBottles == 3` 并且 `numExchange == 1` ，则不能用 `3` 个空水瓶交换成 `3` 个满水瓶。

返回你 最多 可以喝到多少瓶水。

__示例:__

示例 1：

![3100-3](https://assets.leetcode.com/uploads/2024/01/28/exampleone1.png)

```text
输入：numBottles = 13, numExchange = 6
输出：15
解释：上表显示了满水瓶的数量、空水瓶的数量、numExchange 的值，以及累计喝掉的水瓶数量。
```

示例 2：

![3100-4](https://assets.leetcode.com/uploads/2024/01/28/example231.png)

```text
输入：numBottles = 10, numExchange = 3
输出：13
解释：上表显示了满水瓶的数量、空水瓶的数量、numExchange 的值，以及累计喝掉的水瓶数量。
```

__提示：__

- `1 <= numBottles <= 100`
- `1 <= numExchange <= 100`

__思路:__

```text
1. 模拟
按照题意模拟
一次次兑换空瓶直到空瓶数量不够
时间复杂度为 O(N * 1 / 2), 空间复杂度为 O(1), N 为 numBottles 的数量
2. 数学
需要交换 k 次
那么 numBottles - (numExchange + (numExchange + 1) + ... + (numExchange + k)) < numExchange + k
解这个一元二次方程得到 k
返回 numBottles + k 即可
时间复杂度为 O(1), 空间复杂度为 O(1), N 为 numBottles 的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxBottlesDrunk(int numBottles, int numExchange) 
    {
        return numBottles + ((int)(sqrt(((numExchange << 1) - 1) * ((numExchange << 1) - 1) + ((numBottles - numExchange) << 3)) - ((numExchange << 1) - 1) + 2) >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int maxBottlesDrunk(int numBottles, int numExchange) {
        return numBottles + ((int)(Math.sqrt(((numExchange << 1) - 1) * ((numExchange << 1) - 1) + ((numBottles - numExchange) << 3)) - ((numExchange << 1) - 1) + 2) >> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def maxBottlesDrunk(self, numBottles: int, numExchange: int) -> int:
        return numBottles + ((isqrt((a := (numExchange << 1) - 1) * a + ((numBottles - numExchange) << 3)) - a + 2) >> 1)
```
