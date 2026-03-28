# 2952 Minimum Number of Coins to be Added 需要添加的硬币的最小数量

__Description:__

You are given a __0-indexed__ integer array `coins`, representing the values of the coins available, and an integer `target`.

An integer `x` is __obtainable__ if there exists a subsequence of `coins` that sums to `x`.

Return _the __minimum__ number of coins __of any value__ that need to be added to the array so that every integer in the range_ `[1, target]` _is __obtainable___.

A subsequence of an array is a new non-empty array that is formed from the original array by deleting some (possibly none) of the elements without disturbing the relative positions of the remaining elements.

__Example:__

Example 1:

```text
Input: coins = [1,4,10], target = 19
Output: 2
Explanation: We need to add coins 2 and 8. The resulting array will be [1,2,4,8,10].
It can be shown that all integers from 1 to 19 are obtainable from the resulting array, and that 2 is the minimum number of coins that need to be added to the array.
```

Example 2:

```text
Input: coins = [1,4,10,5,7,19], target = 19
Output: 1
Explanation: We only need to add the coin 2. The resulting array will be [1,2,4,5,7,10,19].
It can be shown that all integers from 1 to 19 are obtainable from the resulting array, and that 1 is the minimum number of coins that need to be added to the array.
```

Example 3:

```text
Input: coins = [1,1,1], target = 20
Output: 3
Explanation: We need to add coins 4, 8, and 16. The resulting array will be [1,1,1,4,8,16].
It can be shown that all integers from 1 to 20 are obtainable from the resulting array, and that 3 is the minimum number of coins that need to be added to the array.
```

__Constraints:__

- `1 <= target <= 10 ^ 5`
- `1 <= coins.length <= 10 ^ 5`
- `1 <= coins[i] <= target`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `coins`，表示可用的硬币的面值，以及一个整数 `target` 。

如果存在某个 `coins` 的子序列总和为 `x`，那么整数 `x` 就是一个 __可取得的金额__ 。

返回需要添加到数组中的 __任意面值__ 硬币的 __最小数量__ ，使范围 `[1, target]` 内的每个整数都属于 __可取得的金额__ 。

数组的 子序列 是通过删除原始数组的一些（可能不删除）元素而形成的新的 非空 数组，删除过程不会改变剩余元素的相对位置。

__示例:__

示例 1：

```text
输入：coins = [1,4,10], target = 19
输出：2
解释：需要添加面值为 2 和 8 的硬币各一枚，得到硬币数组 [1,2,4,8,10] 。
可以证明从 1 到 19 的所有整数都可由数组中的硬币组合得到，且需要添加到数组中的硬币数目最小为 2 。
```

示例 2：

```text
输入：coins = [1,4,10,5,7,19], target = 19
输出：1
解释：只需要添加一枚面值为 2 的硬币，得到硬币数组 [1,2,4,5,7,10,19] 。
可以证明从 1 到 19 的所有整数都可由数组中的硬币组合得到，且需要添加到数组中的硬币数目最小为 1 。
```

示例 3：

```text
输入：coins = [1,1,1], target = 20
输出：3
解释：
需要添加面值为 4 、8 和 16 的硬币各一枚，得到硬币数组 [1,1,1,4,8,16] 。 
可以证明从 1 到 20 的所有整数都可由数组中的硬币组合得到，且需要添加到数组中的硬币数目最小为 3 。
```

__提示：__

- `1 <= target <= 10 ^ 5`
- `1 <= coins.length <= 10 ^ 5`
- `1 <= coins[i] <= target`

__思路:__

```text
贪心
先对 coins 进行排序
对于 cur 已经满足
那么就是 [1, cur] 都已经满足
如果下一个 coins[i] <= cur
对于 [1, cur] 每个数都加上 coins[i] 可以扩展为 [1 + coins[i], cur + coins[i]]
也就是说 cur 可以加上当前的 coins[i] 也是都能满足的
假设不能满足
按照贪心原则直接将 cur 加入当前的数组
那么 [1, cur] 能满足
并且由于加入了 cur 所以 [1, cur * 2] 都能满足
所以 cur <<= 1, 并且结果加 1
时间复杂度为 O(NlogN + logM), 空间复杂度为 O(logN), M 为 target 的大小, 主要开销在排序上
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int minimumAddedCoins(vector<int>& coins, int target) 
    {
        sort(coins.begin(), coins.end());
        int result = 0, cur = 1, i = 0, n = coins.size();
        while (cur <= target) 
        {
            if (i < n and coins[i] <= cur) cur += coins[i++];
            else 
            {
                ++result;
                cur <<= 1;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumAddedCoins(int[] coins, int target) {
        Arrays.sort(coins);
        int result = 0, cur = 1, i = 0, n = coins.length;
        while (cur <= target) {
            if (i < n && coins[i] <= cur) cur += coins[i++];
            else {
                ++result;
                cur <<= 1;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumAddedCoins(self, coins: List[int], target: int) -> int:
        coins.sort()
        result, n, cur, i = 0, len(coins), 1, 0
        while cur <= target:
            if i < n and coins[i] <= cur:
                cur += coins[i]
                i += 1
            else:
                cur <<= 1
                result += 1
        return result 
```
