# 2211 Count Collisions on a Road 统计道路上的碰撞次数

__Description:__

There are `n` cars on an infinitely long road. The cars are numbered from `0` to `n - 1` from left to right and each car is present at a __unique__ point.

You are given a __0-indexed__ string `directions` of length `n`. `directions[i]` can be either `'L'`, `'R'`, or `'S'` denoting whether the `i ^ th` car is moving towards the __left__, towards the __right__, or __staying__ at its current point respectively. Each moving car has the __same speed__.

The number of collisions can be calculated as follows:

- When two cars moving in __opposite__ directions collide with each other, the number of collisions increases by `2`.
- When a moving car collides with a stationary car, the number of collisions increases by `1`.

After a collision, the cars involved can no longer move and will stay at the point where they collided. Other than that, cars cannot change their state or direction of motion.

Return the total number of collisions that will happen on the road.

__Example:__

Example 1:

```text
Input: directions = "RLRSLL"
Output: 5
Explanation:
The collisions that will happen on the road are:
```

- Cars 0 and 1 will collide with each other. Since they are moving in opposite directions, the number of collisions becomes 0 + 2 = 2.
- Cars 2 and 3 will collide with each other. Since car 3 is stationary, the number of collisions becomes 2 + 1 = 3.
- Cars 3 and 4 will collide with each other. Since car 3 is stationary, the number of collisions becomes 3 + 1 = 4.
- Cars 4 and 5 will collide with each other. After car 4 collides with car 3, it will stay at the point of collision and get hit by car 5. The number of collisions becomes 4 + 1 = 5.

Thus, the total number of collisions that will happen on the road is 5.

Example 2:

```text
Input: directions = "LLRR"
Output: 0
Explanation:
No cars will collide with each other. Thus, the total number of collisions that will happen on the road is 0.
```

__Constraints:__

- `1 <= directions.length <= 10 ^ 5`
- `directions[i]` is either `'L'`, `'R'`, or `'S'`.

__题目描述:__

在一条无限长的公路上有 `n` 辆汽车正在行驶。汽车按从左到右的顺序按从 `0` 到 `n - 1` 编号，每辆车都在一个 __独特的__ 位置。

给你一个下标从 __0__ 开始的字符串 `directions` ，长度为 `n` 。 `directions[i]` 可以是 `'L'`、 `'R'` 或 `'S'` 分别表示第 `i` 辆车是向 __左__ 、向 __右__ 或者 __停留__ 在当前位置。每辆车移动时 __速度相同__ 。

碰撞次数可以按下述方式计算：

- 当两辆移动方向 __相反__ 的车相撞时，碰撞次数加 `2` 。
- 当一辆移动的车和一辆静止的车相撞时，碰撞次数加 `1` 。

碰撞发生后，涉及的车辆将无法继续移动并停留在碰撞位置。除此之外，汽车不能改变它们的状态或移动方向。

返回在这条道路上发生的 碰撞总次数 。

__示例:__

示例 1：

```text
输入：directions = "RLRSLL"
输出：5
解释：
将会在道路上发生的碰撞列出如下：
```

- 车 0 和车 1 会互相碰撞。由于它们按相反方向移动，碰撞数量变为 0 + 2 = 2 。
- 车 2 和车 3 会互相碰撞。由于 3 是静止的，碰撞数量变为 2 + 1 = 3 。
- 车 3 和车 4 会互相碰撞。由于 3 是静止的，碰撞数量变为 3 + 1 = 4 。
- 车 4 和车 5 会互相碰撞。在车 4 和车 3 碰撞之后，车 4 会待在碰撞位置，接着和车 5 碰撞。碰撞数量变为 4 + 1 = 5 。

因此，将会在道路上发生的碰撞总次数是 5 。

示例 2：

```text
输入：directions = "LLRR"
输出：0
解释：
不存在会发生碰撞的车辆。因此，将会在道路上发生的碰撞总次数是 0 。
```

__提示：__

- `1 <= directions.length <= 10 ^ 5`
- `directions[i]` 的值为 `'L'`、 `'R'` 或 `'S'`

__思路:__

```text
计数
开头的 L 和结尾的 R 不可能发生碰撞
中间的只要不为 S 就会发生碰撞
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countCollisions(string directions) 
    {
        int n = directions.size(), left = 0, right = n - 1, s = 0;
        while (left < n and directions[left] == 'L') ++left;
        while (right > -1 and directions[right] == 'R') --right;
        for (int i = left; i <= right; i++) s += directions[i] == 'S';
        return right - left + 1 - s;
    }
};
```

__Java__:

```Java
class Solution {
    public int countCollisions(String directions) {
        int n = directions.length(), left = 0, right = n - 1, s = 0;
        while (left < n && directions.charAt(left) == 'L') ++left;
        while (right > -1 && directions.charAt(right) == 'R') --right;
        for (int i = left; i <= right; i++) if (directions.charAt(i) == 'S') ++s;
        return right - left + 1 - s;
    }
}
```

__Python__:

```Python
class Solution:
    def countCollisions(self, directions: str) -> int:
        return len(s := directions.lstrip('L').rstrip('R')) - s.count('S')
```
