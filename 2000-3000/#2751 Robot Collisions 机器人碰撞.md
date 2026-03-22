# 2751 Robot Collisions 机器人碰撞

__Description:__

There are `n` __1-indexed__ robots, each having a position on a line, health, and movement direction.

You are given __0-indexed__ integer arrays `positions`, `healths`, and a string `directions` ( `directions[i]` is either __'L'__ for __left__ or __'R'__ for __right__). All integers in `positions` are __unique__.

All robots start moving on the line simultaneously at the same speed in their given directions. If two robots ever share the same position while moving, they will collide.

If two robots collide, the robot with lower health is removed from the line, and the health of the other robot decreases by one. The surviving robot continues in the same direction it was going. If both robots have the same health, they are both removed from the line.

Your task is to determine the health of the robots that survive the collisions, in the same order that the robots were given, i.e. final health of robot 1 (if survived), final health of robot 2 (if survived), and so on. If there are no survivors, return an empty array.

Return an array containing the health of the remaining robots (in the order they were given in the input), after no further collisions can occur.

Note: The positions may be unsorted.

__Example:__

Example 1:

![2751-1](https://assets.leetcode.com/uploads/2023/05/15/image-20230516011718-12.png)

```text
Input: positions = [5,4,3,2,1], healths = [2,17,9,15,10], directions = "RRRRR"
Output: [2,17,9,15,10]
Explanation: No collision occurs in this example, since all robots are moving in the same direction. So, the health of the robots in order from the first robot is returned, [2, 17, 9, 15, 10].
```

Example 2:

![2751-2](https://assets.leetcode.com/uploads/2023/05/15/image-20230516004433-7.png)

```text
Input: positions = [3,5,2,6], healths = [10,10,15,12], directions = "RLRL"
Output: [14]
Explanation: There are 2 collisions in this example. Firstly, robot 1 and robot 2 will collide, and since both have the same health, they will be removed from the line. Next, robot 3 and robot 4 will collide and since robot 4's health is smaller, it gets removed, and robot 3's health becomes 15 - 1 = 14. Only robot 3 remains, so we return [14].
```

Example 3:

![2751-3](https://assets.leetcode.com/uploads/2023/05/15/image-20230516005114-9.png)

```text
Input: positions = [1,2,5,6], healths = [10,10,11,11], directions = "RLRL"
Output: []
Explanation: Robot 1 and robot 2 will collide and since both have the same health, they are both removed. Robot 3 and 4 will collide and since both have the same health, they are both removed. So, we return an empty array, [].
```

__Constraints:__

- `1 <= positions.length == healths.length == directions.length == n <= 10 ^ 5`
- `1 <= positions[i], healths[i] <= 10 ^ 9`
- `directions[i] == 'L'` or `directions[i] == 'R'`
- All values in `positions` are distinct

__题目描述:__

现有 `n` 个机器人，编号从 __1__ 开始，每个机器人包含在路线上的位置、健康度和移动方向。

给你下标从 __0__ 开始的两个整数数组 `positions`、 `healths` 和一个字符串 `directions`（ `directions[i]` 为 __'L'__ 表示 __向左__ 或 __'R'__ 表示 __向右__）。 `positions` 中的所有整数 __互不相同__ 。

所有机器人以 相同速度 同时 沿给定方向在路线上移动。如果两个机器人移动到相同位置，则会发生 碰撞 。

如果两个机器人发生碰撞，则将 健康度较低 的机器人从路线中 移除 ，并且另一个机器人的健康度 减少 1 。幸存下来的机器人将会继续沿着与之前 相同 的方向前进。如果两个机器人的健康度相同，则将二者都从路线中移除。

请你确定全部碰撞后幸存下的所有机器人的 健康度 ，并按照原来机器人编号的顺序排列。即机器人 1 （如果幸存）的最终健康度，机器人 2 （如果幸存）的最终健康度等。 如果不存在幸存的机器人，则返回空数组。

在不再发生任何碰撞后，请你以数组形式，返回所有剩余机器人的健康度（按机器人输入中的编号顺序）。

__注意:__ 位置  `positions` 可能是乱序的。

__示例:__

示例 1：

![2751-4](https://assets.leetcode.com/uploads/2023/05/15/image-20230516011718-12.png)

```text
输入：positions = [5,4,3,2,1], healths = [2,17,9,15,10], directions = "RRRRR"
输出：[2,17,9,15,10]
解释：在本例中不存在碰撞，因为所有机器人向同一方向移动。所以，从第一个机器人开始依序返回健康度，[2, 17, 9, 15, 10] 。
```

示例 2：

![2751-5](https://assets.leetcode.com/uploads/2023/05/15/image-20230516004433-7.png)

```text
输入：positions = [3,5,2,6], healths = [10,10,15,12], directions = "RLRL"
输出：[14]
解释：本例中发生 2 次碰撞。首先，机器人 1 和机器人 2 将会碰撞，因为二者健康度相同，二者都将被从路线中移除。接下来，机器人 3 和机器人 4 将会发生碰撞，由于机器人 4 的健康度更小，则它会被移除，而机器人 3 的健康度变为 15 - 1 = 14 。仅剩机器人 3 ，所以返回 [14] 。
```

示例 3：

![2751-6](https://assets.leetcode.com/uploads/2023/05/15/image-20230516005114-9.png)

```text
输入：positions = [1,2,5,6], healths = [10,10,11,11], directions = "RLRL"
输出：[]
解释：机器人 1 和机器人 2 将会碰撞，因为二者健康度相同，二者都将被从路线中移除。机器人 3 和机器人 4 将会碰撞，因为二者健康度相同，二者都将被从路线中移除。所以返回空数组 [] 。
```

__提示：__

- `1 <= positions.length == healths.length == directions.length == n <= 10 ^ 5`
- `1 <= positions[i], healths[i] <= 10 ^ 9`
- `directions[i] == 'L'` 或 `directions[i] == 'R'`
- `positions` 中的所有值互不相同

__思路:__

```text
模拟
使用栈进行模拟
栈中存放向右移动的机器人
先将机器人按照位置进行排序
如果机器人向右移动, 直接放入栈中, 并遍历下一个机器人
如果机器人向左移动
此时如果栈中没有向右移动的机器人, 那么不会发生碰撞, 可以直接将机器人加入结果中
如果栈中有机器人
分为三种情况讨论
如果栈顶的机器人健康度更大, 那么当前机器人被摧毁, 栈顶机器人健康度减 1, 退出循环
如果两者健康度相等, 那么两个机器人都被摧毁, 弹出栈顶
如果当前遍历的机器人健康度更大, 当前机器人健康度减 1, 可以继续碰撞
也可以直接修改 healths, 碰撞之后要么健康度减 1 要么归零
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要开销在排序上
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> survivedRobotsHealths(vector<int>& positions, vector<int>& healths, string directions) 
    {
        int n = healths.size(), cur = 0, idx[n];
        iota(idx, idx + n, 0);
        sort(idx, idx + n, [&](const auto& a, const auto& b) { return positions[a] < positions[b]; });
        stack<int> right;
        for (const auto& i : idx)
        {
            if (directions[i] == 'R') 
            {
                right.push(i);
                continue;
            }
            while (!right.empty())
            {
                if (healths[cur = right.top()] > healths[i])
                {
                    --healths[cur];
                    healths[i] = 0;
                    break;
                }
                if (healths[cur] == healths[i])
                {
                    healths[cur] = healths[i] = 0;
                    right.pop();
                    break;
                }
                healths[cur] = 0;
                right.pop();
                --healths[i];
            }
        }
        healths.erase(remove(healths.begin(), healths.end(), 0), healths.end());
        return healths;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> survivedRobotsHealths(int[] positions, int[] healths, String directions) {
        int n = healths.length, cur = 0;
        var idx = new Integer[n];
        for (int i = 0; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (a, b) -> positions[a] - positions[b]);
        var right = new ArrayDeque<Integer>();
        for (int i : idx) {
            if (directions.charAt(i) == 'R') {
                right.push(i);
                continue;
            }
            while (!right.isEmpty()) {
                if (healths[cur = right.peek()] >= healths[i]) {
                    healths[cur] = healths[cur] > healths[i] ? healths[cur] - 1 : healths[cur] - healths[right.pop()];
                    healths[i] = 0;
                    break;
                }
                healths[right.pop()] = 0;
                --healths[i];
            }
        }
        var result = new ArrayList<Integer>();
        for (int h : healths) if (h > 0) result.add(h);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def survivedRobotsHealths(self, positions: List[int], healths: List[int], directions: str) -> List[int]:
        robots, n, left, right = sorted([(p, h, d, i) for i, (p, h, d) in enumerate(zip(positions, healths, directions))]), len(healths), [], []
        for _, h, d, i in robots:
            if d == 'R':
                right.append([h, i])
                continue
            while right:
                if (cur := right[-1])[0] > h:
                    cur[0] -= 1
                    break
                if cur[0] == h:
                    right.pop()
                    break
                h -= 1
                right.pop()
            else:
                left.append([h, i])
        return [h for h, _ in sorted(left + right, key=lambda r: r[1])]
```
