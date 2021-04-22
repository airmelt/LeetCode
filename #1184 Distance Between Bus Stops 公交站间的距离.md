# 1184 Distance Between Bus Stops 公交站间的距离

__Description__:
A bus has n stops numbered from 0 to n - 1 that form a circle. We know the distance between all pairs of neighboring stops where distance[i] is the distance between the stops number i and (i + 1) % n.

The bus goes along both directions i.e. clockwise and counterclockwise.

Return the shortest distance between the given start and destination stops.

__Example:__

Example 1:

![Bus Stops 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/08/untitled-diagram-1.jpg)

Input: distance = [1,2,3,4], start = 0, destination = 1
Output: 1
Explanation: Distance between 0 and 1 is 1 or 9, minimum is 1.

Example 2:

![Bus Stops 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/08/untitled-diagram-1-1.jpg)

Input: distance = [1,2,3,4], start = 0, destination = 2
Output: 3
Explanation: Distance between 0 and 2 is 3 or 7, minimum is 3.

Example 3:

![Bus Stops 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/08/untitled-diagram-1-2.jpg)

Input: distance = [1,2,3,4], start = 0, destination = 3
Output: 4
Explanation: Distance between 0 and 3 is 6 or 4, minimum is 4.

__Constraints:__
1 <= n <= 10^4
distance.length == n
0 <= start, destination < n
0 <= distance[i] <= 10^4

__题目描述__:
环形公交路线上有 n 个站，按次序从 0 到 n - 1 进行编号。我们已知每一对相邻公交站之间的距离，distance[i] 表示编号为 i 的车站和编号为 (i + 1) % n 的车站之间的距离。

环线上的公交车都可以按顺时针和逆时针的方向行驶。

返回乘客从出发点 start 到目的地 destination 之间的最短距离。

__示例 :__

示例 1：

![环形公交路线1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/08/untitled-diagram-1.jpg)

输入：distance = [1,2,3,4], start = 0, destination = 1
输出：1
解释：公交站 0 和 1 之间的距离是 1 或 9，最小值是 1。

示例 2：

![环形公交路线2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/08/untitled-diagram-1-1.jpg)

输入：distance = [1,2,3,4], start = 0, destination = 2
输出：3
解释：公交站 0 和 2 之间的距离是 3 或 7，最小值是 3。

示例 3：

![环形公交路线3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/08/untitled-diagram-1-2.jpg)

输入：distance = [1,2,3,4], start = 0, destination = 3
输出：4
解释：公交站 0 和 3 之间的距离是 6 或 4，最小值是 4。

__提示：__

1 <= n <= 10^4
distance.length == n
0 <= start, destination < n
0 <= distance[i] <= 10^4

__思路__:

由于公交车只能顺时针或者逆时针移动, 只要比较两个方向的距离和即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int distanceBetweenBusStops(vector<int>& distance, int start, int destination) 
    {
        int clockwise = 0, sum = accumulate(distance.begin(), distance.end(), 0);
        if (start > destination) swap(start, destination);
        for (int i = start; i < destination; ++i) clockwise += distance[I];
        return min(clockwise, sum - clockwise);
    }
};
```

__Java__:

```Java
class Solution {
    public int distanceBetweenBusStops(int[] distance, int start, int destination) {
        int clockwise = 0, sum = 0;
        if (start > destination) {
            start ^= destination;
            destination ^= start;
            start ^= destination;
        }
        for (int i : distance) sum += I;
        for (int i = start; i < destination; ++i) clockwise += distance[I];
        return Math.min(clockwise, sum - clockwise);
    }
}
```

__Python__:

```Python
class Solution:
    def distanceBetweenBusStops(self, distance: List[int], start: int, destination: int) -> int:
        return min(sum(distance[start:destination]), sum(distance) - sum(distance[start:destination])) if start < destination else min(sum(distance[destination:start]), sum(distance) - sum(distance[destination:start]))
```
