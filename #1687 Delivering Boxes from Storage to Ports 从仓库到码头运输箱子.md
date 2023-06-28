# 1687 Delivering Boxes from Storage to Ports 从仓库到码头运输箱子

__Description:__

You have the task of delivering some boxes from storage to their ports using only one ship. However, this ship has a limit on the number of boxes and the total weight that it can carry.

You are given an array `boxes`, where `boxes[i] = [ports​​i​, weighti]`, and three integers `portsCount`, `maxBoxes`, and `maxWeight`.

- `ports​​i` is the port where you need to deliver the `i ^ th` box and `weightsi` is the weight of the `i ^ th` box.
- `portsCount` is the number of ports.
- `maxBoxes` and `maxWeight` are the respective box and weight limits of the ship.

The boxes need to be delivered in the order they are given. The ship will follow these steps:

- The ship will take some number of boxes from the `boxes` queue, not violating the `maxBoxes` and `maxWeight` constraints.
- For each loaded box __in order__, the ship will make a __trip__ to the port the box needs to be delivered to and deliver it. If the ship is already at the correct port, no __trip__ is needed, and the box can immediately be delivered.
- The ship then makes a return __trip__ to storage to take more boxes from the queue.

The ship must end at storage after all the boxes have been delivered.

Return the minimum number of trips the ship needs to make to deliver all boxes to their respective ports.

__Example:__

Example 1:

```text
Input: boxes = [[1,1],[2,1],[1,1]], portsCount = 2, maxBoxes = 3, maxWeight = 3
Output: 4
Explanation: The optimal strategy is as follows: 
- The ship takes all the boxes in the queue, goes to port 1, then port 2, then port 1 again, then returns to storage. 4 trips.
So the total number of trips is 4.
Note that the first and third boxes cannot be delivered together because the boxes need to be delivered in order (i.e. the second box needs to be delivered at port 2 before the third box).
```

Example 2:

```text
Input: boxes = [[1,2],[3,3],[3,1],[3,1],[2,4]], portsCount = 3, maxBoxes = 3, maxWeight = 6
Output: 6
Explanation: The optimal strategy is as follows: 
- The ship takes the first box, goes to port 1, then returns to storage. 2 trips.
- The ship takes the second, third and fourth boxes, goes to port 3, then returns to storage. 2 trips.
- The ship takes the fifth box, goes to port 2, then returns to storage. 2 trips.
So the total number of trips is 2 + 2 + 2 = 6.
```

Example 3:

```text
Input: boxes = [[1,4],[1,2],[2,1],[2,1],[3,2],[3,4]], portsCount = 3, maxBoxes = 6, maxWeight = 7
Output: 6
Explanation: The optimal strategy is as follows:
- The ship takes the first and second boxes, goes to port 1, then returns to storage. 2 trips.
- The ship takes the third and fourth boxes, goes to port 2, then returns to storage. 2 trips.
- The ship takes the fifth and sixth boxes, goes to port 3, then returns to storage. 2 trips.
So the total number of trips is 2 + 2 + 2 = 6.
```

__Constraints:__

- `1 <= boxes.length <= 10 ^ 5`
- `1 <= portsCount, maxBoxes, maxWeight <= 10 ^ 5`
- `1 <= ports​​i <= portsCount`
- `1 <= weightsi <= maxWeight`

__题目描述:__

你有一辆货运卡车，你需要用这一辆车把一些箱子从仓库运送到码头。这辆卡车每次运输有 箱子数目的限制 和 总重量的限制 。

给你一个箱子数组 `boxes` 和三个整数 `portsCount`, `maxBoxes` 和 `maxWeight` ，其中 `boxes[i] = [ports​​i​, weighti]` 。

- `ports​​i` 表示第 `i` 个箱子需要送达的码头， `weightsi` 是第 `i` 个箱子的重量。
- `portsCount` 是码头的数目。
- `maxBoxes` 和 `maxWeight` 分别是卡车每趟运输箱子数目和重量的限制。

箱子需要按照 数组顺序 运输，同时每次运输需要遵循以下步骤：

- 卡车从 `boxes` 队列中按顺序取出若干个箱子，但不能违反 `maxBoxes` 和 `maxWeight` 限制。
- 对于在卡车上的箱子，我们需要 __按顺序__ 处理它们，卡车会通过 __一趟行程__ 将最前面的箱子送到目的地码头并卸货。如果卡车已经在对应的码头，那么不需要 __额外行程__ ，箱子也会立马被卸货。
- 卡车上所有箱子都被卸货后，卡车需要 __一趟行程__ 回到仓库，从箱子队列里再取出一些箱子。

卡车在将所有箱子运输并卸货后，最后必须回到仓库。

请你返回将所有箱子送到相应码头的 最少行程 次数。

__示例:__

示例 1：

```text
输入：boxes = [[1,1],[2,1],[1,1]], portsCount = 2, maxBoxes = 3, maxWeight = 3
输出：4
解释：最优策略如下：
- 卡车将所有箱子装上车，到达码头 1 ，然后去码头 2 ，然后再回到码头 1 ，最后回到仓库，总共需要 4 趟行程。
所以总行程数为 4 。
注意到第一个和第三个箱子不能同时被卸货，因为箱子需要按顺序处理（也就是第二个箱子需要先被送到码头 2 ，然后才能处理第三个箱子）。
```

示例 2：

