# 3017 Count the Number of Houses at a Certain Distance II 按距离统计房屋对数目 II

__Description:__

You are given three __positive__ integers `n`, `x`, and `y`.

In a city, there exist houses numbered `1` to `n` connected by `n` streets. There is a street connecting the house numbered `i` with the house numbered `i + 1` for all `1 <= i <= n - 1` . An additional street connects the house numbered `x` with the house numbered `y`.

For each `k`, such that `1 <= k <= n`, you need to find the number of __pairs of houses__ `(house1, house2)` such that the __minimum__ number of streets that need to be traveled to reach `house2` from `house1` is `k`.

Return _a __1-indexed__ array_ `result` _of length_ `n` _where_ `result[k]` _represents the __total__ number of pairs of houses such that the __minimum__ streets required to reach one house from the other is_ `k`.

__Note__ that `x` and `y` can be __equal__.

__Example:__

Example 1:

![3017-1](https://assets.leetcode.com/uploads/2023/12/20/example2.png)

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

![3017-2](https://assets.leetcode.com/uploads/2023/12/20/example3.png)

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

![3017-3](https://assets.leetcode.com/uploads/2023/12/20/example5.png)

```text
Input: n = 4, x = 1, y = 1
Output: [6,4,2,0]
Explanation: For each distance k the pairs are:
```

- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (3, 4), and (4, 3).
- For k == 2, the pairs are (1, 3), (3, 1), (2, 4), and (4, 2).
- For k == 3, the pairs are (1, 4), and (4, 1).
- For k == 4, there are no pairs.

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `1 <= x, y <= n`

__题目描述:__

给你三个 __正整数__ `n` 、 `x` 和 `y` 。

在城市中，存在编号从 `1` 到 `n` 的房屋，由 `n` 条街道相连。对所有 `1 <= i < n` ，都存在一条街道连接编号为 `i` 的房屋与编号为 `i + 1` 的房屋。另存在一条街道连接编号为 `x` 的房屋与编号为 `y` 的房屋。

对于每个 `k`（ `1 <= k <= n`），你需要找出所有满足要求的 __房屋对__ `[house1, house2]` ，即从 `house1` 到 `house2` 需要经过的 __最少__ 街道数为 `k` 。

返回一个下标从 __1__ 开始且长度为 `n` 的数组 `result` ，其中 `result[k]` 表示所有满足要求的房屋对的数量，即从一个房屋到另一个房屋需要经过的 __最少__ 街道数为 `k` 。

__注意__， `x` 与 `y` 可以 __相等__ 。

__示例:__

示例 1：

![3017-4](https://assets.leetcode.com/uploads/2023/12/20/example2.png)

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

![3017-5](https://assets.leetcode.com/uploads/2023/12/20/example3.png)

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

![3017-6](https://assets.leetcode.com/uploads/2023/12/20/example5.png)

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

- `2 <= n <= 10 ^ 5`
- `1 <= x, y <= n`

__思路:__

```text
差分数组
如果不考虑 x 到 y 的捷径
另外只考虑 i < j 的情况
则对于所有 i 到右边的房子的距离为 1, 2, 3, ..., n - i
相当于把 [1, n - i] 区间内都加上 1
现在加上 x 到 y 的捷径
不妨设 x <= y, 如果不是, 就交换 x 和 y 不影响结果
此时如果 x + 1 >= y, 则这条捷径没有意义, 可以直接返回 range((n - 1) << 1, -1, -2)
下面分类讨论
1. 如果 i <= x
对于在 [x, y] 范围内的 j 如果要走捷径那说明从 i 到 x 再到 y 并返回 j 的距离比从 i 到 j 更小, 即 x - i + y - j + 1 < j - i, 即 j > (x + y + 1) / 2, 令 k = (x + y + 1) / 2
1.1 对于 [i + 1, k] 的房子可以直接到达所以将 [1, k - i] 区间加上 1
1.2 对于 [k + 1, y - 1] 可以走 x -> y 捷径到达, 从 i 走到 y - 1 需要 x - i + 2 步, 从 i 走到 k + 1 需要 x - i + 1 + y - (k + 1) 步, 那么需要将 [x - i + 2, x + y - i - k] 区间加上 1
1.3 对于 [y, n] 也可以走捷径到达, 从 i 到 y 的距离为 x - i + 1, 从 i 到 n 的距离为 x - i + 1 + n - y
2. 如果 x < i < (x + y) / 2 与 1 的讨论类似
3. 如果 i 为更大的情况, 不需要走捷径直接把 [1, n - i] 加上 1
处理完查分数组之后, 直接求前缀和的两倍即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> countOfPairs(int n, int x, int y) 
    {
        if (x > y) return countOfPairs(n, y, x);
        vector<long long> result(n);
        if (x + 1 >= y)
        {
            for (int i = 1; i <= n; i++) result[i - 1] = (n - i) << 1LL;
            return result;
        }
        vector<int> diff(n + 1);
        auto add = [&](int l, int r) -> void
        {
            ++diff[l];
            --diff[r + 1];
        };
        for (int i = 1, k = 0; i < n; i++)
        {
            if (i <= x)
            {
                add(1, (k = (x + y + 1) >> 1) - i);
                add(x - i + 2, x - i + y - k);
                add(x - i + 1, x - i + 1 + n - y);
            }
            else if (i < (x + y) >> 1)
            {
                add(1, (k = i + ((y - x + 1) >> 1)) - i);
                add(i - x + 2, i - x + y - k);
                add(i - x + 1, i - x + 1 + n - y);
            }
            else add(1, n - i);
        }
        long long s = 0LL;
        for (int i = 1; i <= n; i++)
        {
            s += diff[i];
            result[i - 1] = s << 1LL;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] diff;

    public long[] countOfPairs(int n, int x, int y) {
        if (x > y) return countOfPairs(n, y, x);
        long result[] = new long[n], s = 0L;
        if (x + 1 >= y) {
            for (int i = 1; i < n; i++) result[i - 1] = (n - i) << 1;
            return result;
        }
        diff = new int[n + 1];
        for (int i = 1, k = 0; i < n; i++) {
            if (i <= x) {
                add(1, (k = (x + y + 1) >>> 1) - i);
                add(x - i + 2, x - i + y - k);
                add(x - i + 1, x - i + 1 + n - y);
            } else if (i < (x + y) >>> 1) {
                add(1, (k = i + ((y - x + 1) >>> 1)) - i);
                add(i - x + 2, i - x + y - k);
                add(i - x + 1, i - x + 1 + n - y);
            } else add(1, n - i);
        }
        for (int i = 0; i < n; i++) {
            s += diff[i + 1];
            result[i] = s << 1L;
        }
        return result;
    }

    private void add(int l, int r) {
        ++diff[l];
        --diff[r + 1];
    }
}
```

__Python__:

```Python
class Solution:
    def countOfPairs(self, n: int, x: int, y: int) -> List[int]:
        if (a := min(x, y)) + 1 >= (b := max(x, y)):
            return list(range((n - 1) << 1, -1, -2))
        diff = [0] * (n + 1)

        def add(l: int, r: int) -> None:
            diff[l] += 2
            diff[r + 1] -= 2
        for i in range(1, n):
            if i <= a:
                add(1, (k := (x + y + 1) >> 1) - i)
                add(a - i + 2, x - i + y - k)
                add(a - i + 1, a - i + 1 + n - b)
            elif i < (x + y) >> 1:
                add(1, (k := i + ((b - a + 1) >> 1)) - i)
                add(i - a + 2, i - a + b - k)
                add(i - a + 1, i - a + 1 + n - b)
            else:
                add(1, n - i)
        return list(accumulate(diff))[1:]
```
