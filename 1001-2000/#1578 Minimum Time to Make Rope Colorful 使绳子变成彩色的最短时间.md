# 1578 Minimum Time to Make Rope Colorful 使绳子变成彩色的最短时间

__Description:__

Alice has `n` balloons arranged on a rope. You are given a __0-indexed__ string `colors` where `colors[i]` is the color of the `i ^ th` balloon.

Alice wants the rope to be __colorful__. She does not want __two consecutive balloons__ to be of the same color, so she asks Bob for help. Bob can remove some balloons from the rope to make it __colorful__. You are given a __0-indexed__ integer array `neededTime` where `neededTime[i]` is the time (in seconds) that Bob needs to remove the `i ^ th` balloon from the rope.

Return the minimum time Bob needs to make the rope colorful.

__Example:__

Example 1:

![1578-1](https://assets.leetcode.com/uploads/2021/12/13/ballon1.jpg)

```text
Input: colors = "abaac", neededTime = [1,2,3,4,5]
Output: 3
Explanation: In the above image, 'a' is blue, 'b' is red, and 'c' is green.
Bob can remove the blue balloon at index 2. This takes 3 seconds.
There are no longer two consecutive balloons of the same color. Total time = 3.
```

Example 2:

![1578-2](https://assets.leetcode.com/uploads/2021/12/13/balloon2.jpg)

```text
Input: colors = "abc", neededTime = [1,2,3]
Output: 0
Explanation: The rope is already colorful. Bob does not need to remove any balloons from the rope.
```

Example 3:

![1578-3](https://assets.leetcode.com/uploads/2021/12/13/balloon3.jpg)

```text
Input: colors = "aabaa", neededTime = [1,2,3,4,1]
Output: 2
Explanation: Bob will remove the ballons at indices 0 and 4. Each ballon takes 1 second to remove.
There are no longer two consecutive balloons of the same color. Total time = 1 + 1 = 2.
```

__Constraints:__

- `n == colors.length == neededTime.length`
- `1 <= n <= 10 ^ 5`
- `1 <= neededTime[i] <= 10 ^ 4`
- `colors` contains only lowercase English letters.

__题目描述:__

Alice 把 `n` 个气球排列在一根绳子上。给你一个下标从 __0__ 开始的字符串 `colors` ，其中 `colors[i]` 是第 `i` 个气球的颜色。

Alice 想要把绳子装扮成 __彩色__ ，且她不希望两个连续的气球涂着相同的颜色，所以她喊来 Bob 帮忙。Bob 可以从绳子上移除一些气球使绳子变成 __彩色__ 。给你一个下标从 __0__ 开始的整数数组 `neededTime` ，其中 `neededTime[i]` 是 Bob 从绳子上移除第 `i` 个气球需要的时间（以秒为单位）。

返回 Bob 使绳子变成 彩色 需要的 最少时间 。

__示例:__

示例 1：

![1578-4](https://assets.leetcode.com/uploads/2021/12/13/ballon1.jpg)

```text
输入：colors = "abaac", neededTime = [1,2,3,4,5]
输出：3
解释：在上图中，'a' 是蓝色，'b' 是红色且 'c' 是绿色。
Bob 可以移除下标 2 的蓝色气球。这将花费 3 秒。
移除后，不存在两个连续的气球涂着相同的颜色。总时间 = 3 。
```

示例 2：

![1578-5](https://assets.leetcode.com/uploads/2021/12/13/balloon2.jpg)

```text
输入：colors = "abc", neededTime = [1,2,3]
输出：0
解释：绳子已经是彩色的，Bob 不需要从绳子上移除任何气球。
```

示例 3：

![1578-6](https://assets.leetcode.com/uploads/2021/12/13/balloon3.jpg)

```text
输入：colors = "aabaa", neededTime = [1,2,3,4,1]
输出：2
解释：Bob 会移除下标 0 和下标 4 处的气球。这两个气球各需要 1 秒来移除。
移除后，不存在两个连续的气球涂着相同的颜色。总时间 = 1 + 1 = 2 。
```

__提示：__

- `n == colors.length == neededTime.length`
- `1 <= n <= 10 ^ 5`
- `1 <= neededTime[i] <= 10 ^ 4`
- `colors` 仅由小写英文字母组成

__思路:__

```text
贪心
只要在两个相邻的相同颜色的气球中, 选择较大值保留即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCost(string colors, vector<int>& neededTime) 
    {
        int result = 0, n = neededTime.size();
        for (int i = 1; i < n; i++) 
        {
            if (colors[i] == colors[i - 1])
            {
                result += min(neededTime[i], neededTime[i - 1]);
                neededTime[i] = max(neededTime[i], neededTime[i - 1]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(String colors, int[] neededTime) {
        int result = 0, n = neededTime.length;
        for (int i = 1; i < n; i++) {
            if (colors.charAt(i) == colors.charAt(i - 1)) {
                result += Math.min(neededTime[i], neededTime[i - 1]);
                neededTime[i] = Math.max(neededTime[i], neededTime[i - 1]);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, colors: str, neededTime: List[int]) -> int:
        result, n = 0, len(colors)
        for i in range(1, n):
            if colors[i] == colors[i - 1]:
                result += min(neededTime[i], neededTime[i - 1])
                neededTime[i] = max(neededTime[i], neededTime[i - 1])
        return result
```
