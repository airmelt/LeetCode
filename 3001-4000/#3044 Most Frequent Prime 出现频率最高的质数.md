# 3044 Most Frequent Prime 出现频率最高的质数

__Description:__

You are given a `m x n` __0-indexed__ 2D matrix `mat`. From every cell, you can create numbers in the following way:

- There could be at most `8` paths from the cells namely: east, south-east, south, south-west, west, north-west, north, and north-east.
- Select a path from them and append digits in this path to the number being formed by traveling in this direction.
- Note that numbers are generated at every step, for example, if the digits along the path are `1, 9, 1`, then there will be three numbers generated along the way: `1, 19, 191`.

Return _the most frequent prime number __greater__ than_ `10` _out of all the numbers created by traversing the matrix or_ `-1` _if no such prime number exists. If there are multiple prime numbers with the highest frequency, then return the _largest_ among them._

Note: It is invalid to change the direction during the move.

__Example:__

Example 1:

![3044-1](https://assets.leetcode.com/uploads/2024/02/15/south)

```text
Input: mat = [[1,1],[9,9],[1,1]]
Output: 19
Explanation: 
From cell (0,0) there are 3 possible directions and the numbers greater than 10 which can be created in those directions are:
East: [11], South-East: [19], South: [19,191].
Numbers greater than 10 created from the cell (0,1) in all possible directions are: [19,191,19,11].
Numbers greater than 10 created from the cell (1,0) in all possible directions are: [99,91,91,91,91].
Numbers greater than 10 created from the cell (1,1) in all possible directions are: [91,91,99,91,91].
Numbers greater than 10 created from the cell (2,0) in all possible directions are: [11,19,191,19].
Numbers greater than 10 created from the cell (2,1) in all possible directions are: [11,19,19,191].
The most frequent prime number among all the created numbers is 19.
```

Example 2:

```text
Input: mat = [[7]]
Output: -1
Explanation: The only number which can be formed is 7. It is a prime number however it is not greater than 10, so return -1.
```

Example 3:

```text
Input: mat = [[9,7,8],[4,6,5],[2,8,6]]
Output: 97
Explanation: 
Numbers greater than 10 created from the cell (0,0) in all possible directions are: [97,978,96,966,94,942].
Numbers greater than 10 created from the cell (0,1) in all possible directions are: [78,75,76,768,74,79].
Numbers greater than 10 created from the cell (0,2) in all possible directions are: [85,856,86,862,87,879].
Numbers greater than 10 created from the cell (1,0) in all possible directions are: [46,465,48,42,49,47].
Numbers greater than 10 created from the cell (1,1) in all possible directions are: [65,66,68,62,64,69,67,68].
Numbers greater than 10 created from the cell (1,2) in all possible directions are: [56,58,56,564,57,58].
Numbers greater than 10 created from the cell (2,0) in all possible directions are: [28,286,24,249,26,268].
Numbers greater than 10 created from the cell (2,1) in all possible directions are: [86,82,84,86,867,85].
Numbers greater than 10 created from the cell (2,2) in all possible directions are: [68,682,66,669,65,658].
The most frequent prime number among all the created numbers is 97.
```

__Constraints:__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 6`
- `1 <= mat[i][j] <= 9`

__题目描述:__

给你一个大小为 `m x n` 、下标从 __0__ 开始的二维矩阵 `mat` 。在每个单元格，你可以按以下方式生成数字:

- 最多有 `8` 条路径可以选择:东，东南，南，西南，西，西北，北，东北。
- 选择其中一条路径，沿着这个方向移动，并且将路径上的数字添加到正在形成的数字后面。
- 注意，每一步都会生成数字，例如，如果路径上的数字是 `1, 9, 1`，那么在这个方向上会生成三个数字: `1, 19, 191` 。

返回在遍历矩阵所创建的所有数字中，出现频率最高的、__大于__ `10`的质数；如果不存在这样的质数，则返回 `-1` 。如果存在多个出现频率最高的质数，那么返回其中最大的那个。

注意：移动过程中不允许改变方向。

__示例:__

示例 1：

![3044-2](https://assets.leetcode.com/uploads/2024/02/15/south)

```text
输入：mat = [[1,1],[9,9],[1,1]]
输出：19
解释： 
从单元格 (0,0) 出发，有 3 个可能的方向，这些方向上可以生成的大于 10 的数字有：
东方向: [11], 东南方向: [19], 南方向: [19,191] 。
从单元格 (0,1) 出发，所有可能方向上生成的大于 10 的数字有：[19,191,19,11] 。
从单元格 (1,0) 出发，所有可能方向上生成的大于 10 的数字有：[99,91,91,91,91] 。
从单元格 (1,1) 出发，所有可能方向上生成的大于 10 的数字有：[91,91,99,91,91] 。
从单元格 (2,0) 出发，所有可能方向上生成的大于 10 的数字有：[11,19,191,19] 。
从单元格 (2,1) 出发，所有可能方向上生成的大于 10 的数字有：[11,19,19,191] 。
在所有生成的数字中，出现频率最高的质数是 19 。
```

示例 2：

```text
输入：mat = [[7]]
输出：-1
解释：唯一可以生成的数字是 7 。它是一个质数，但不大于 10 ，所以返回 -1 。
```

示例 3：

```text
输入：mat = [[9,7,8],[4,6,5],[2,8,6]]
输出：97
解释： 
从单元格 (0,0) 出发，所有可能方向上生成的大于 10 的数字有: [97,978,96,966,94,942] 。
从单元格 (0,1) 出发，所有可能方向上生成的大于 10 的数字有: [78,75,76,768,74,79] 。
从单元格 (0,2) 出发，所有可能方向上生成的大于 10 的数字有: [85,856,86,862,87,879] 。
从单元格 (1,0) 出发，所有可能方向上生成的大于 10 的数字有: [46,465,48,42,49,47] 。
从单元格 (1,1) 出发，所有可能方向上生成的大于 10 的数字有: [65,66,68,62,64,69,67,68] 。
从单元格 (1,2) 出发，所有可能方向上生成的大于 10 的数字有: [56,58,56,564,57,58] 。
从单元格 (2,0) 出发，所有可能方向上生成的大于 10 的数字有: [28,286,24,249,26,268] 。
从单元格 (2,1) 出发，所有可能方向上生成的大于 10 的数字有: [86,82,84,86,867,85] 。
从单元格 (2,2) 出发，所有可能方向上生成的大于 10 的数字有: [68,682,66,669,65,658] 。
在所有生成的数字中，出现频率最高的质数是 97 。
```

__提示：__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 6`
- `1 <= mat[i][j] <= 9`

__思路:__

```text
DFS
将 8 个方向路径上的质数值都加入哈希表中
找到哈希表中值最大的
如果值相等就找到键最大的
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(MN ^ 2), 可以预处理质数表加快判断速度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int mostFrequentPrime(vector<vector<int>> &mat) 
    {
        int m = mat.size(), n = mat.front().size(), result = -1, mx = 0;
        unordered_map<int, int> c;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                for (const auto& d: DIRS) 
                {
                    int x = i + d[0], y = j + d[1], v = mat[i][j];
                    while (x > -1 and x < m and y > -1 and y < n) 
                    {
                        v = v * 10 + mat[x][y];
                        if (c.contains(v) or is_prime(v)) ++c[v];
                        x += d[0];
                        y += d[1];
                    }
                }
            }
        }
        for (const auto& [k, v]: c)
        {
            if (v > mx) 
            {
                result = k;
                mx = v;
            } 
            else if (v == mx) result = max(result, k);
        }
        return result;
    }
private:
    static constexpr int DIRS[8][2] = {{1, 0}, {1, 1}, {0, 1}, {-1, 1}, {-1, 0}, {-1, -1}, {0, -1}, {1, -1}};

    bool is_prime(int n) 
    {
        for (int i = 2; i * i <= n; i++) if (!(n % i)) return false;
        return true;
    }

};
```

__Java__:

```Java
class Solution {
    private static final int[][] DIRS = {{1, 0}, {1, 1}, {0, 1}, {-1, 1}, {-1, 0}, {-1, -1}, {0, -1}, {1, -1}};

    public int mostFrequentPrime(int[][] mat) {
        int m = mat.length, n = mat[0].length, result = -1, mx = 0;
        var c = new HashMap<Integer, Integer>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int[] d : DIRS) {
                    int x = i + d[0], y = j + d[1], v = mat[i][j];
                    while (x > -1 && x < m && y > -1 && y < n) {
                        v = v * 10 + mat[x][y];
                        if (isPrime(v)) c.merge(v, 1, Integer::sum);
                        x += d[0];
                        y += d[1];
                    }
                }
            }
        }
        for (var e : c.entrySet()) {
            if (e.getValue() > mx) {
                result = e.getKey();
                mx = e.getValue();
            } else if (e.getValue() == mx) result = Math.max(result, e.getKey());
        }
        return result;
    }

    private boolean isPrime(int n) {
        for (int i = 2; i * i <= n; i++) if (n % i == 0) return false;
        return true;
    }
}
```

__Python__:

```Python
def linear_primes(n: int) -> tuple[int, list[int]]:
    """
    return count of primes from [1, n] and list of primes
    :param n: right range
    :return:
    """
    n = max(2, n)
    primes = n * [0]
    count = 0
    is_prime = [0] * 2 + [1] * (n - 1)
    
    for i in range(2, n):
        if is_prime[i]:
            primes[count] = i
            count += 1
        for j in range(count):
            if primes[j] * i >= n:
                break
            is_prime[primes[j] * i] = 0
            if not i % primes[j]:
                break
    return count, primes

PRIMES = set(linear_primes(10 ** 7 + 1)[1])

class Solution:
    def mostFrequentPrime(self, mat: List[List[int]]) -> int:
        m, n, c, d = len(mat), len(mat[0]), defaultdict(int), [(1, 1), (1, -1), (-1, 1), (-1, -1), (1, 0), (-1, 0), (0, 1), (0, -1)]

        def dfs(x: int, y: int, cur: int, dx: int, dy: int) -> NoReturn:
            if -1 < x < m and -1 < y < n:
                if (cur := 10 * cur + mat[x][y]) in PRIMES:
                    c[cur] += 1
                dfs(x + dx, y + dy, cur, dx, dy)

        for i in range(m):
            for j in range(n):
                for dx, dy in d:
                    dfs(i, j, 0, dx, dy)
        return sorted(a, key=lambda p: (-p[1], -p[0]))[0][0] if (a := [(k, v) for k, v in c.items() if k > 10]) else -1
```
