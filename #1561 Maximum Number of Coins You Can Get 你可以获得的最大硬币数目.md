# 1561 Maximum Number of Coins You Can Get 你可以获得的最大硬币数目

__Description:__

There are `3n` piles of coins of varying size, you and your friends will take piles of coins as follows:

- In each step, you will choose __any__ `3` piles of coins (not necessarily consecutive).
- Of your choice, Alice will pick the pile with the maximum number of coins.
- You will pick the next pile with the maximum number of coins.
- Your friend Bob will pick the last pile.
- Repeat until there are no more piles of coins.

Given an array of integers `piles` where `piles[i]` is the number of coins in the `i ^ th` pile.

Return the maximum number of coins that you can have.

__Example:__

Example 1:

```text
Input: piles = [2,4,1,2,7,8]
Output: 9
Explanation: Choose the triplet (2, 7, 8), Alice Pick the pile with 8 coins, you the pile with 7 coins and Bob the last one.
Choose the triplet (1, 2, 4), Alice Pick the pile with 4 coins, you the pile with 2 coins and Bob the last one.
The maximum number of coins which you can have are: 7 + 2 = 9.
On the other hand if we choose this arrangement (1, 2, 8), (2, 4, 7) you only get 2 + 4 = 6 coins which is not optimal.
```

Example 2:

```text
Input: piles = [2,4,5]
Output: 4
```

Example 3:

```text
Input: piles = [9,8,7,6,5,1,2,3,4]
Output: 18
```

__Constraints:__

- `3 <= piles.length <= 10 ^ 5`
- `piles.length % 3 == 0`
- `1 <= piles[i] <= 10 ^ 4`

__题目描述:__

There are `3n` piles of coins of varying size, you and your friends will take piles of coins as follows:

- In each step, you will choose __any__ `3` piles of coins (not necessarily consecutive).
- Of your choice, Alice will pick the pile with the maximum number of coins.
- You will pick the next pile with the maximum number of coins.
- Your friend Bob will pick the last pile.
- Repeat until there are no more piles of coins.

Given an array of integers `piles` where `piles[i]` is the number of coins in the `i ^ th` pile.

Return the maximum number of coins that you can have.

__Example:__

Example 1:

```text
Input: piles = [2,4,1,2,7,8]
Output: 9
Explanation: Choose the triplet (2, 7, 8), Alice Pick the pile with 8 coins, you the pile with 7 coins and Bob the last one.
Choose the triplet (1, 2, 4), Alice Pick the pile with 4 coins, you the pile with 2 coins and Bob the last one.
The maximum number of coins which you can have are: 7 + 2 = 9.
On the other hand if we choose this arrangement (1, 2, 8), (2, 4, 7) you only get 2 + 4 = 6 coins which is not optimal.
```

Example 2:

```text
Input: piles = [2,4,5]
Output: 4
```

Example 3:

```text
Input: piles = [9,8,7,6,5,1,2,3,4]
Output: 18
```

__Constraints:__

- `3 <= piles.length <= 10 ^ 5`
- `piles.length % 3 == 0`
- `1 <= piles[i] <= 10 ^ 4`

__思路:__

```text
1. 排序
因为 Bob 每次都取最小的, 所以 Bob 必然取得最小的 1 / 3 部分
每次取最大的两个数
自己拿较小的那个
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 统计
将所有石头的大小进行统计
并分配给三人
时间复杂度为 O(N + S), 空间复杂度为 O(S), S 为石头数组的范围
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxCoins(vector<int>& piles) 
    {
        int n = piles.size(), num = n / 3, result = 0, cur = 0;
        vector<int> count(10005);
        for (int i = 0; i < n; i++) ++count[piles[i]];
        for (int i = 10000; i >= 0; i--)
        {
            if (count[i] == 0) continue;
            if (!cur)
            {
                int p = count[i] >> 1;
                if (num >= p)
                {
                    result += p * i;
                    num -= p;
                    cur = count[i] & 1;
                }
                else
                {
                    result += num * i;
                    break;
                }
            }
            else
            {
                int p = (count[i] + 1) >> 1;
                if (num >= p)
                {
                    result += p * i;
                    num -= p;
                    cur = (count[i] + 1) & 1;
                }
                else
                {
                    result += num * i;
                    break;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxCoins(int[] piles) {
        Arrays.sort(piles);
        int result = 0, n = piles.length;
        for (int i = n / 3; i < n; i += 2) result += piles[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxCoins(self, piles: List[int]) -> int:
        piles.sort()
        result, n = 0, len(piles)
        for i in range(n // 3, n, 2):
            result += piles[i]
        return result
```