```text
输入：boxes = [[1,2],[3,3],[3,1],[3,1],[2,4]], portsCount = 3, maxBoxes = 3, maxWeight = 6
输出：6
解释：最优策略如下：
- 卡车首先运输第一个箱子，到达码头 1 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第二、第三、第四个箱子，到达码头 3 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第五个箱子，到达码头 2 ，回到仓库，总共 2 趟行程。
总行程数为 2 + 2 + 2 = 6 。
```

示例 3：

```text
输入：boxes = [[1,4],[1,2],[2,1],[2,1],[3,2],[3,4]], portsCount = 3, maxBoxes = 6, maxWeight = 7
输出：6
解释：最优策略如下：
- 卡车运输第一和第二个箱子，到达码头 1 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第三和第四个箱子，到达码头 2 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第五和第六个箱子，到达码头 3 ，然后回到仓库，总共 2 趟行程。
总行程数为 2 + 2 + 2 = 6 。
```

示例 4：

```text
输入：boxes = [[2,4],[2,5],[3,1],[3,2],[3,7],[3,1],[4,4],[1,3],[5,2]], portsCount = 5, maxBoxes = 5, maxWeight = 7
输出：14
解释：最优策略如下：
- 卡车运输第一个箱子，到达码头 2 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第二个箱子，到达码头 2 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第三和第四个箱子，到达码头 3 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第五个箱子，到达码头 3 ，然后回到仓库，总共 2 趟行程。
- 卡车运输第六和第七个箱子，到达码头 3 ，然后去码头 4 ，然后回到仓库，总共 3 趟行程。
- 卡车运输第八和第九个箱子，到达码头 1 ，然后去码头 5 ，然后回到仓库，总共 3 趟行程。
总行程数为 2 + 2 + 2 + 2 + 3 + 3 = 14 。
```

__提示：__

- `1 <= boxes.length <= 10 ^ 5`
- `1 <= portsCount, maxBoxes, maxWeight <= 10 ^ 5`
- `1 <= ports​​i <= portsCount`
- `1 <= weightsi <= maxWeight`

__思路:__

```text
动态规划 ➕ 单调队列
设 dp[i] 表示前 i 个箱子需要的行程数
dp[i] = min(dp[j] + cost(j, i)), 即从第 j 个箱子开始, 搬运 j - i 的箱子的最小值, j 需要满足 maxWeight 和 maxBoxes 两个条件
cost(j, i) 表示从第 j 个箱子到第 i 个箱子需要的行程数
这个算法的时间复杂度为 O(N ^ 3)
由 dp[i] = dp[i - maxBoxes] + cost(i - maxBoxes, i) + ... + dp[i - 1] + cost(i - 1, i)
dp[i - 1] = dp[i - maxBoxes - 1] + cost(i - maxBoxes - 1, i - 1) + ... + dp[i - 1] + cost(i - 1, i - 1)
可以把等式右边看作一个滑动窗口, 每次求滑动窗口里的最小值
对比 dp[i] 和 dp[i - 1], 可以得到每次窗口会相差一个当前码头的行程
我们可以用一个变量 diff 记录, 如果当前码头和之前的码头相同则不需要增加行程, 否则需要增加 1
由于窗口的大小为 maxBoxes, 所以我们可以用一个单调递增队列来维护窗口
还要用一个变量 weight 来记录窗口里的箱子的重量
滑动窗口记录第 i 个箱子, 行程数和重量
每次结果取单调队列的第一个行程数加上 diff 即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int boxDelivering(vector<vector<int>>& boxes, int portsCount, int maxBoxes, int maxWeight) 
    {
        long n = boxes.size(), result = 0, diff = 0, weight = 0;
        deque<vector<long>> q;
        for (int i = 0; i < n; i++) 
        {
            int cur = result + 2;
            diff += i and boxes[i].front() != boxes[i - 1].front();
            weight += boxes[i].back();
            while (!q.empty() and q.back()[1] + diff >= cur) q.pop_back();
            q.push_back({i, cur - diff, boxes[i][1] - weight}); 
            while (q.front().front() <= i - maxBoxes || q.front().back() + weight > maxWeight) q.pop_front();
            result = q.front()[1] + diff; 
        }  
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int boxDelivering(int[][] boxes, int portsCount, int maxBoxes, int maxWeight) {
        int n = boxes.length, result = 0, diff = 0, weight = 0;
        Deque<int[]> q = new ArrayDeque<int[]>();
        for (int i = 0; i < n; i++) {
            int cur = result + 2;
            diff += i > 0 && boxes[i][0] != boxes[i - 1][0] ? 1 : 0;
            weight += boxes[i][1];
            while (!q.isEmpty() && q.peekLast()[1] + diff >= cur) q.pollLast();
            q.add(new int[]{i, cur - diff, boxes[i][1] - weight}); 
            while (q.peekFirst()[0] <= i - maxBoxes || q.peekFirst()[2] + weight > maxWeight) q.pollFirst();
            result = q.peekFirst()[1] + diff; 
        }  
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def boxDelivering(self, boxes: List[List[int]], portsCount: int, maxBoxes: int, maxWeight: int) -> int:
        n, result, diff, weight, q = len(boxes), 0, 0, 0, deque()
        for i in range(n):
            cur = result + 2
            diff += i and boxes[i][0] != boxes[i - 1][0]
            weight += boxes[i][1]
            while q and q[-1][1] + diff >= cur:
                q.pop()
            q.append([i, cur - diff, boxes[i][1] - weight])
            while i - q[0][0] >= maxBoxes or q[0][-1] + weight > maxWeight:
                q.popleft()
            result = q[0][1] + diff
        return result
```
