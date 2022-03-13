# 1041 Robot Bounded In Circle 困于环中的机器人

__Description__:
On an infinite plane, a robot initially stands at (0, 0) and faces north. Note that:

The north direction is the positive direction of the y-axis.
The south direction is the negative direction of the y-axis.
The east direction is the positive direction of the x-axis.
The west direction is the negative direction of the x-axis.
The robot can receive one of three instructions:

"G": go straight 1 unit.
"L": turn 90 degrees to the left (i.e., anti-clockwise direction).
"R": turn 90 degrees to the right (i.e., clockwise direction).
The robot performs the instructions given in order, and repeats them forever.

Return true if and only if there exists a circle in the plane such that the robot never leaves the circle.

__Example:__

Example 1:

Input: instructions = "GGLLGG"
Output: true
Explanation: The robot is initially at (0, 0) facing the north direction.
"G": move one step. Position: (0, 1). Direction: North.
"G": move one step. Position: (0, 2). Direction: North.
"L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: West.
"L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: South.
"G": move one step. Position: (0, 1). Direction: South.
"G": move one step. Position: (0, 0). Direction: South.
Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (0, 2) --> (0, 1) --> (0, 0).
Based on that, we return true.

Example 2:

Input: instructions = "GG"
Output: false
Explanation: The robot is initially at (0, 0) facing the north direction.
"G": move one step. Position: (0, 1). Direction: North.
"G": move one step. Position: (0, 2). Direction: North.
Repeating the instructions, keeps advancing in the north direction and does not go into cycles.
Based on that, we return false.

Example 3:

Input: instructions = "GL"
Output: true
Explanation: The robot is initially at (0, 0) facing the north direction.
"G": move one step. Position: (0, 1). Direction: North.
"L": turn 90 degrees anti-clockwise. Position: (0, 1). Direction: West.
"G": move one step. Position: (-1, 1). Direction: West.
"L": turn 90 degrees anti-clockwise. Position: (-1, 1). Direction: South.
"G": move one step. Position: (-1, 0). Direction: South.
"L": turn 90 degrees anti-clockwise. Position: (-1, 0). Direction: East.
"G": move one step. Position: (0, 0). Direction: East.
"L": turn 90 degrees anti-clockwise. Position: (0, 0). Direction: North.
Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (-1, 1) --> (-1, 0) --> (0, 0).
Based on that, we return true.

__Constraints:__

1 <= instructions.length <= 100
instructions[i] is 'G', 'L' or, 'R'.

__题目描述__:
在无限的平面上，机器人最初位于 (0, 0) 处，面朝北方。机器人可以接受下列三条指令之一：

"G"：直走 1 个单位
"L"：左转 90 度
"R"：右转 90 度
机器人按顺序执行指令 instructions，并一直重复它们。

只有在平面中存在环使得机器人永远无法离开时，返回 true。否则，返回 false。

__示例 :__

示例 1：

输入："GGLLGG"
输出：true
解释：
机器人从 (0,0) 移动到 (0,2)，转 180 度，然后回到 (0,0)。
重复这些指令，机器人将保持在以原点为中心，2 为半径的环中进行移动。

示例 2：

输入："GG"
输出：false
解释：
机器人无限向北移动。

示例 3：

输入："GL"
输出：true
解释：
机器人按 (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ... 进行移动。

__提示:__

1 <= instructions.length <= 100
instructions[i] 在 {'G', 'L', 'R'} 中

__思路__:

模拟
循环 4 次 instructions 记录移动的方向和位置
可以使用复数的方式记录, 也可以用 4 个 int 记录
开始时方向指向 0
如果左转, 方向变为 (direction + 3) % 4
如果右转, 方向变为 (direction + 1) % 4
否则, 当前位置加上 direction
最后退出循环时查看是否回到原点
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isRobotBounded(string instructions)
    {
        int direction = 0, i = 0;
        vector<int> pos(4);
        while (i < 4) 
        {
            for (const auto& c : instructions) 
            {
                switch(c) 
                {
                    case 'G':
                        ++pos[direction];
                        break;
                    case 'L':
                        direction = (direction + 3) % 4;
                        break;
                    case 'R':
                        direction = (direction + 1) % 4;
                        break;
                    default:
                        return false;
                }
            }
            ++i;
        }
        return pos[0] == pos[2] and pos[1] == pos[3];
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isRobotBounded(String instructions) {
        int direction = 0, pos[] = new int[4], i = 0;
        while (i < 4) {
            for (char c : instructions.toCharArray()) {
                switch(c) {
                    case 'G':
                        ++pos[direction];
                        break;
                    case 'L':
                        direction = (direction + 3) % 4;
                        break;
                    case 'R':
                        direction = (direction + 1) % 4;
                        break;
                    default:
                        return false;
                }
            }
            ++i;
        }
        return pos[0] == pos[2] && pos[1] == pos[3];
    }
}
```

__Python__:

```Python
class Solution:
    def isRobotBounded(self, instructions: str) -> bool:
        step, direction, pos = [1j, 1, -1j, -1], 0, 0j
        for instruction in instructions * 4:
            if instruction == 'G':
                pos += step[direction]
            elif instruction == 'L':
                direction = (direction - 1) % 4
            elif instruction == 'R':
                direction = (direction + 1) % 4
        return pos == 0j
```
