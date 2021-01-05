# 1232 Check If It Is a Straight Line 缀点成线

__Description__:
You are given an array coordinates, coordinates[i] = [x, y], where [x, y] represents the coordinate of a point. Check if these points make a straight line in the XY plane.

__Example:__

Example 1:
![coordinates 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/untitled-diagram-2.jpg)
Input: coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
Output: true

Example 2:
![coordinates 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/untitled-diagram-1.jpg)
Input: coordinates = [[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]
Output: false

__Constraints:__

2 <= coordinates.length <= 1000
coordinates[i].length == 2
-10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4
coordinates contains no duplicate point.

__题目描述__:
在一个 XY 坐标系中有一些点，我们用数组 coordinates 来分别记录它们的坐标，其中 coordinates[i] = [x, y] 表示横坐标为 x、纵坐标为 y 的点。

请你来判断，这些点是否在该坐标系中属于同一条直线上，是则返回 true，否则请返回 false。

__示例 :__

示例 1：
![坐标系1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/untitled-diagram-2.jpg)
输入：coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
输出：true

示例 2：
![坐标系2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/untitled-diagram-1.jpg)
输入：coordinates = [[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]
输出：false

__提示：__

2 <= coordinates.length <= 1000
coordinates[i].length == 2
-10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4
coordinates 中不含重复的点

__思路__:

参考[LeetCode #1037 Valid Boomerang 有效的回旋镖](https://www.jianshu.com/p/9bef484a710a)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool checkStraightLine(vector<vector<int>>& coordinates) 
    {
        int x = coordinates[1][0] - coordinates[0][0], y = coordinates[1][1] - coordinates[0][1];
        for (int i = 1; i < coordinates.size(); i++) if ((coordinates[i][0] - coordinates[0][0]) * y != x * (coordinates[i][1] - coordinates[0][1])) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkStraightLine(int[][] coordinates) {
        int x = coordinates[1][0] - coordinates[0][0], y = coordinates[1][1] - coordinates[0][1];
        for (int i = 1; i < coordinates.length; i++) if ((coordinates[i][0] - coordinates[0][0]) * y != x * (coordinates[i][1] - coordinates[0][1])) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def checkStraightLine(self, coordinates: List[List[int]]) -> bool:
        return all((coordinates[1][1] - coordinates[0][1]) * (coordinates[i][0] - coordinates[0][0]) == (coordinates[i][1] - coordinates[0][1]) * (coordinates[1][0] - coordinates[0][0]) for i in range(2, len(coordinates)))
```
