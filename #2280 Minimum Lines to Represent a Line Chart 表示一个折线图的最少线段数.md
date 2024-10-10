# 2280 Minimum Lines to Represent a Line Chart 表示一个折线图的最少线段数

__Description:__

You are given a 2D integer array `stockPrices` where `stockPrices[i] = [day_i, price_i]` indicates the price of the stock on day `day_i` is `price_i`. A __line chart__ is created from the array by plotting the points on an XY plane with the X-axis representing the day and the Y-axis representing the price and connecting adjacent points. One such example is shown below:

![2280-1](https://assets.leetcode.com/uploads/2022/03/30/1920px-pushkin_population_historysvg.png)

Return the minimum number of lines needed to represent the line chart.

__Example:__

Example 1:

![2280-2](https://assets.leetcode.com/uploads/2022/03/30/ex0.png)

```text
Input: stockPrices = [[1,7],[2,6],[3,5],[4,4],[5,4],[6,3],[7,2],[8,1]]
Output: 3
Explanation:
The diagram above represents the input, with the X-axis representing the day and Y-axis representing the price.
The following 3 lines can be drawn to represent the line chart:
```

- Line 1 (in red) from (1,7) to (4,4) passing through (1,7), (2,6), (3,5), and (4,4).
- Line 2 (in blue) from (4,4) to (5,4).
- Line 3 (in green) from (5,4) to (8,1) passing through (5,4), (6,3), (7,2), and (8,1).

It can be shown that it is not possible to represent the line chart using less than 3 lines.

Example 2:

![2280-3](https://assets.leetcode.com/uploads/2022/03/30/ex1.png)

```text
Input: stockPrices = [[3,4],[1,2],[7,8],[2,3]]
Output: 1
Explanation:
As shown in the diagram above, the line chart can be represented with a single line.
```

__Constraints:__

- `1 <= stockPrices.length <= 10 ^ 5`
- `stockPrices[i].length == 2`
- `1 <= day_i, price_i <= 10 ^ 9`
- All `day_i` are __distinct__.

__题目描述:__

给你一个二维整数数组 `stockPrices` ，其中 `stockPrices[i] = [day_i, price_i]` 表示股票在 `day_i` 的价格为 `price_i` 。__折线图__ 是一个二维平面上的若干个点组成的图，横坐标表示日期，纵坐标表示价格，折线图由相邻的点连接而成。比方说下图是一个例子:

![2280-4](https://assets.leetcode.com/uploads/2022/03/30/1920px-pushkin_population_historysvg.png)

请你返回要表示一个折线图所需要的 最少线段数 。

__示例:__

示例 1：

![2280-5](https://assets.leetcode.com/uploads/2022/03/30/ex0.png)

```text
输入：stockPrices = [[1,7],[2,6],[3,5],[4,4],[5,4],[6,3],[7,2],[8,1]]
输出：3
解释：
上图为输入对应的图，横坐标表示日期，纵坐标表示价格。
以下 3 个线段可以表示折线图：
```

- 线段 1 （红色）从 (1,7) 到 (4,4) ，经过 (1,7) ，(2,6) ，(3,5) 和 (4,4) 。
- 线段 2 （蓝色）从 (4,4) 到 (5,4) 。
- 线段 3 （绿色）从 (5,4) 到 (8,1) ，经过 (5,4) ，(6,3) ，(7,2) 和 (8,1) 。

可以证明，无法用少于 3 条线段表示这个折线图。

示例 2：

![2280-6](https://assets.leetcode.com/uploads/2022/03/30/ex1.png)

```text
输入：stockPrices = [[3,4],[1,2],[7,8],[2,3]]
输出：1
解释：
如上图所示，折线图可以用一条线段表示。
```

__提示：__

- `1 <= stockPrices.length <= 10 ^ 5`
- `stockPrices[i].length == 2`
- `1 <= day_i, price_i <= 10 ^ 9`
- 所有 `day_i` __互不相同__ 。

__思路:__

```text
排序
按照日期排序
然后遍历, 每次计算两个点之间的斜率, 如果斜率不相等, 则需要增加一条线段
计算时为了避免除法, 使用乘法进行比较
dy = y2 - y1, dx = x2 - x1
dy * pre_dx != dx * pre_dy 时, result++
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要是排序的时间复杂度
```

__代码:__

__C++__:

```C++
class Solution {
    public int minimumLines(int[][] stockPrices) {
        Arrays.sort(stockPrices, Comparator.comparingInt(a -> a[0]));
        int result = 0, preY = 1, preX = 0, n = stockPrices.length;
        for (int i = 1, dy = 0, dx = 0; i < n; i++) {
            result += (dy = stockPrices[i][1] - stockPrices[i - 1][1]) * preX != (dx = stockPrices[i][0] - stockPrices[i - 1][0]) * preY ? 1 : 0;
            preY = dy;
            preX = dx;
        }
        return result;
    }
}
```

__Java__:

```Java
class Solution {
    public int minimumLines(int[][] stockPrices) {
        Arrays.sort(stockPrices, Comparator.comparingInt(a -> a[0]));
        int result = 0, preY = 1, preX = 0, n = stockPrices.length;
        for (int i = 1, dy = 0, dx = 0; i < n; i++) {
            result += (dy = stockPrices[i][1] - stockPrices[i - 1][1]) * preX != (dx = stockPrices[i][0] - stockPrices[i - 1][0]) * preY ? 1 : 0;
            preY = dy;
            preX = dx;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumLines(self, stockPrices: List[List[int]]) -> int:
        stockPrices.sort(key=lambda x: x[0])
        result, pre_dy, pre_dx = 0, 1, 0
        for (x1, y1), (x2, y2) in pairwise(stockPrices):
            result += (dy := y2 - y1) * pre_dx != pre_dy * (dx := x2 - x1)
            pre_dy, pre_dx = dy, dx
        return result
```
