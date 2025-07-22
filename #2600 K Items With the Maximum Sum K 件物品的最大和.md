# 2600 K Items With the Maximum Sum K 件物品的最大和

__Description:__

There is a bag that consists of items, each item has a number `1`, `0`, or `-1` written on it.

You are given four __non-negative__ integers `numOnes`, `numZeros`, `numNegOnes`, and `k`.

The bag initially contains:

- `numOnes` items with `1`s written on them.
- `numZeroes` items with `0`s written on them.
- `numNegOnes` items with `-1`s written on them.

We want to pick exactly `k` items among the available items. Return _the __maximum__ possible sum of numbers written on the items_.

__Example:__

Example 1:

```text
Input: numOnes = 3, numZeros = 2, numNegOnes = 0, k = 2
Output: 2
Explanation: We have a bag of items with numbers written on them {1, 1, 1, 0, 0}. We take 2 items with 1 written on them and get a sum in a total of 2.
It can be proven that 2 is the maximum possible sum.
```

Example 2:

```text
Input: numOnes = 3, numZeros = 2, numNegOnes = 0, k = 4
Output: 3
Explanation: We have a bag of items with numbers written on them {1, 1, 1, 0, 0}. We take 3 items with 1 written on them, and 1 item with 0 written on it, and get a sum in a total of 3.
It can be proven that 3 is the maximum possible sum.
```

__Constraints:__

- `0 <= numOnes, numZeros, numNegOnes <= 50`
- `0 <= k <= numOnes + numZeros + numNegOnes`

__题目描述:__

袋子中装有一些物品，每个物品上都标记着数字 `1` 、 `0` 或 `-1` 。

给你四个非负整数 `numOnes` 、 `numZeros` 、 `numNegOnes` 和 `k` 。

袋子最初包含：

- `numOnes` 件标记为 `1` 的物品。
- `numZeros` 件标记为 `0` 的物品。
- `numNegOnes` 件标记为 `-1` 的物品。

现计划从这些物品中恰好选出 `k` 件物品。返回所有可行方案中，物品上所标记数字之和的最大值。

__示例:__

示例 1：

```text
输入：numOnes = 3, numZeros = 2, numNegOnes = 0, k = 2
输出：2
解释：袋子中的物品分别标记为 {1, 1, 1, 0, 0} 。取 2 件标记为 1 的物品，得到的数字之和为 2 。
可以证明 2 是所有可行方案中的最大值。
```

示例 2：

```text
输入：numOnes = 3, numZeros = 2, numNegOnes = 0, k = 4
输出：3
解释：袋子中的物品分别标记为 {1, 1, 1, 0, 0} 。取 3 件标记为 1 的物品，1 件标记为 0 的物品，得到的数字之和为 3 。
可以证明 3 是所有可行方案中的最大值。
```

__提示：__

- `0 <= numOnes, numZeros, numNegOnes <= 50`
- `0 <= k <= numOnes + numZeros + numNegOnes`

__思路:__

```text
模拟
如果 numOnes + numZeros >= k, 则可以取 k 件物品, 最大和为 min(k, numOnes), 即取完 1 再取 0
否则, 取所有的 1 和 0, 再取 -(k - numOnes - numZeros - numOnes)
即取完 1 和 0 后, 还需要取 k - numOnes - numZeros 件物品, 这些物品都是 -1, 所以和为 -(k - numOnes - numZeros - numOnes)
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int kItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) 
    {
        return numOnes + numZeros >= k ? min(k, numOnes) : -(k - numOnes - numZeros - numOnes);
    }
};
```

__Java__:

```Java
class Solution {
    public int kItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) {
        return numOnes + numZeros >= k ? Math.min(k, numOnes) : -(k - numOnes - numZeros - numOnes);
    }
}
```

__Python__:

```Python
class Solution:
    def kItemsWithMaximumSum(self, numOnes: int, numZeros: int, numNegOnes: int, k: int) -> int:
        return min(k, numOnes) if numOnes + numZeros >= k else -(k - numOnes - numZeros - numOnes) 
```
