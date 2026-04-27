# 2069 Walking Robot Simulation II 模拟行走机器人 II

__Description:__

A `width x height` grid is on an XY-plane with the __bottom-left__ cell at `(0, 0)` and the __top-right__ cell at `(width - 1, height - 1)`. The grid is aligned with the four cardinal directions ( `"North"`, `"East"`, `"South"`, and `"West"`). A robot is __initially__ at cell `(0, 0)` facing direction `"East"`.

The robot can be instructed to move for a specific number of steps. For each step, it does the following.

After the robot finishes moving the number of steps required, it stops and awaits the next instruction.

Implement the `Robot` class:

- `Robot(int width, int height)` Initializes the `width x height` grid with the robot at `(0, 0)` facing `"East"`.
- `void step(int num)` Instructs the robot to move forward `num` steps.
- `int[] getPos()` Returns the current cell the robot is at, as an array of length 2, `[x, y]`.
- `String getDir()` Returns the current direction of the robot, `"North"`, `"East"`, `"South"`, or `"West"`.

__Example:__

Example 1:

![2069-1](https://assets.leetcode.com/uploads/2021/10/09/example-1.png)

```text
Input
["Robot", "step", "step", "getPos", "getDir", "step", "step", "step", "getPos", "getDir"]
[[6, 3], [2], [2], [], [], [2], [1], [4], [], []]
Output
[null, null, null, [4, 0], "East", null, null, null, [1, 2], "West"]

Explanation
```

```Java
Robot robot = new Robot(6, 3); // Initialize the grid and the robot at (0, 0) facing East.
robot.step(2);  // It moves two steps East to (2, 0), and faces East.
robot.step(2);  // It moves two steps East to (4, 0), and faces East.
robot.getPos(); // return [4, 0]
robot.getDir(); // return "East"
robot.step(2);  // It moves one step East to (5, 0), and faces East.
                // Moving the next step East would be out of bounds, so it turns and faces North.
                // Then, it moves one step North to (5, 1), and faces North.
robot.step(1);  // It moves one step North to (5, 2), and faces North (not West).
robot.step(4);  // Moving the next step North would be out of bounds, so it turns and faces West.
                // Then, it moves four steps West to (1, 2), and faces West.
robot.getPos(); // return [1, 2]
robot.getDir(); // return "West"
```

__Constraints:__

- `2 <= width, height <= 100`
- `1 <= num <= 10 ^ 5`
- At most `10 ^ 4` calls __in total__ will be made to `step`, `getPos`, and `getDir`.

__题目描述:__

给你一个在 XY 平面上的 `width x height` 的网格图，__左下角__ 的格子为 `(0, 0)` ，__右上角__ 的格子为 `(width - 1, height - 1)` 。网格图中相邻格子为四个基本方向之一（ `"North"`， `"East"`， `"South"` 和 `"West"`）。一个机器人 __初始__ 在格子 `(0, 0)` ，方向为 `"East"` 。

机器人可以根据指令移动指定的 步数 。每一步，它可以执行以下操作。

如果机器人完成了指令要求的移动步数，它将停止移动并等待下一个指令。

请你实现 `Robot` 类:

- `Robot(int width, int height)` 初始化一个 `width x height` 的网格图，机器人初始在 `(0, 0)` ，方向朝 `"East"` 。
- `void step(int num)` 给机器人下达前进 `num` 步的指令。
- `int[] getPos()` 返回机器人当前所处的格子位置，用一个长度为 2 的数组 `[x, y]` 表示。
- `String getDir()` 返回当前机器人的朝向，为 `"North"` ， `"East"` ， `"South"` 或者 `"West"` 。

__示例:__

示例 1：

![2069-2](https://assets.leetcode.com/uploads/2021/10/09/example-1.png)

```text
输入：
["Robot", "step", "step", "getPos", "getDir", "step", "step", "step", "getPos", "getDir"]
[[6, 3], [2], [2], [], [], [2], [1], [4], [], []]
输出：
[null, null, null, [4, 0], "East", null, null, null, [1, 2], "West"]

解释：
```

```Java
Robot robot = new Robot(6, 3); // 初始化网格图，机器人在 (0, 0) ，朝东。
robot.step(2);  // 机器人朝东移动 2 步，到达 (2, 0) ，并朝东。
robot.step(2);  // 机器人朝东移动 2 步，到达 (4, 0) ，并朝东。
robot.getPos(); // 返回 [4, 0]
robot.getDir(); // 返回 "East"
robot.step(2);  // 朝东移动 1 步到达 (5, 0) ，并朝东。
                // 下一步继续往东移动将出界，所以逆时针转变方向朝北。
                // 然后，往北移动 1 步到达 (5, 1) ，并朝北。
robot.step(1);  // 朝北移动 1 步到达 (5, 2) ，并朝 北 （不是朝西）。
robot.step(4);  // 下一步继续往北移动将出界，所以逆时针转变方向朝西。
                // 然后，移动 4 步到 (1, 2) ，并朝西。
robot.getPos(); // 返回 [1, 2]
robot.getDir(); // 返回 "West"
```

__提示：__

- `2 <= width, height <= 100`
- `1 <= num <= 10 ^ 5`
- `step` ， `getPos` 和 `getDir` __总共__ 调用次数不超过 `10 ^ 4` 次。

__思路:__

```text
模拟
设当前的位置为 cur
每次走 (width + height - 2) * 2 就走完一圈
根据 cur 和 width, height 的大小关系可以判断位置及坐标
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Robot
{
public:
    Robot(int width, int height) 
    {
        cur = 0;
        w = width;
        h = height;
    }
    
    void step(int num) 
    {
        cur = (cur + num - 1) % ((w + h - 2) << 1) + 1;
    }
    
    vector<int> getPos() 
    {
        return cur < w ? vector<int>{cur, 0} : (cur < w + h - 1 ? vector<int>{w - 1, cur - w + 1} : (cur < (w << 1) + h - 2) ? vector<int>{(w << 1) + h - 3 - cur, h - 1} : vector<int>{0, ((w + h - 2) << 1) - cur});
    }
    
    string getDir() 
    {
        return cur < w ? "East" : (cur < w + h - 1 ? "North" : (cur < (w << 1) + h - 2) ? "West" : "South");
    }
private:
    int w, h;
    int cur;
};

/**
 * Your Robot object will be instantiated and called as such:
 * Robot* obj = new Robot(width, height);
 * obj->step(num);
 * vector<int> param_2 = obj->getPos();
 * string param_3 = obj->getDir();
 */
```

__Java__:

```Java
class Robot {
    int w, h;
    int cur;

    public Robot(int width, int height) {
        cur = 0;
        w = width;
        h = height;
    }
    
    public void step(int num) {
        cur = (cur + num - 1) % ((w + h - 2) << 1) + 1;
    }
    
    public int[] getPos() {
        return cur < w ? new int[]{cur, 0} : (cur < w + h - 1 ? new int[]{w - 1, cur - w + 1} : (cur < (w << 1) + h - 2) ? new int[]{(w << 1) + h - 3 - cur, h - 1} : new int[]{0, ((w + h - 2) << 1) - cur});
    }
    
    public String getDir() {
        return cur < w ? "East" : (cur < w + h - 1 ? "North" : (cur < (w << 1) + h - 2) ? "West" : "South");
    }
}

/**
 * Your Robot object will be instantiated and called as such:
 * Robot obj = new Robot(width, height);
 * obj.step(num);
 * int[] param_2 = obj.getPos();
 * String param_3 = obj.getDir();
 */
```

__Python__:

```Python
class Robot:

    def __init__(self, width: int, height: int):
        self.width = width
        self.height = height
        self.cur = 0


    def step(self, num: int) -> None:
        self.cur = (self.cur + num - 1) % ((self.width + self.height - 2) << 1) + 1


    def getPos(self) -> List[int]:
        return [self.cur, 0] if self.cur < self.width else [self.width - 1, self.cur - self.width + 1] if self.cur < self.width + self.height - 1 else [(self.width << 1) + self.height - 3 - self.cur, self.height - 1] if self.cur < (self.width << 1) + self.height - 2 else [0, ((self.width + self.height - 2) << 1) - self.cur]


    def getDir(self) -> str:
        return 'East' if self.cur < self.width else 'North' if self.cur < self.width + self.height - 1 else 'West' if self.cur < (self.width << 1) + self.height - 2 else 'South'



# Your Robot object will be instantiated and called as such:
# obj = Robot(width, height)
# obj.step(num)
# param_2 = obj.getPos()
# param_3 = obj.getDir()
```
