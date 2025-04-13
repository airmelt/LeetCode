# 2498 Frog Jump II 青蛙过河 II

__Description:__

You are given a __0-indexed__ integer array `stones` sorted in __strictly increasing order__ representing the positions of stones in a river.

A frog, initially on the first stone, wants to travel to the last stone and then return to the first stone. However, it can jump to any stone at most once.

The length of a jump is the absolute difference between the position of the stone the frog is currently on and the position of the stone to which the frog jumps.

- More formally, if the frog is at `stones[i]` and is jumping to `stones[j]`, the length of the jump is `|stones[i] - stones[j]|`.

The cost of a path is the maximum length of a jump among all jumps in the path.

Return the minimum cost of a path for the frog.

__Example:__

Example 1:

![2498-1](https://assets.leetcode.com/uploads/2022/11/14/example-1.png)

```text
Input: stones = [0,2,5,6,7]
Output: 5
Explanation: The above figure represents one of the optimal paths the frog can take.
The cost of this path is 5, which is the maximum length of a jump.
Since it is not possible to achieve a cost of less than 5, we return it.
```

Example 2:

![2498-2](https://assets.leetcode.com/uploads/2022/11/14/example-2.png)

```text
Input: stones = [0,3,9]
Output: 9
Explanation: 
The frog can jump directly to the last stone and come back to the first stone. 
In this case, the length of each jump will be 9. The cost for the path will be max(9, 9) = 9.
It can be shown that this is the minimum achievable cost.
```

__Constraints:__

- `2 <= stones.length <= 10 ^ 5`
- `0 <= stones[i] <= 10 ^ 9`
- `stones[0] == 0`
- `stones` is sorted in a strictly increasing order.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `stones` ，数组中的元素 __严格递增__ ，表示一条河中石头的位置。

一只青蛙一开始在第一块石头上，它想到达最后一块石头，然后回到第一块石头。同时每块石头 至多 到达 一次。

一次跳跃的 长度 是青蛙跳跃前和跳跃后所在两块石头之间的距离。

- 更正式的，如果青蛙从 `stones[i]` 跳到 `stones[j]` ，跳跃的长度为 `|stones[i] - stones[j]|` 。

一条路径的 代价 是这条路径里的 最大跳跃长度 。

请你返回这只青蛙的 最小代价 。

__示例:__

示例 1：

![2498-3](https://assets.leetcode.com/uploads/2022/11/14/example-1.png)

```text
输入：stones = [0,2,5,6,7]
输出：5
解释：上图展示了一条最优路径。
这条路径的代价是 5 ，是这条路径中的最大跳跃长度。
无法得到一条代价小于 5 的路径，我们返回 5 。
```

示例 2：

![2498-4](https://assets.leetcode.com/uploads/2022/11/14/example-2.png)

```text
输入：stones = [0,3,9]
输出：9
解释：
青蛙可以直接跳到最后一块石头，然后跳回第一块石头。
在这条路径中，每次跳跃长度都是 9 。所以路径代价是 max(9, 9) = 9 。
这是可行路径中的最小代价。
```

__提示：__

- `2 <= stones.length <= 10 ^ 5`
- `0 <= stones[i] <= 10 ^ 9`
- `stones[0] == 0`
- `stones` 中的元素严格递增。

__思路:__

```text
贪心
考虑 a - b - c - d
选择 a - c - d - b 或者 a - b - d - c
一定比 a - d 要更短
所以选择 stones[i] - stones[i - 2] 最小值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxJump(vector<int>& stones) 
    {
        int n = stones.size(), result = stones[1] - stones[0];
        for (int i = 2; i < n; i++) result = max(result, stones[i] - stones[i - 2]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxJump(int[] stones) {
        int n = stones.length, result = stones[1] - stones[0];
        for (int i = 2; i < n; i++) result = Math.max(result, stones[i] - stones[i - 2]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxJump(self, stones: List[int]) -> int:
        return max([stones[i] - stones[i - 2] for i in range(2, len(stones))]) if len(stones) > 2 else stones[-1]
```
