# 1620 Coordinate With Maximum Network Quality 网络信号最好的坐标

__Description:__

You are given an array of network towers `towers`, where `towers[i] = [xi, yi, qi]` denotes the `i ^ th` network tower with location `(xi, yi)` and quality factor `qi`. All the coordinates are __integral coordinates__ on the X-Y plane, and the distance between the two coordinates is the __Euclidean distance__.

You are also given an integer `radius` where a tower is __reachable__ if the distance is __less than or equal to__ `radius`. Outside that distance, the signal becomes garbled, and the tower is __not reachable__.

The signal quality of the `i ^ th` tower at a coordinate `(x, y)` is calculated with the formula `⌊qi / (1 + d)⌋`, where `d` is the distance between the tower and the coordinate. The __network quality__ at a coordinate is the sum of the signal qualities from all the __reachable__ towers.

Return _the array_ `[cx, cy]` _representing the __integral__ coordinate_ `(cx, cy)` _where the __network quality__ is maximum. If there are multiple coordinates with the same __network quality__, return the lexicographically minimum __non-negative__ coordinate._

Note:

- A coordinate `(x1, y1)` is lexicographically smaller than `(x2, y2)` if either:
  - `x1 < x2`, or
  - `x1 == x2` and `y1 < y2`.
- `x1 < x2`, or
- `x1 == x2` and `y1 < y2`.
- `⌊val⌋` is the greatest integer less than or equal to `val` (the floor function).

- `x1 < x2`, or
- `x1 == x2` and `y1 < y2`.

__Example:__

Example 1:

