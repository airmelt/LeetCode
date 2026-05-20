# 3027 Find the Number of Ways to Place People II 人员站位的方案数 II

__Description:__

You are given a 2D array `points` of size `n x 2` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

We define the right direction as positive x-axis (increasing x-coordinate) and the left direction as negative x-axis (decreasing x-coordinate). Similarly, we define the up direction as positive y-axis (increasing y-coordinate) and the down direction as negative y-axis (decreasing y-coordinate)

You have to place `n` people, including Alice and Bob, at these points such that there is __exactly one__ person at every point. Alice wants to be alone with Bob, so Alice will build a rectangular fence with Alice's position as the __upper left corner__ and Bob's position as the __lower right corner__ of the fence (__Note__ that the fence __might not__ enclose any area, i.e. it can be a line). If any person other than Alice and Bob is either __inside__ the fence or __on__ the fence, Alice will be sad.

Return the number of pairs of points where you can place Alice and Bob, such that Alice does not become sad on building the fence.

__Note__ that Alice can only build a fence with Alice's position as the upper left corner, and Bob's position as the lower right corner. For example, Alice cannot build either of the fences in the picture below with four corners `(1, 1)`, `(1, 3)`, `(3, 1)`, and `(3, 3)`, because:

- With Alice at `(3, 3)` and Bob at `(1, 1)`, Alice's position is not the upper left corner and Bob's position is not the lower right corner of the fence.
- With Alice at `(1, 3)` and Bob at `(1, 1)` (as the rectangle shown in the image instead of a line), Bob's position is not the lower right corner of the fence.

