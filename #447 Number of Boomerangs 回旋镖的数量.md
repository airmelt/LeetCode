# 447 Number of Boomerangs 回旋镖的数量

__Description__:
Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).

__Example:__

Input:
[[0,0],[1,0],[2,0]]

Output:
2

__Explanation:__
The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]

__题目描述__:
给定平面上 n 对不同的点，“回旋镖” 是由点表示的元组 (i, j, k) ，其中 i 和 j 之间的距离和 i 和 k 之间的距离相等（需要考虑元组的顺序）。

找到所有回旋镖的数量。你可以假设 n 最大为 500，所有点的坐标在闭区间 [-10000, 10000] 中。

__示例：__

输入:
[[0,0],[1,0],[2,0]]

输出:
2

__解释:__
两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]

__思路__:
题意是说, 求 3个点, 其中中点到两个端点的距离相等
注意到 distance(i, j) = distance(j, i), 可以简化计算
距离可以不用开方直接存储
求取距离相等的个数 n对应的解为 A(n, 2) = n(n - 1), 即有 n组相等的距离的点的数量为 n(n - 1)
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution {
public:
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int result = 0, s = points.size(), distance[s][s] = {0};
        for (int i = 0; i < s - 1; i++) for (int j = i + 1; j < s; j++) distance[i][j] = distance[j][i] = getDistance(points[i], points[j]);
        for (int i = 0; i < s; i++) {
            sort(distance[i], distance[i] + s);
            int temp = 1;
            for (int j = 1; j < s; j++) {
                if (distance[i][j] == distance[i][j - 1]) temp++;
                else {
                    result += temp * (temp - 1);
                    temp = 1;
                }
            }
            result += temp * (temp - 1);
        }
        return result;
    }
private:
    int getDistance(vector<int> pointX, vector<int> pointY) {
        return (pointX[0] - pointY[0]) * (pointX[0] - pointY[0]) + (pointX[1] - pointY[1]) * (pointX[1] - pointY[1]);
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        int result = 0, s = points.length;
        int[][] distance = new int[s][s];
        for (int i = 0; i < s - 1; i++) for (int j = i + 1; j < s; j++) distance[i][j] = distance[j][i] = getDistance(points[i], points[j]);
        for (int i = 0; i < s; i++) {
            Arrays.sort(distance[i]);
            int temp = 1;
            for (int j = 1; j < s; j++) {
                if (distance[i][j] == distance[i][j - 1]) temp++;
                else {
                    result += temp * (temp - 1);
                    temp = 1;
                }
            }
            result += temp * (temp - 1);
        }
        return result;
    }

    private int getDistance(int[] pointX, int[] pointY) {
        return (pointX[0] - pointY[0]) * (pointX[0] - pointY[0]) + (pointX[1] - pointY[1]) * (pointX[1] - pointY[1]);
    }
}

```

__Python__:

```Python
class Solution:
    def numberOfBoomerangs(self, points: List[List[int]]) -> int:
        result = 0
        for pointA in points:
            d = {}
            for pointB in points:
                x, y = (pointA[0] - pointB[0]), (pointA[1] - pointB[1])
                distance = x * x + y * y
                if distance in d:
                    d[distance] += 1
                else:
                    d[distance] = 1
            for i in d:
                if d[i] > 1:
                    result += d[i] * (d[i] - 1)
        return result
```
