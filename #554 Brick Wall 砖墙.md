# 554 Brick Wall 砖墙

__Description__:
There is a brick wall in front of you. The wall is rectangular and has several rows of bricks. The bricks have the same height but different width. You want to draw a vertical line from the top to the bottom and cross the least bricks.

The brick wall is represented by a list of rows. Each row is a list of integers representing the width of each brick in this row from left to right.

If your line go through the edge of a brick, then the brick is not considered as crossed. You need to find out how to draw the line to cross the least bricks and return the number of crossed bricks.

You cannot draw a line just along one of the two vertical edges of the wall, in which case the line will obviously cross no bricks.

__Example:__

Input: [[1,2,2,1],
        [3,1,2],
        [1,3,2],
        [2,4],
        [3,1,2],
        [1,3,1,1]]

Output: 2

Explanation:

![brick_wall](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/brick_wall.png)

__Note:__

The width sum of bricks in different rows are the same and won't exceed INT_MAX.
The number of bricks in each row is in range [1,10,000]. The height of wall is in range [1,10,000]. Total number of bricks of the wall won't exceed 20,000.

__题目描述__:
你的面前有一堵矩形的、由多行砖块组成的砖墙。 这些砖块高度相同但是宽度不同。你现在要画一条自顶向下的、穿过最少砖块的垂线。

砖墙由行的列表表示。 每一行都是一个代表从左至右每块砖的宽度的整数列表。

如果你画的线只是从砖块的边缘经过，就不算穿过这块砖。你需要找出怎样画才能使这条线穿过的砖块数量最少，并且返回穿过的砖块数量。

你不能沿着墙的两个垂直边缘之一画线，这样显然是没有穿过一块砖的。

__示例 :__

输入: [[1,2,2,1],
      [3,1,2],
      [1,3,2],
      [2,4],
      [3,1,2],
      [1,3,1,1]]

输出: 2

解释:

![砖墙](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/brick_wall.png)

__提示:__

每一行砖块的宽度之和应该相等，并且不能超过 INT_MAX。
每一行砖块的数量在 [1,10,000] 范围内， 墙的高度在 [1,10,000] 范围内， 总的砖块数量不超过 20,000。

__思路__:

前缀和➕ 哈希表
记录每一行的前缀和
输出 wall.size() - max(m.values())
时间复杂度 O(mn), 空间复杂度 O(n), m 为 wall 的长度, n 为 wall 的每一块砖的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int leastBricks(vector<vector<int>>& wall) 
    {
        map<int, int> m;
        for (auto &line : wall) 
        {
            int s = 0;
            for (int i = 0; i < line.size() - 1; i++) ++m[s += line[i]];
        }
        int result = 0;
        for (auto &v : m) result = max(result, v.second);
        return wall.size() - result;
    }
};
```

__Java__:

```Java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {
        Map<Integer, Integer> map = new HashMap<>();
        for (List<Integer> line : wall) {
            int s = 0;
            for (int i = 0; i < line.size() - 1; i++) {
                s += line.get(i);
                map.put(s, map.getOrDefault(s, 0) + 1);
            }
        }
        int result = 0;
        for (int v : map.values()) result = Math.max(result, v);
        return wall.size() - result;
    }
}
```

__Python__:

```Python
class Solution:
    def leastBricks(self, wall: List[List[int]]) -> int:
        d = defaultdict(lambda : len(wall))
        for line in wall:
            pos = 0
            for brick in line[:-1]:
                pos += brick
                d[pos] -= 1
        return min(d.values()) if d else len(wall)
```
