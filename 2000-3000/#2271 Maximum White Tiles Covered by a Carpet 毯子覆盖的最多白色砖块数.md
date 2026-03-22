# 2271 Maximum White Tiles Covered by a Carpet 毯子覆盖的最多白色砖块数

__Description:__

You are given a 2D integer array `tiles` where `tiles[i] = [li, ri]` represents that every tile `j` in the range `li <= j <= ri` is colored white.

You are also given an integer `carpetLen`, the length of a single carpet that can be placed __anywhere__.

Return the maximum number of white tiles that can be covered by the carpet.

__Example:__

Example 1:

![2271-1](https://assets.leetcode.com/uploads/2022/03/25/example1drawio3.png)

```text
Input: tiles = [[1,5],[10,11],[12,18],[20,25],[30,32]], carpetLen = 10
Output: 9
Explanation: Place the carpet starting on tile 10. 
It covers 9 white tiles, so we return 9.
Note that there may be other places where the carpet covers 9 white tiles.
It can be shown that the carpet cannot cover more than 9 white tiles.
```

Example 2:

![2271-2](https://assets.leetcode.com/uploads/2022/03/24/example2drawio.png)

```text
Input: tiles = [[10,11],[1,1]], carpetLen = 2
Output: 2
Explanation: Place the carpet starting on tile 10. 
It covers 2 white tiles, so we return 2.
```

__Constraints:__

- `1 <= tiles.length <= 5 * 10 ^ 4`
- `tiles[i].length == 2`
- `1 <= li <= ri <= 10 ^ 9`
- `1 <= carpetLen <= 10 ^ 9`
- The `tiles` are __non-overlapping__.

__题目描述:__

给你一个二维整数数组 `tiles` ，其中 `tiles[i] = [li, ri]` ，表示所有在 `li <= j <= ri` 之间的每个瓷砖位置 `j` 都被涂成了白色。

同时给你一个整数 `carpetLen` ，表示可以放在 __任何位置__ 的一块毯子的长度。

请你返回使用这块毯子，最多 可以盖住多少块瓷砖。

__示例:__

示例 1：

![2271-3](https://assets.leetcode.com/uploads/2022/03/25/example1drawio3.png)

```text
输入：tiles = [[1,5],[10,11],[12,18],[20,25],[30,32]], carpetLen = 10
输出：9
解释：将毯子从瓷砖 10 开始放置。
总共覆盖 9 块瓷砖，所以返回 9 。
注意可能有其他方案也可以覆盖 9 块瓷砖。
可以看出，瓷砖无法覆盖超过 9 块瓷砖。
```

示例 2：

![2271-4](https://assets.leetcode.com/uploads/2022/03/24/example2drawio.png)

```text
输入：tiles = [[10,11],[1,1]], carpetLen = 2
输出：2
解释：将毯子从瓷砖 10 开始放置。
总共覆盖 2 块瓷砖，所以我们返回 2 。
```

__提示：__

- `1 <= tiles.length <= 5 * 10 ^ 4`
- `tiles[i].length == 2`
- `1 <= li <= ri <= 10 ^ 9`
- `1 <= carpetLen <= 10 ^ 9`
- `tiles` 互相 __不会重叠__ 。

__思路:__

```text
贪心
将瓷砖按左端点排序
尝试将毯子放在每个瓷砖的右端点, 计算覆盖的白色砖块数
此时瓷砖应该被毯子完全覆盖
所以如果瓷砖的右端点小于毯子的左端点, 则移动到下一个瓷砖
将覆盖范围更新为 cover - max(right - carpetLen + 1 - tiles[i][0], 0), 因为毯子有可能在瓷砖的内部
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要开销在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumWhiteTiles(vector<vector<int>>& tiles, int carpetLen) 
    {
        sort(tiles.begin(), tiles.end(), [](auto& a, auto& b) { return a.front() < b.front(); });
        int result = 0, i = 0, cover = 0, left = 0, right = 0;
        for (const auto& tile : tiles) 
        {
            cover += (right = tile.back()) - (left = tile.front()) + 1;
            while (tiles[i].back() < right - carpetLen + 1) cover -= tiles[i].back() - tiles[i++].front() + 1;
            result = max(result, cover - max(right - carpetLen + 1 - tiles[i].front(), 0));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumWhiteTiles(int[][] tiles, int carpetLen) {
        Arrays.sort(tiles, (a, b) -> a[0] - b[0]);
        int result = 0, i = 0, cover = 0, left = 0, right = 0;
        for (int[] tile : tiles) {
            cover += (right = tile[1]) - (left = tile[0]) + 1;
            while (tiles[i][1] < right - carpetLen + 1) cover -= tiles[i][1] - tiles[i++][0] + 1;
            result = Math.max(result, cover - Math.max(right - carpetLen + 1 - tiles[i][0], 0));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumWhiteTiles(self, tiles: List[List[int]], carpetLen: int) -> int:
        tiles.sort(key=lambda x: x[0])
        result = cover = i = 0
        for left, right in tiles:
            cover += right - left + 1
            while tiles[i][1] < right - carpetLen + 1:
                cover -= tiles[i][1] - tiles[i][0] + 1
                i += 1
            result = max(result, cover - max(right - carpetLen + 1 - tiles[i][0], 0))
        return result
```