![3027-1](https://assets.leetcode.com/uploads/2024/01/04/example0alicebob-1.png)

__Example:__

Example 1:

![3027-2](https://assets.leetcode.com/uploads/2024/01/04/example1alicebob.png)

```text
Input: points = [[1,1],[2,2],[3,3]]
Output: 0
Explanation: There is no way to place Alice and Bob such that Alice can build a fence with Alice's position as the upper left corner and Bob's position as the lower right corner. Hence we return 0.
```

Example 2:

![3027-3](https://assets.leetcode.com/uploads/2024/02/04/example2alicebob.png)

```text
Input: points = [[6,2],[4,4],[2,6]]
Output: 2
Explanation: There are two ways to place Alice and Bob such that Alice will not be sad:
```

- Place Alice at (4, 4) and Bob at (6, 2).
- Place Alice at (2, 6) and Bob at (4, 4).

You cannot place Alice at (2, 6) and Bob at (6, 2) because the person at (4, 4) will be inside the fence.

Example 3:

![3027-4](https://assets.leetcode.com/uploads/2024/02/04/example4alicebob.png)

```text
Input: points = [[3,1],[1,3],[1,1]]
Output: 2
Explanation: There are two ways to place Alice and Bob such that Alice will not be sad:
```

- Place Alice at (1, 1) and Bob at (3, 1).
- Place Alice at (1, 3) and Bob at (1, 1).
You cannot place Alice at (1, 3) and Bob at (3, 1) because the person at (1, 1) will be on the fence.

Note that it does not matter if the fence encloses any area, the first and second fences in the image are valid.

__Constraints:__

- `2 <= n <= 1000`
- `points[i].length == 2`
- `-10 ^ 9 <= points[i][0], points[i][1] <= 10 ^ 9`
- All `points[i]` are distinct.

__题目描述:__

给你一个  `n x 2` 的二维数组 `points` ，它表示二维平面上的一些点坐标，其中 `points[i] = [xi, yi]` 。

我们定义 x 轴的正方向为 右 （x 轴递增的方向），x 轴的负方向为 左 （x 轴递减的方向）。类似的，我们定义 y 轴的正方向为 上 （y 轴递增的方向），y 轴的负方向为 下 （y 轴递减的方向）。

你需要安排这 `n` 个人的站位，这 `n` 个人中包括 Alice 和 Bob 。你需要确保每个点处 __恰好__ 有 __一个__ 人。同时，Alice 想跟 Bob 单独玩耍，所以 Alice 会以 _Alice_ 的坐标为 __左上角__ ，_Bob_ 的坐标为 __右下角__ 建立一个矩形的围栏（__注意__，围栏可能 __不__ 包含任何区域，也就是说围栏可能是一条线段）。如果围栏的 __内部__ 或者 __边缘__ 上有任何其他人，Alice 都会难过。

请你在确保 Alice 不会 难过的前提下，返回 Alice 和 Bob 可以选择的 点对 数目。

_注意_ ，Alice 建立的围栏必须确保 Alice 的位置是矩形的左上角，Bob 的位置是矩形的右下角。比方说，以 `(1, 1)` ， `(1, 3)` ， `(3, 1)` 和 `(3, 3)` 为矩形的四个角，给定下图的两个输入，Alice 都不能建立围栏，原因如下:

- 图一中，Alice 在 `(3, 3)` 且 Bob 在 `(1, 1)` ，Alice 的位置不是左上角且 Bob 的位置不是右下角。
- 图二中，Alice 在 `(1, 3)` 且 Bob 在 `(1, 1)`（如图所示，用矩形代替线条），Bob 的位置不是在围栏的右下角。

![3027-5](https://assets.leetcode.com/uploads/2024/01/04/example0alicebob-1.png)

__示例:__

示例 1：

![3027-6](https://assets.leetcode.com/uploads/2024/01/04/example1alicebob.png)

```text
输入：points = [[1,1],[2,2],[3,3]]
输出：0
解释：没有办法可以让 Alice 的围栏以 Alice 的位置为左上角且 Bob 的位置为右下角。所以我们返回 0 。
```

示例 2：

![3027-7](https://pic.leetcode.cn/1708226715-CxjXKb-20240218-112338.jpeg)

```text
输入：points = [[6,2],[4,4],[2,6]]
输出：2
解释：总共有 2 种方案安排 Alice 和 Bob 的位置，使得 Alice 不会难过：
```

- Alice 站在 (4, 4) ，Bob 站在 (6, 2) 。
- Alice 站在 (2, 6) ，Bob 站在 (4, 4) 。

不能安排 Alice 站在 (2, 6) 且 Bob 站在 (6, 2) ，因为站在 (4, 4) 的人处于围栏内。

示例 3：

![3027-8](https://pic.leetcode.cn/1708226721-wTbEuK-20240218-112351.jpeg)

```text
输入：points = [[3,1],[1,3],[1,1]]
输出：2
解释：总共有 2 种方案安排 Alice 和 Bob 的位置，使得 Alice 不会难过：
```

- Alice 站在 (1, 1) ，Bob 站在 (3, 1) 。
- Alice 站在 (1, 3) ，Bob 站在 (1, 1) 。

不能安排 Alice 站在 (1, 3) 且 Bob 站在 (3, 1) ，因为站在 (1, 1) 的人处于围栏内。

注意围栏是可以不包含任何面积的，上图中第一和第二个围栏都是合法的。

__提示：__

- `2 <= n <= 1000`
- `points[i].length == 2`
- `-10 ^ 9 <= points[i][0], points[i][1] <= 10 ^ 9`
- `points[i]` 点对两两不同。

__思路:__

```text
排序
对于两个点 (x1, y1), (x2, y2)
首先为了满足左上角和右下角, 需要满足
x1 <= x2
y1 >= y2
那么可以将横坐标从小到大排序, 纵坐标从大到小排序
内层循环从 i + 1 开始
那么从前往后遍历时 x1 <= x2 一定满足
可以用 mx 记录当前遍历的纵坐标的最大值
如果 mx 比 y2 小那么从 [i + 1, j - 1] 里的纵坐标都比 y2 小
所以当横坐标相同时只会遍历到相邻的 y
比如 [(1, 3), (1, 2), (1, 1)]
只有 (1, 3) 和 (1, 2) 以及 (1, 2) 和 (1, 1) 是合法的
而 y2 > y1 的情况不计算合法的矩形直接跳过
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfPairs(vector<vector<int>>& points) 
    {
        sort(points.begin(), points.end(), [](auto& a, auto& b) { return a.front() != b.front() ? a.front() < b.front() : a.back() > b.back(); });
        int result = 0, n = points.size();
        for (int i = 0; i < n; i++) 
        {
            for (int y1 = points[i][1], j = i + 1, mx = INT_MIN, y2 = 0; j < n; j++) 
            {
                if (y1 >= (y2 = points[j][1]) and y2 > mx) 
                {
                    mx = y2;
                    ++result;
                }
                if (y1 == mx) break;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfPairs(int[][] points) {
        Arrays.sort(points, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
        int result = 0, n = points.length;
        for (int i = 0; i < n; i++) {
            for (int y1 = points[i][1], j = i + 1, mx = Integer.MIN_VALUE, y2 = 0; j < n; j++) {
                if (y1 >= (y2 = points[j][1]) && y2 > mx) {
                    mx = y2;
                    ++result;
                }
                if (y1 == mx) break;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPairs(self, points: List[List[int]]) -> int:
        points.sort(key=lambda p: (p[0], -p[1]))
        result = 0
        for i, (_, y1) in enumerate(points):
            mx = -inf
            for _, y2 in points[i + 1:]:
                if y1 >= y2 > mx:
                    mx = y2
                    result += 1
                if mx == y1:
                    break
        return result
```
