# 2001 Number of Pairs of Interchangeable Rectangles 可互换矩形的组数

__Description:__

You are given `n` rectangles represented by a __0-indexed__ 2D integer array `rectangles`, where `rectangles[i] = [widthi, heighti]` denotes the width and height of the `i ^ th` rectangle.

Two rectangles `i` and `j` ( `i < j`) are considered __interchangeable__ if they have the __same__ width-to-height ratio. More formally, two rectangles are __interchangeable__ if `widthi/heighti == widthj/heightj` (using decimal division, not integer division).

Return _the __number__ of pairs of __interchangeable__ rectangles in_ `rectangles`.

__Example:__

Example 1:

```text
Input: rectangles = [[4,8],[3,6],[10,20],[15,30]]
Output: 6
Explanation: The following are the interchangeable pairs of rectangles by index (0-indexed):
```

- Rectangle 0 with rectangle 1: 4/8 == 3/6.
- Rectangle 0 with rectangle 2: 4/8 == 10/20.
- Rectangle 0 with rectangle 3: 4/8 == 15/30.
- Rectangle 1 with rectangle 2: 3/6 == 10/20.
- Rectangle 1 with rectangle 3: 3/6 == 15/30.
- Rectangle 2 with rectangle 3: 10/20 == 15/30.

Example 2:

```text
Input: rectangles = [[4,5],[7,8]]
Output: 0
Explanation: There are no interchangeable pairs of rectangles.
```

__Constraints:__

- `n == rectangles.length`
- `1 <= n <= 10 ^ 5`
- `rectangles[i].length == 2`
- `1 <= widthi, heighti <= 10 ^ 5`

__题目描述:__

用一个下标从 __0__ 开始的二维整数数组 `rectangles` 来表示 `n` 个矩形，其中 `rectangles[i] = [widthi, heighti]` 表示第 `i` 个矩形的宽度和高度。

如果两个矩形 `i` 和 `j`（ `i < j`）的宽高比相同，则认为这两个矩形 __可互换__ 。更规范的说法是，两个矩形满足 `widthi/heighti == widthj/heightj`（使用实数除法而非整数除法），则认为这两个矩形 __可互换__ 。

计算并返回 `rectangles` 中有多少对 __可互换__ 矩形。

__示例:__

示例 1：

```text
输入：rectangles = [[4,8],[3,6],[10,20],[15,30]]
输出：6
解释：下面按下标（从 0 开始）列出可互换矩形的配对情况：
```

- 矩形 0 和矩形 1 ：4/8 == 3/6
- 矩形 0 和矩形 2 ：4/8 == 10/20
- 矩形 0 和矩形 3 ：4/8 == 15/30
- 矩形 1 和矩形 2 ：3/6 == 10/20
- 矩形 1 和矩形 3 ：3/6 == 15/30
- 矩形 2 和矩形 3 ：10/20 == 15/30

示例 2：

```text
输入：rectangles = [[4,5],[7,8]]
输出：0
解释：不存在成对的可互换矩形。
```

__提示：__

- `n == rectangles.length`
- `1 <= n <= 10 ^ 5`
- `rectangles[i].length == 2`
- `1 <= widthi, heighti <= 10 ^ 5`

__思路:__

```text
数学
将宽和高化简为最简分数, 使用哈希表记录化简后的分数, 以及对应的个数 n
统计 (n * (n - 1)) / 2 个数的和
或者在每次加入哈希表的时候, 直接加上 n
时间复杂度为 O(NlogM), 空间复杂度为 O(N), M 为数组元素范围
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long interchangeableRectangles(vector<vector<int>>& rectangles) 
    {
        unordered_map<int, unordered_map<int, long long>> d;
        for (const auto& r : rectangles) ++d[r.front() / gcd(r.front(), r.back())][r.back() / gcd(r.front(), r.back())];
        long long result = 0;
        for (auto iter = d.begin(); iter != d.end(); iter++) for (auto i = iter -> second.begin(); i != iter -> second.end(); i++) result += i -> second * (i -> second - 1) >> 1;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long interchangeableRectangles(int[][] rectangles) {
        Map<Long, Long> map = new HashMap<>();
        long result = 0L, offset = 100000;
        for (int[] r : rectangles) {
            long value = r[0] / gcd(r[0], r[1]) * offset + r[1] / gcd(r[0], r[1]);
            result += map.getOrDefault(value, 0L);
            map.merge(value, 1L, Long::sum);
        }
        return result;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def interchangeableRectangles(self, rectangles: List[List[int]]) -> int:
        d, result = defaultdict(int), 0
        for w, h in rectangles:
            result += d[w / h]
            d[w / h] += 1
        return result
```
