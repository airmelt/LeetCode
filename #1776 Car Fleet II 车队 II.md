# 1776 Car Fleet II 车队 II

__Description:__

There are `n` cars traveling at different speeds in the same direction along a one-lane road. You are given an array `cars` of length `n`, where `cars[i] = [positioni, speedi]` represents:

- `positioni` is the distance between the `i ^ th` car and the beginning of the road in meters. It is guaranteed that `positioni < positioni+1`.
- `speedi` is the initial speed of the `i ^ th` car in meters per second.

For simplicity, cars can be considered as points moving along the number line. Two cars collide when they occupy the same position. Once a car collides with another car, they unite and form a single car fleet. The cars in the formed fleet will have the same position and the same speed, which is the initial speed of the slowest car in the fleet.

Return an array `answer`, where `answer[i]` is the time, in seconds, at which the `i ^ th` car collides with the next car, or `-1` if the car does not collide with the next car. Answers within `10 ^ -5` of the actual answers are accepted.

__Example:__

Example 1:

```text
Input: cars = [[1,2],[2,1],[4,3],[7,2]]
Output: [1.00000,-1.00000,3.00000,-1.00000]
Explanation: After exactly one second, the first car will collide with the second car, and form a car fleet with speed 1 m/s. After exactly 3 seconds, the third car will collide with the fourth car, and form a car fleet with speed 2 m/s.
```

Example 2:

```text
Input: cars = [[3,4],[5,4],[6,3],[9,1]]
Output: [2.00000,1.00000,1.50000,-1.00000]
```

__Constraints:__

- `1 <= cars.length <= 10 ^ 5`
- `1 <= positioni, speedi <= 10 ^ 6`
- `positioni < positioni+1`

__题目描述:__

在一条单车道上有 `n` 辆车，它们朝着同样的方向行驶。给你一个长度为 `n` 的数组 `cars` ，其中 `cars[i] = [positioni, speedi]` ，它表示:

- `positioni` 是第 `i` 辆车和道路起点之间的距离（单位:米）。题目保证 `positioni < positioni+1` 。
- `speedi` 是第 `i` 辆车的初始速度（单位:米/秒）。

简单起见，所有车子可以视为在数轴上移动的点。当两辆车占据同一个位置时，我们称它们相遇了。一旦两辆车相遇，它们会合并成一个车队，这个车队里的车有着同样的位置和相同的速度，速度为这个车队里 最慢 一辆车的速度。

请你返回一个数组 `answer` ，其中 `answer[i]` 是第 `i` 辆车与下一辆车相遇的时间（单位:秒），如果这辆车不会与下一辆车相遇，则 `answer[i]` 为 `-1` 。答案精度误差需在 `10 ^ -5` 以内。

__示例:__

示例 1：

```text
输入：cars = [[1,2],[2,1],[4,3],[7,2]]
输出：[1.00000,-1.00000,3.00000,-1.00000]
解释：经过恰好 1 秒以后，第一辆车会与第二辆车相遇，并形成一个 1 m/s 的车队。经过恰好 3 秒以后，第三辆车会与第四辆车相遇，并形成一个 2 m/s 的车队。
```

示例 2：

```text
输入：cars = [[3,4],[5,4],[6,3],[9,1]]
输出：[2.00000,1.00000,1.50000,-1.00000]
```

__提示：__

- `1 <= cars.length <= 10 ^ 5`
- `1 <= positioni, speedi <= 10 ^ 6`
- `positioni < positioni+1`

__思路:__

```text
单调栈
从后往前遍历，维护一个单调递增的栈，栈中存储的是车的编号
车的编号对应该车队的最大速度
两辆车相遇一定要保证 t <= result[next], 这是因为如果 t > result[next], next 对应的车辆已经进入下一个车队
每次遍历的时候需要把所有速度大于当前遍历的车辆的车辆都出栈
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<double> getCollisionTimes(vector<vector<int>>& cars) 
    {
        auto T = [&](int i, int j) -> double { return 1.0 * (cars[j][0] - cars[i][0]) / (cars[i][1] - cars[j][1]); };
        int n = cars.size();
        vector<double> result(n, -1.0);
        stack<int> s;
        for (int i = n - 1; i >= 0; --i) 
        {
            while (!s.empty()) 
            {
                int next = s.top();
                if (cars[i][1] > cars[next][1]) 
                {
                    double t = T(i, next);
                    if (-1.0 == result[next] or t <= result[next]) 
                    {
                        result[i] = t;
                        break;
                    }
                }
                s.pop();
            }
            s.push(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public double[] getCollisionTimes(int[][] cars) {
        int n = cars.length;
        double EPS = 0.000001, t = 0.0, result[] = new double[n];
        Arrays.fill(result, -1.0);
        Stack<Integer> stack = new Stack<>();
        for (int i = n - 1; i > -1; i--) {
            while (!stack.isEmpty()) {
                int next = stack.peek();
                if (cars[next][1] < cars[i][1]) {
                    if ((t = helper(cars, i, next)) < result[next] || Math.abs(result[next] + 1.0) < EPS) {
                        result[i] = t;
                        break;
                    }
                }
                stack.pop();
            }
            stack.push(i);
        }
        return result;
    }

    private double helper(int[][] cars, int i, int j) {
        return 1.0 * (cars[j][0] - cars[i][0]) / (cars[i][1] - cars[j][1]);
    }
}
```

__Python__:

```Python
class Solution:
    def getCollisionTimes(self, cars: List[List[int]]) -> List[float]:
        result, stack, EPS = [-1.0] * (n := len(cars)), deque(), 10 ** -6
        for i in range(n - 1, -1, -1):
            while stack:
                next = stack[-1]
                if cars[i][1] > cars[next][1]:
                    if (t := 1.0 * (cars[next][0] - cars[i][0]) / (cars[i][1] - cars[next][1])) <= result[next] or abs(result[next] + 1) < EPS:
                        result[i] = t
                        break
                stack.pop()
            stack.append(i)
        return result
```
