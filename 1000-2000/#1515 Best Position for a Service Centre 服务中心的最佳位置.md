# 1515 Best Position for a Service Centre 服务中心的最佳位置

__Description:__

A delivery company wants to build a new service center in a new city. The company knows the positions of all the customers in this city on a 2D-Map and wants to build the new center in a position such that the sum of the euclidean distances to all customers is minimum.

Given an array `positions` where `positions[i] = [xi, yi]` is the position of the `ith` customer on the map, return _the minimum sum of the euclidean distances_ to all customers.

In other words, you need to choose the position of the service center `[xcentre, ycentre]` such that the following formula is minimized:

![1515-1](https://assets.leetcode.com/uploads/2020/06/25/q4_edited.jpg)

Answers within `10 ^ -5` of the actual value will be accepted.

__Example:__

Example 1:

![1515-2](https://assets.leetcode.com/uploads/2020/06/25/q4_e1.jpg)

```text
Input: positions = [[0,1],[1,0],[1,2],[2,1]]
Output: 4.00000
Explanation: As shown, you can see that choosing [xcentre, ycentre] = [1, 1] will make the distance to each customer = 1, the sum of all distances is 4 which is the minimum possible we can achieve.
```

Example 2:

![1515-3](https://assets.leetcode.com/uploads/2020/06/25/q4_e3.jpg)

```text
Input: positions = [[1,1],[3,3]]
Output: 2.82843
Explanation: The minimum possible sum of distances = sqrt(2) + sqrt(2) = 2.82843
```

__Constraints:__

- `1 <= positions.length <= 50`
- `positions[i].length == 2`
- `0 <= xi, yi <= 100`

__题目描述:__

一家快递公司希望在新城市建立新的服务中心。公司统计了该城市所有客户在二维地图上的坐标，并希望能够以此为依据为新的服务中心选址：使服务中心 到所有客户的欧几里得距离的总和最小 。

给你一个数组 `positions` ，其中 `positions[i] = [xi, yi]` 表示第 `i` 个客户在二维地图上的位置，返回到所有客户的 __欧几里得距离的最小总和 。__

换句话说，请你为服务中心选址，该位置的坐标 `[xcentre, ycentre]` 需要使下面的公式取到最小值：

![1515-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/q4_edited.jpg)

与真实值误差在 `10 ^ -5`之内的答案将被视作正确答案。

__示例:__

示例 1：

![1515-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/q4_e1.jpg)

```text
输入：positions = [[0,1],[1,0],[1,2],[2,1]]
输出：4.00000
解释：如图所示，你可以选 [xcentre, ycentre] = [1, 1] 作为新中心的位置，这样一来到每个客户的距离就都是 1，所有距离之和为 4 ，这也是可以找到的最小值。
```

示例 2：

![1515-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/q4_e3.jpg)

```text
输入：positions = [[1,1],[3,3]]
输出：2.82843
解释：欧几里得距离可能的最小总和为 sqrt(2) + sqrt(2) = 2.82843
```

__提示：__

- `1 <= positions.length <= 50`
- `positions[i].length == 2`
- `0 <= xi, yi <= 100`

__思路:__

```text
梯度下降法
本题没有解析法, 也无法估计时间复杂度
采取搜索的方式得到一个局部最优解来代替全局最优解
f = (x_mean - xi) ^ 2 + (y_mean - yi) ^ 2
将 f 分别对 x, y 求偏导
使用梯度的反向
从 (x_mean, y_mean) 开始进行迭代
x_next = x_pre - af'x
y_next = y_pre - af'x
其中 a 表示学习率
注意在每次迭代时需要防止分母为 0
如果更新的值很小(1e-7, 或者 1e-8), 就退出迭代
给学习率设置一个衰减, 保证不会在最优解附近震荡
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    double getMinDistSum(vector<vector<int>>& positions) 
    {
        double eps = 1e-8, alpha = 1, decay = 1e-3, x = 0.0, y = 0.0, result = 0;
        int n = positions.size();
        for (const auto& pos: positions) 
        {
            x += pos.front();
            y += pos.back();
        }
        x /= n;
        y /= n;
        while (true) 
        {
            double pre_x = x, pre_y = y, dx = 0.0, dy = 0.0;
            for (const auto& pos: positions) 
            {
                dx += (x - pos.front()) / (sqrt((x - pos.front()) * (x - pos.front()) + (y - pos.back()) * (y - pos.back())) + eps);
                dy += (y - pos.back()) / (sqrt((x - pos.front()) * (x - pos.front()) + (y - pos.back()) * (y - pos.back())) + eps);
            }
            x -= alpha * dx;
            y -= alpha * dy;
            alpha *= (1.0 - decay);
            if (sqrt((x - pre_x) * (x - pre_x) + (y - pre_y) * (y - pre_y)) < eps) break;
        }
        for (const auto& pos: positions) result += sqrt((pos.front() - x) * (pos.front() - x) + (pos.back() - y) * (pos.back() - y));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public double getMinDistSum(int[][] positions) {
        double eps = 1e-7, alpha = 1, decay = 1e-3, x = 0.0, y = 0.0, result = 0;
        int n = positions.length;
        for (int[] pos : positions) {
            x += pos[0];
            y += pos[1];
        }
        x /= n;
        y /= n;
        while (true) {
            double preX = x, preY = y, dx = 0.0, dy = 0.0; 
            for (int[] pos: positions) {
                dx += (x - pos[0]) / (Math.sqrt((x - pos[0]) * (x - pos[0]) + (y - pos[1]) * (y - pos[1])) + eps);
                dy += (y - pos[1]) / (Math.sqrt((x - pos[0]) * (x - pos[0]) + (y - pos[1]) * (y - pos[1])) + eps);
            }
            x -= alpha * dx;
            y -= alpha * dy;
            alpha *= (1.0 - decay);
            if (Math.sqrt((x - preX) * (x - preX) + (y - preY) * (y - preY)) < eps) break;
        }
        for (int[] pos : positions) result += Math.sqrt((pos[0] - x) * (pos[0] - x) + (pos[1] - y) * (pos[1] - y));
        return result;
    }
}
```

__Python__:

```Python
from scipy.optimize import minimize
class Solution:
    def getMinDistSum(self, positions: List[List[int]]) -> float:
        return minimize(lambda x: sum([sqrt((x[0] - x1) ** 2 + (x[1] - y1) ** 2) for x1, y1 in positions]), [50, 50]).fun
```
