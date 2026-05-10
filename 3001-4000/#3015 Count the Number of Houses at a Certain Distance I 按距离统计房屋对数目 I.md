# 3015 Count the Number of Houses at a Certain Distance I 按距离统计房屋对数目 I

__Description:__

You are given three __positive__ integers `n`, `x`, and `y`.

In a city, there exist houses numbered `1` to `n` connected by `n` streets. There is a street connecting the house numbered `i` with the house numbered `i + 1` for all `1 <= i <= n - 1` . An additional street connects the house numbered `x` with the house numbered `y`.

For each `k`, such that `1 <= k <= n`, you need to find the number of __pairs of houses__ `(house1, house2)` such that the __minimum__ number of streets that need to be traveled to reach `house2` from `house1` is `k`.

Return _a __1-indexed__ array_ `result` _of length_ `n` _where_ `result[k]` _represents the __total__ number of pairs of houses such that the __minimum__ streets required to reach one house from the other is_ `k`.

__Note__ that `x` and `y` can be __equal__.

__Example:__

Example 1:

![3015-1](https://assets.leetcode.com/uploads/2023/12/20/example2.png)

```text
Input: n = 3, x = 1, y = 3
Output: [6,0,0]
Explanation: Let's look at each pair of houses:
```

- For the pair (1, 2), we can go from house 1 to house 2 directly.
- For the pair (2, 1), we can go from house 2 to house 1 directly.
- For the pair (1, 3), we can go from house 1 to house 3 directly.
- For the pair (3, 1), we can go from house 3 to house 1 directly.
- For the pair (2, 3), we can go from house 2 to house 3 directly.
- For the pair (3, 2), we can go from house 3 to house 2 directly.

Example 2:

![3015-2](https://assets.leetcode.com/uploads/2023/12/20/example3.png)

```text
Input: n = 5, x = 2, y = 4
Output: [10,8,2,0,0]
Explanation: For each distance k the pairs are:
```

- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (2, 4), (4, 2), (3, 4), (4, 3), (4, 5), and (5, 4).
- For k == 2, the pairs are (1, 3), (3, 1), (1, 4), (4, 1), (2, 5), (5, 2), (3, 5), and (5, 3).
- For k == 3, the pairs are (1, 5), and (5, 1).
- For k == 4 and k == 5, there are no pairs.

Example 3:

![3015-3](https://assets.leetcode.com/uploads/2023/12/20/example5.png)

```text
Input: n = 4, x = 1, y = 1
Output: [6,4,2,0]
Explanation: For each distance k the pairs are:
- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (3, 4), and (4, 3).
- For k == 2, the pairs are (1, 3), (3, 1), (2, 4), and (4, 2).
- For k == 3, the pairs are (1, 4), and (4, 1).
- For k == 4, there are no pairs.
```

__Constraints:__

- `2 <= n <= 100`
- `1 <= x, y <= n`

__题目描述:__

给你三个 __正整数__ `n` 、 `x` 和 `y` 。

在城市中，存在编号从 `1` 到 `n` 的房屋，由 `n` 条街道相连。对所有 `1 <= i < n` ，都存在一条街道连接编号为 `i` 的房屋与编号为 `i + 1` 的房屋。另存在一条街道连接编号为 `x` 的房屋与编号为 `y` 的房屋。

对于每个 `k`（ `1 <= k <= n`），你需要找出所有满足要求的 __房屋对__ `[house1, house2]` ，即从 `house1` 到 `house2` 需要经过的 __最少__ 街道数为 `k` 。

返回一个下标从 __1__ 开始且长度为 `n` 的数组 `result` ，其中 `result[k]` 表示所有满足要求的房屋对的数量，即从一个房屋到另一个房屋需要经过的 __最少__ 街道数为 `k` 。

__注意__， `x` 与 `y` 可以 __相等__ 。

__示例:__

示例 1：

![3015-4](https://assets.leetcode.com/uploads/2023/12/20/example2.png)

```text
输入：n = 3, x = 1, y = 3
输出：[6,0,0]
解释：让我们检视每个房屋对
```

- 对于房屋对 (1, 2)，可以直接从房屋 1 到房屋 2。
- 对于房屋对 (2, 1)，可以直接从房屋 2 到房屋 1。
- 对于房屋对 (1, 3)，可以直接从房屋 1 到房屋 3。
- 对于房屋对 (3, 1)，可以直接从房屋 3 到房屋 1。
- 对于房屋对 (2, 3)，可以直接从房屋 2 到房屋 3。
- 对于房屋对 (3, 2)，可以直接从房屋 3 到房屋 2。

示例 2：

![3015-5](https://assets.leetcode.com/uploads/2023/12/20/example3.png)

```text
输入：n = 5, x = 2, y = 4
输出：[10,8,2,0,0]
解释：对于每个距离 k ，满足要求的房屋对如下：
```

- 对于 k == 1，满足要求的房屋对有 (1, 2), (2, 1), (2, 3), (3, 2), (2, 4), (4, 2), (3, 4), (4, 3), (4, 5), 以及 (5, 4)。
- 对于 k == 2，满足要求的房屋对有 (1, 3), (3, 1), (1, 4), (4, 1), (2, 5), (5, 2), (3, 5), 以及 (5, 3)。
- 对于 k == 3，满足要求的房屋对有 (1, 5)，以及 (5, 1) 。
- 对于 k == 4 和 k == 5，不存在满足要求的房屋对。

示例 3：

![3015-6](https://assets.leetcode.com/uploads/2023/12/20/example5.png)

```text
输入：n = 4, x = 1, y = 1
输出：[6,4,2,0]
解释：对于每个距离 k ，满足要求的房屋对如下：
```

- 对于 k == 1，满足要求的房屋对有 (1, 2), (2, 1), (2, 3), (3, 2), (3, 4), 以及 (4, 3)。
- 对于 k == 2，满足要求的房屋对有 (1, 3), (3, 1), (2, 4), 以及 (4, 2)。
- 对于 k == 3，满足要求的房屋对有 (1, 4), 以及 (4, 1)。
- 对于 k == 4，不存在满足要求的房屋对。

__提示：__

- `2 <= n <= 100`
- `1 <= x, y <= n`

__思路:__

```text
枚举
枚举任意两个点, 计算需要的最小距离
除相邻两点之间连边外, 只有 x 和 y 之间连边
所有 i 到 j 的最短距离就是 
min(i - j, abs(x - i - 1) + abs(y - j - 1) + 1)
注意房屋编号从 1 开始
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countOfPairs(int n, int x, int y) 
    {
        vector<int> result(n);
        for (int i = 0, a = min(x, y), b = max(x, y); i < n; i++) for (int j = i + 1; j < n; j++) result[min(j - i, abs(a - i - 1) + abs(b - j - 1) + 1) - 1] += 2;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countOfPairs(int n, int x, int y) {
        int a = Math.min(x, y), b = Math.max(x, y), result[] = new int[n];
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) result[Math.min(j - i, Math.abs(a - i - 1) + Math.abs(b - j - 1) + 1) - 1] += 2;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countOfPairs(self, n: int, x: int, y: int) -> List[int]:
        x, y, result = min(x, y), max(x, y), [0] * n
        for i in range(n):
            for j in range(i + 1, n):
                result[min(j - i, abs(x - i - 1) + abs(y - j - 1) + 1) - 1] += 2
        return result
```