![1620-1](https://assets.leetcode.com/uploads/2020/09/22/untitled-diagram.png)

```text
Input: towers = [[1,2,5],[2,1,7],[3,1,9]], radius = 2
Output: [2,1]
Explanation: At coordinate (2, 1) the total quality is 13.
- Quality of 7 from (2, 1) results in ⌊7 / (1 + sqrt(0)⌋ = ⌊7⌋ = 7
- Quality of 5 from (1, 2) results in ⌊5 / (1 + sqrt(2)⌋ = ⌊2.07⌋ = 2
- Quality of 9 from (3, 1) results in ⌊9 / (1 + sqrt(1)⌋ = ⌊4.5⌋ = 4
No other coordinate has a higher network quality.
```

Example 2:

```text
Input: towers = [[23,11,21]], radius = 9
Output: [23,11]
Explanation: Since there is only one tower, the network quality is highest right at the tower's location.
```

Example 3:

```text
Input: towers = [[1,2,13],[2,1,7],[0,1,9]], radius = 2
Output: [1,2]
Explanation: Coordinate (1, 2) has the highest network quality.
```

__Constraints:__

- `1 <= towers.length <= 50`
- `towers[i].length == 3`
- `0 <= xi, yi, qi <= 50`
- `1 <= radius <= 50`

__题目描述:__

给你一个数组 `towers` 和一个整数 `radius` 。

数组  `towers` 中包含一些网络信号塔，其中 `towers[i] = [xi, yi, qi]` 表示第 `i` 个网络信号塔的坐标是 `(xi, yi)` 且信号强度参数为 `qi` 。所有坐标都是在 X-Y 坐标系内的 __整数__ 坐标。两个坐标之间的距离用 __欧几里得距离__ 计算。

整数 `radius` 表示一个塔 __能到达__ 的 __最远距离__ 。如果一个坐标跟塔的距离在 `radius` 以内，那么该塔的信号可以到达该坐标。在这个范围以外信号会很微弱，所以 `radius` 以外的距离该塔是 __不能到达的__ 。

如果第 `i` 个塔能到达 `(x, y)` ，那么该塔在此处的信号为 `⌊qi / (1 + d)⌋` ，其中 `d` 是塔跟此坐标的距离。一个坐标的 _信号强度_ 是所有 __能到达__ 该坐标的塔的信号强度之和。

请你返回数组 `[cx, cy]` ，表示 __信号强度__ 最大的 __整数__ 坐标点 `(cx, cy)` 。如果有多个坐标网络信号一样大，请你返回字典序最小的 __非负__ 坐标。

注意：

- 坐标 `(x1, y1)` 字典序比另一个坐标 `(x2, y2)` 小，需满足以下条件之一:
  - 要么 `x1 < x2` ，
  - 要么 `x1 == x2` 且 `y1 < y2` 。
- 要么 `x1 < x2` ，
- 要么 `x1 == x2` 且 `y1 < y2` 。
- `⌊val⌋` 表示小于等于 `val` 的最大整数（向下取整函数）。

- 要么 `x1 < x2` ，
- 要么 `x1 == x2` 且 `y1 < y2` 。

__示例:__

示例 1：

![1620-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/17/untitled-diagram.png)

```text
输入：towers = [[1,2,5],[2,1,7],[3,1,9]], radius = 2
输出：[2,1]
解释：
坐标 (2, 1) 信号强度之和为 13
- 塔 (2, 1) 强度参数为 7 ，在该点强度为 ⌊7 / (1 + sqrt(0)⌋ = ⌊7⌋ = 7
- 塔 (1, 2) 强度参数为 5 ，在该点强度为 ⌊5 / (1 + sqrt(2)⌋ = ⌊2.07⌋ = 2
- 塔 (3, 1) 强度参数为 9 ，在该点强度为 ⌊9 / (1 + sqrt(1)⌋ = ⌊4.5⌋ = 4
没有别的坐标有更大的信号强度。
```

示例 2：

```text
输入：towers = [[23,11,21]], radius = 9
输出：[23,11]
解释：由于仅存在一座信号塔，所以塔的位置信号强度最大。
```

示例 3：

```text
输入：towers = [[1,2,13],[2,1,7],[0,1,9]], radius = 2
输出：[1,2]
解释：坐标 (1, 2) 的信号强度最大。
```

__提示：__

- `1 <= towers.length <= 50`
- `towers[i].length == 3`
- `0 <= xi, yi, qi <= 50`
- `1 <= radius <= 50`

__思路:__

```text
暴力
遍历 (0, 0) ~ (50, 50) 内的所有点
计算每个点的信号强度
取最大的信号强度的点
为了不损失精度, 比较距离的时先用距离平方的平方比较再计算信号大小
时间复杂度为 O(C ^ 2N), 空间复杂度为 O(1), 其中 C = 50
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> bestCoordinate(vector<vector<int>>& towers, int radius) 
    {
        int m = 0, cx = 0, cy = 0;
        for (int i = 0; i <= 50; i++) 
        {
            for (int j = 0; j <= 50; j++) 
            {
                int quality = 0;
                for (const auto& tower : towers) 
                {
                    int x = tower[0], y = tower[1], q = tower[2], d = (x - i) * (x - i) + (y - j) * (y - j);
                    if (d <= radius * radius) quality += (int)(q / (1 + sqrt(d)));
                }
                if (m < quality) 
                {
                    m = quality;
                    cx = i;
                    cy = j;
                }
            }
        }
        return vector<int>{cx, cy};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] bestCoordinate(int[][] towers, int radius) {
        int m = 0, cx = 0, cy = 0;
        for (int i = 0; i <= 50; i++) {
            for (int j = 0; j <= 50; j++) {
                int quality = 0;
                for (int[] tower : towers) {
                    int x = tower[0], y = tower[1], q = tower[2], d = (x - i) * (x - i) + (y - j) * (y - j);
                    if (d <= radius * radius) quality += (int)(q / (1 + Math.sqrt(d)));
                }
                if (m < quality) {
                    m = quality;
                    cx = i;
                    cy = j;
                }
            }
        }
        return new int[]{cx, cy};
    }
}
```

__Python__:

```Python
class Solution:
    def bestCoordinate(self, towers: List[List[int]], radius: int) -> List[int]:
        cx = cy = max_quality = 0
        for x in range(51):
            for y in range(51):
                if (quality := sum(int(q / (1 + d ** 0.5)) for tx, ty, q in towers if (d := (x - tx) ** 2 + (y - ty) ** 2) <= radius ** 2)) > max_quality:
                    cx, cy, max_quality = x, y, quality
        return [cx, cy]
```
