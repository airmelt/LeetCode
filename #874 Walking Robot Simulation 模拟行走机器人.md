__Description__:
A robot on an infinite grid starts at point (0, 0) and faces north.  The robot can receive one of three possible types of commands:

-2: turn left 90 degrees
-1: turn right 90 degrees
1 <= x <= 9: move forward x units
Some of the grid squares are obstacles. 

The i-th obstacle is at grid point (obstacles[i][0], obstacles[i][1])

If the robot would try to move onto them, the robot stays on the previous grid square instead (but still continues following the rest of the route.)

Return the square of the maximum Euclidean distance that the robot will be from the origin.

__Example:__
Example 1:

Input: commands = [4,-1,3], obstacles = []
Output: 25
Explanation: robot will go to (3, 4)

Example 2:

Input: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
Output: 65
Explanation: robot will be stuck at (1, 4) before turning left and going to (1, 8)
 
__Note:__

0 <= commands.length <= 10000
0 <= obstacles.length <= 10000
-30000 <= obstacle[i][0] <= 30000
-30000 <= obstacle[i][1] <= 30000
The answer is guaranteed to be less than 2 ^ 31.

__题目描述__:
机器人在一个无限大小的网格上行走，从点 (0, 0) 处开始出发，面向北方。该机器人可以接收以下三种类型的命令：

-2：向左转 90 度
-1：向右转 90 度
1 <= x <= 9：向前移动 x 个单位长度
在网格上有一些格子被视为障碍物。

第 i 个障碍物位于网格点  (obstacles[i][0], obstacles[i][1])

如果机器人试图走到障碍物上方，那么它将停留在障碍物的前一个网格方块上，但仍然可以继续该路线的其余部分。

返回从原点到机器人的最大欧式距离的平方。

__示例 :__
示例 1：

输入: commands = [4,-1,3], obstacles = []
输出: 25
解释: 机器人将会到达 (3, 4)

示例 2：

输入: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
输出: 65
解释: 机器人在左转走到 (1, 8) 之前将被困在 (1, 4) 处
 
__提示：__

0 <= commands.length <= 10000
0 <= obstacles.length <= 10000
-30000 <= obstacle[i][0] <= 30000
-30000 <= obstacle[i][1] <= 30000
答案保证小于 2 ^ 31

__思路__:
- 首先这个题目描述的就有点问题, 返回的是在整个运动过程中, 机器人与原点的最大的欧式距离的平方
- 机器人会在障碍前停留直到转向
- 机器人的移动只会是 x或者 y方向及反方向, 所以分成两个坐标考虑即可
- 判断是否有障碍物需要把障碍物的 list转化为 set, 把查找时间减少到 O(1), 否则会超时

时间复杂度O(n + m), 空间复杂度O(m), 其中 n为 commands的大小, m为 obstacles的大小

__代码__:
__C++__:
```C++
class Solution {
public:
    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        int dx[4] = {0, 1, 0, -1};
        int dy[4] = {1, 0, -1, 0};
        int x = 0, y = 0, direction = 0, result = 0;
        unordered_set<string> obstacleSet;
        for (vector<int> obstacle: obstacles) obstacleSet.insert(to_string(obstacle[0]) + ", " + to_string(obstacle[1]));
        for (int conmmand: commands) 
        {
            if (conmmand == -2) direction = (direction + 3) % 4;
            else if (conmmand == -1) direction = (direction + 1) % 4;
            else 
            {
                for (int i = 0; i < conmmand; i++) 
                {
                    if (obstacleSet.find(to_string(x + dx[direction]) + ", " + to_string(y + dy[direction])) == obstacleSet.end()) 
                    {
                        x += dx[direction];
                        y += dy[direction];
                        result = max(result, x * x + y * y);
                    }
                }
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        int[] dx = new int[]{0, 1, 0, -1};
        int[] dy = new int[]{1, 0, -1, 0};
        int x = 0, y = 0, direction = 0, result = 0;
        Set<String> obstacleSet = new HashSet();
        for (int[] obstacle: obstacles) obstacleSet.add(String.valueOf(obstacle[0]) + ", " + String.valueOf(obstacle[1]));
        for (int command: commands) {
            if (command == -2) direction = (direction + 3) % 4;
            else if (command == -1) direction = (direction + 1) % 4;
            else {
                for (int i = 0; i < command; i++) {
                    if (!obstacleSet.contains(String.valueOf(x + dx[direction]) + ", " + String.valueOf(y + dy[direction]))) {
                        x += dx[direction];
                        y += dy[direction];
                        result = Math.max(result, x * x + y * y);
                    }
                }
            }
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def robotSim(self, commands: List[int], obstacles: List[List[int]]) -> int:
        obstacles, dx, dy = set(map(tuple, obstacles)), [0, 1, 0, -1], [1, 0, -1, 0]
        x = y = direction = 0
        result = 0
        for command in commands:
            if command == -2:
                direction = (direction + 3) % 4
            elif command == -1:
                direction = (direction + 1) % 4
            else:
                for _ in range(command):
                    if (x + dx[direction], y + dy[direction]) not in obstacles:
                        x += dx[direction]
                        y += dy[direction]
                        result = max(result, x * x + y * y)
        return result
```