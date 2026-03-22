# 1642 Furthest Building You Can Reach 可以到达的最远建筑

__Description:__

You are given an integer array `heights` representing the heights of buildings, some `bricks`, and some `ladders`.

You start your journey from building `0` and move to the next building by possibly using bricks or ladders.

While moving from building `i` to building `i+1` (__0-indexed__),

- If the current building's height is __greater than or equal__ to the next building's height, you do __not__ need a ladder or bricks.
- If the current building's height is _less than_ the next building's height, you can either use __one ladder__ or `(h[i+1] - h[i])` __bricks__.

Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally.

__Example:__

Example 1:

![1642-1](https://assets.leetcode.com/uploads/2020/10/27/q4.gif)

```text
Input: heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
Output: 4
Explanation: Starting at building 0, you can follow these steps:
- Go to building 1 without using ladders nor bricks since 4 >= 2.
- Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 < 7.
- Go to building 3 without using ladders nor bricks since 7 >= 6.
- Go to building 4 using your only ladder. You must use either bricks or ladders because 6 < 9.
It is impossible to go beyond building 4 because you do not have any more bricks or ladders.
```

Example 2:

```text
Input: heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
Output: 7
```

Example 3:

```text
Input: heights = [14,3,19,3], bricks = 17, ladders = 0
Output: 3
```

__Constraints:__

- `1 <= heights.length <= 10 ^ 5`
- `1 <= heights[i] <= 10 ^ 6`
- `0 <= bricks <= 10 ^ 9`
- `0 <= ladders <= heights.length`

__题目描述:__

给你一个整数数组 `heights` ，表示建筑物的高度。另有一些砖块 `bricks` 和梯子 `ladders` 。

你从建筑物 `0` 开始旅程，不断向后面的建筑物移动，期间可能会用到砖块或梯子。

当从建筑物 `i` 移动到建筑物 `i+1`（下标 __从 0 开始__ ）时:

- 如果当前建筑物的高度 __大于或等于__ 下一建筑物的高度，则不需要梯子或砖块
- 如果当前建筑的高度 __小于__ 下一个建筑的高度，您可以使用 __一架梯子__ 或 __`(h[i+1] - h[i])` 个砖块__

__示例:__

示例 1：

![1642-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/31/q4.gif)

```text
输入：heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
输出：4
解释：从建筑物 0 出发，你可以按此方案完成旅程：
- 不使用砖块或梯子到达建筑物 1 ，因为 4 >= 2
- 使用 5 个砖块到达建筑物 2 。你必须使用砖块或梯子，因为 2 < 7
- 不使用砖块或梯子到达建筑物 3 ，因为 7 >= 6
- 使用唯一的梯子到达建筑物 4 。你必须使用砖块或梯子，因为 6 < 9
无法越过建筑物 4 ，因为没有更多砖块或梯子。
```

示例 2：

```text
输入：heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
输出：7
```

示例 3：

```text
输入：heights = [14,3,19,3], bricks = 17, ladders = 0
输出：3
```

__提示：__

- `1 <= heights.length <= 10 ^ 5`
- `1 <= heights[i] <= 10 ^ 6`
- `0 <= bricks <= 10 ^ 9`
- `0 <= ladders <= heights.length`

__思路:__

```text
优先队列 ➕ 贪心
优先使用梯子, 并将高度差加入优先队列
如果使用的梯子数量超过了, 即优先队列的长度超过了梯子数
弹出小根堆堆顶, 这个值为当前最小的差值, 替换为砖块
如果砖块数不够, 提前终止
时间复杂度为 O(NlogM), 空间复杂度为 O(N), M 为梯子的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) 
    {
        int n = heights.size(), result = 0;
        priority_queue<int, vector<int>, greater<int>> q;
        while (result++ < n - 1) 
        {
            if (heights[result] > heights[result - 1]) 
            {
                q.push(heights[result] - heights[result - 1]);
                if (q.size() > ladders) 
                {
                    bricks -= q.top();
                    q.pop();
                }
                if (bricks < 0) return result - 1;
            }
        }
        return result - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        int result = 0, n = heights.length;
        while (result++ < n - 1) {
            if (heights[result] > heights[result - 1]) {
                queue.add(heights[result] - heights[result - 1]);
                if (queue.size() > ladders) bricks -= queue.poll();
                if (bricks < 0) return result - 1;
            }
        }
        return result - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def furthestBuilding(self, heights: List[int], bricks: int, ladders: int) -> int:
        n, q, result = len(heights), [], 0
        while result < n - 1:
            if heights[result] >= heights[result + 1]:
                result += 1
            else:
                heappush(q, heights[result + 1] - heights[result])
                if len(q) > ladders:
                    bricks -= heappop(q)
                if bricks < 0:
                    return result
                result += 1
        return result
```
