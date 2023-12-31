# 1936 Add Minimum Number of Rungs 新增的最少台阶数

__Description:__

You are given a __strictly increasing__ integer array `rungs` that represents the __height__ of rungs on a ladder. You are currently on the __floor__ at height `0`, and you want to reach the last rung.

You are also given an integer `dist`. You can only climb to the next highest rung if the distance between where you are currently at (the floor or on a rung) and the next rung is __at most__ `dist`. You are able to insert rungs at any positive __integer__ height if a rung is not already there.

Return the minimum number of rungs that must be added to the ladder in order for you to climb to the last rung.

__Example:__

Example 1:

```text
Input: rungs = [1,3,5,10], dist = 2
Output: 2
Explanation:
You currently cannot reach the last rung.
Add rungs at heights 7 and 8 to climb this ladder. 
The ladder will now have rungs at [1,3,5,7,8,10].
```

Example 2:

```text
Input: rungs = [3,6,8,10], dist = 3
Output: 0
Explanation:
This ladder can be climbed without adding additional rungs.
```

Example 3:

```text
Input: rungs = [3,4,6,7], dist = 2
Output: 1
Explanation:
You currently cannot reach the first rung from the ground.
Add a rung at height 1 to climb this ladder.
The ladder will now have rungs at [1,3,4,6,7].
```

__Constraints:__

- `1 <= rungs.length <= 10 ^ 5`
- `1 <= rungs[i] <= 10 ^ 9`
- `1 <= dist <= 10 ^ 9`
- `rungs` is __strictly increasing__.

__题目描述:__

给你一个 __严格递增__ 的整数数组 `rungs` ，用于表示梯子上每一台阶的 __高度__ 。当前你正站在高度为 `0` 的地板上，并打算爬到最后一个台阶。

另给你一个整数 `dist` 。每次移动中，你可以到达下一个距离你当前位置（地板或台阶）__不超过__ `dist` 高度的台阶。当然，你也可以在任何正 __整数__ 高度处插入尚不存在的新台阶。

返回爬到最后一阶时必须添加到梯子上的 最少 台阶数。

__示例:__

示例 1：

```text
输入：rungs = [1,3,5,10], dist = 2
输出：2
解释：
现在无法到达最后一阶。
在高度为 7 和 8 的位置增设新的台阶，以爬上梯子。 
梯子在高度为 [1,3,5,7,8,10] 的位置上有台阶。
```

示例 2：

```text
输入：rungs = [3,6,8,10], dist = 3
输出：0
解释：
这个梯子无需增设新台阶也可以爬上去。
```

示例 3：

```text
输入：rungs = [3,4,6,7], dist = 2
输出：1
解释：
现在无法从地板到达梯子的第一阶。 
在高度为 1 的位置增设新的台阶，以爬上梯子。 
梯子在高度为 [1,3,4,6,7] 的位置上有台阶。
```

示例 4：

```text
输入：rungs = [5], dist = 10
输出：0
解释：这个梯子无需增设新台阶也可以爬上去。
```

__提示：__

- `1 <= rungs.length <= 10 ^ 5`
- `1 <= rungs[i] <= 10 ^ 9`
- `1 <= dist <= 10 ^ 9`
- `rungs` __严格递增__

__思路:__

```text
贪心
从 0 开始计算相邻两个台阶的高度
如果高度 - 1 > dist, 则需要新增 (高度 - 1) / dist 个台阶
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int addRungs(vector<int>& rungs, int dist) 
    {
        int result = 0, n = rungs.size();
        for (int i = 0; i < n; i++) result += (rungs[i] - (!i ? i : rungs[i - 1]) - 1) / dist;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int addRungs(int[] rungs, int dist) {
        int result = 0, n = rungs.length;
        for (int i = 0; i < n; i++) result += (rungs[i] - (i == 0 ? i : rungs[i - 1]) - 1) / dist;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def addRungs(self, rungs: List[int], dist: int) -> int:
        return sum((rungs[i] - (0 if not i else rungs[i - 1]) - 1) // dist for i in range(len(rungs)))
```
