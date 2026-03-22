# 1610 Maximum Number of Visible Points 可见点的最大数目

__Description:__

You are given an array `points`, an integer `angle`, and your `location`, where `location = [posx, posy]` and `points[i] = [xi, yi]` both denote __integral coordinates__ on the X-Y plane.

Initially, you are facing directly east from your position. You __cannot move__ from your position, but you can __rotate__. In other words, `posx` and `posy` cannot be changed. Your field of view in __degrees__ is represented by `angle`, determining how wide you can see from any given view direction. Let `d` be the amount in degrees that you rotate counterclockwise. Then, your field of view is the __inclusive__ range of angles `[d - angle/2, d + angle/2]`.

You can see some set of points if, for each point, the angle formed by the point, your position, and the immediate east direction from your position is in your field of view.

There can be multiple points at one coordinate. There may be points at your location, and you can always see these points regardless of your rotation. Points do not obstruct your vision to other points.

Return the maximum number of points you can see.

__Example:__

Example 1:

![1610-1](https://assets.leetcode.com/uploads/2020/09/30/89a07e9b-00ab-4967-976a-c723b2aa8656.png)

```text
Input: points = [[2,1],[2,2],[3,3]], angle = 90, location = [1,1]
Output: 3
Explanation: The shaded region represents your field of view. All points can be made visible in your field of view, including [3,3] even though [2,2] is in front and in the same line of sight.
```

Example 2:

```text
Input: points = [[2,1],[2,2],[3,4],[1,1]], angle = 90, location = [1,1]
Output: 4
Explanation: All points can be made visible in your field of view, including the one at your location.
```

Example 3:

![1610-2](https://assets.leetcode.com/uploads/2020/09/30/5010bfd3-86e6-465f-ac64-e9df941d2e49.png)

```text
Input: points = [[1,0],[2,1]], angle = 13, location = [1,1]
Output: 1
Explanation: You can only see one of the two points, as shown above.
```

__Constraints:__

- `1 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `location.length == 2`
- `0 <= angle < 360`
- `0 <= posx, posy, xi, yi <= 100`

__题目描述:__

给你一个点数组 `points` 和一个表示角度的整数 `angle` ，你的位置是 `location` ，其中 `location = [posx, posy]` 且 `points[i] = [xi, yi]` 都表示 X-Y 平面上的整数坐标。

最开始，你面向东方进行观测。你 __不能__ 进行移动改变位置，但可以通过 __自转__ 调整观测角度。换句话说， `posx` 和 `posy` 不能改变。你的视野范围的角度用 `angle` 表示， 这决定了你观测任意方向时可以多宽。设 `d` 为你逆时针自转旋转的度数，那么你的视野就是角度范围 `[d - angle/2, d + angle/2]` 所指示的那片区域。

对于每个点，如果由该点、你的位置以及从你的位置直接向东的方向形成的角度 位于你的视野中 ，那么你就可以看到它。

同一个坐标上可以有多个点。你所在的位置也可能存在一些点，但不管你的怎么旋转，总是可以看到这些点。同时，点不会阻碍你看到其他点。

返回你能看到的点的最大数目。

__示例:__

示例 1：

![1610-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/89a07e9b-00ab-4967-976a-c723b2aa8656.png)

```text
输入：points = [[2,1],[2,2],[3,3]], angle = 90, location = [1,1]
输出：3
解释：阴影区域代表你的视野。在你的视野中，所有的点都清晰可见，尽管 [2,2] 和 [3,3]在同一条直线上，你仍然可以看到 [3,3] 。
```

示例 2：

```text
输入：points = [[2,1],[2,2],[3,4],[1,1]], angle = 90, location = [1,1]
输出：4
解释：在你的视野中，所有的点都清晰可见，包括你所在位置的那个点。
```

示例 3：

![1610-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/5010bfd3-86e6-465f-ac64-e9df941d2e49.png)

```text
输入：points = [[1,0],[2,1]], angle = 13, location = [1,1]
输出：1
解释：如图所示，你只能看到两点之一。
```

__提示：__

- `1 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `location.length == 2`
- `0 <= angle < 360`
- `0 <= posx, posy, xi, yi <= 100`

__思路:__

```text
数学
先统计有多少个点与起始观察点重合, 这些点一定能被观测到
计算每个点相对于观察点的夹角, 用正切值表示
将极角列表排序, 然后将列表连成环, 添加所有极角再加 2 pi 的值
利用滑动窗口, 统计 angle 范围内的点即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int visiblePoints(vector<vector<int>>& points, int angle, vector<int>& location) 
    {
        int origin = 0;
        vector<double> polars;
        for (const auto& p: points) 
        {
            if (p == location) ++origin;
            else polars.emplace_back(atan2(p.back() - location.back(), p.front() - location.front()));
        }
        sort(polars.begin(), polars.end());
        int n = polars.size(), result = origin, right = 0;
        for (int i = 0; i < n; i++) polars.emplace_back(polars[i] + M_PI * 2);
        double degree = angle * M_PI / 180.0;
        for (int i = 0; i < n; i++) 
        {
            while (right < (n << 1) and polars[right] <= polars[i] + degree) ++right;
            result = max(result, origin + right - i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {
        int origin = 0;
        List<Double> polars = new ArrayList<>();
        for (List<Integer> p: points) {
            if (p.equals(location)) ++origin;
            else polars.add(Math.atan2(p.get(1) - location.get(1), p.get(0) - location.get(0)));
        }
        Collections.sort(polars);
        int n = polars.size(), result = origin, right = 0;
        for (int i = 0; i < n; i++) polars.add(polars.get(i) + Math.PI * 2);
        double degree = angle * Math.PI / 180.0;
        for (int i = 0; i < n; i++) {
            while (right < (n << 1) && polars.get(right) <= polars.get(i) + degree) ++right;
            result = Math.max(result, origin + right - i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def visiblePoints(self, points: List[List[int]], angle: int, location: List[int]) -> int:
        origin, polars = 0, deque()
        for p in points:
            if p == location:
                origin += 1
            else:
                polars.append(atan2(p[1] - location[1], p[0] - location[0]))
        polars = list(polars)
        polars.sort()
        n = len(polars)
        polars += [deg + 2 * pi for deg in polars]
        result, right, degree = origin, 0, angle * pi / 180
        for i in range(n):
            while right < (n << 1) and polars[right] <= polars[i] + degree:
                right += 1
            result = max(result, origin + right - i)
        return result
```
