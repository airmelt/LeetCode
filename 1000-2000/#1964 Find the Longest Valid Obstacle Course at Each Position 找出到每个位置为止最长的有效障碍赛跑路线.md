# 1964 Find the Longest Valid Obstacle Course at Each Position 找出到每个位置为止最长的有效障碍赛跑路线

__Description:__

You want to build some obstacle courses. You are given a __0-indexed__ integer array `obstacles` of length `n`, where `obstacles[i]` describes the height of the `i ^ th` obstacle.

For every index `i` between `0` and `n - 1` (__inclusive__), find the length of the __longest obstacle course__ in `obstacles` such that:

- You choose any number of obstacles between `0` and `i` __inclusive__.
- You must include the `i ^ th` obstacle in the course.
- You must put the chosen obstacles in the __same order__ as they appear in `obstacles`.
- Every obstacle (except the first) is __taller__ than or the __same height__ as the obstacle immediately before it.

Return _an array_ `ans` _of length_ `n`, _where_ `ans[i]` _is the length of the __longest obstacle course__ for index_ `i` _as described above_.

__Example:__

Example 1:

```text
Input: obstacles = [1,2,3,2]
Output: [1,2,3,3]
Explanation: The longest valid obstacle course at each position is:
```

- i = 0: [1], [1] has length 1.
- i = 1: [1,2], [1,2] has length 2.
- i = 2: [1,2,3], [1,2,3] has length 3.
- i = 3: [1,2,3,2], [1,2,2] has length 3.

Example 2:

```text
Input: obstacles = [2,2,1]
Output: [1,2,1]
Explanation: The longest valid obstacle course at each position is:
```

- i = 0: [2], [2] has length 1.
- i = 1: [2,2], [2,2] has length 2.
- i = 2: [2,2,1], [1] has length 1.

Example 3:

```text
Input: obstacles = [3,1,5,6,4,2]
Output: [1,1,2,3,2,2]
Explanation: The longest valid obstacle course at each position is:
```

- i = 0: [3], [3] has length 1.
- i = 1: [3,1], [1] has length 1.
- i = 2: [3,1,5], [3,5] has length 2. [1,5] is also valid.
- i = 3: [3,1,5,6], [3,5,6] has length 3. [1,5,6] is also valid.
- i = 4: [3,1,5,6,4], [3,4] has length 2. [1,4] is also valid.
- i = 5: [3,1,5,6,4,2], [1,2] has length 2.

__Constraints:__

- `n == obstacles.length`
- `1 <= n <= 10 ^ 5`
- `1 <= obstacles[i] <= 10 ^ 7`

__题目描述:__

你打算构建一些障碍赛跑路线。给你一个 __下标从 0 开始__ 的整数数组 `obstacles` ，数组长度为 `n` ，其中 `obstacles[i]` 表示第 `i` 个障碍的高度。

对于每个介于 `0` 和 `n - 1` 之间（包含 `0` 和 `n - 1`）的下标  `i` ，在满足下述条件的前提下，请你找出 `obstacles` 能构成的最长障碍路线的长度:

- 你可以选择下标介于 `0` 到 `i` 之间（包含 `0` 和 `i`）的任意个障碍。
- 在这条路线中，必须包含第 `i` 个障碍。
- 你必须按障碍在 `obstacles` 中的 __出现顺序__ 布置这些障碍。
- 除第一个障碍外，路线中每个障碍的高度都必须和前一个障碍 __相同__ 或者 __更高__ 。

返回长度为 `n` 的答案数组 `ans` ，其中 `ans[i]` 是上面所述的下标 `i` 对应的最长障碍赛跑路线的长度。

__示例:__

示例 1：

```text
输入：obstacles = [1,2,3,2]
输出：[1,2,3,3]
解释：每个位置的最长有效障碍路线是：
```

- i = 0: [1], [1] 长度为 1
- i = 1: [1,2], [1,2] 长度为 2
- i = 2: [1,2,3], [1,2,3] 长度为 3
- i = 3: [1,2,3,2], [1,2,2] 长度为 3

示例 2：

```text
输入：obstacles = [2,2,1]
输出：[1,2,1]
解释：每个位置的最长有效障碍路线是：
```

- i = 0: [2], [2] 长度为 1
- i = 1: [2,2], [2,2] 长度为 2
- i = 2: [2,2,1], [1] 长度为 1

示例 3：

```text
输入：obstacles = [3,1,5,6,4,2]
输出：[1,1,2,3,2,2]
解释：每个位置的最长有效障碍路线是：
```

- i = 0: [3], [3] 长度为 1
- i = 1: [3,1], [1] 长度为 1
- i = 2: [3,1,5], [3,5] 长度为 2, [1,5] 也是有效的障碍赛跑路线
- i = 3: [3,1,5,6], [3,5,6] 长度为 3, [1,5,6] 也是有效的障碍赛跑路线
- i = 4: [3,1,5,6,4], [3,4] 长度为 2, [1,4] 也是有效的障碍赛跑路线
- i = 5: [3,1,5,6,4,2], [1,2] 长度为 2

__提示：__

- `n == obstacles.length`
- `1 <= n <= 10 ^ 5`
- `1 <= obstacles[i] <= 10 ^ 7`

__思路:__

```text
二分查找
实际上题目要求的就是到当前位置为止, 最长的递增子序列的长度
因此可以使用二分查找, 用 dp 数组维护当前的递增子序列
如果当前元素大于 dp 中的最后一个元素, 则直接添加到 dp 中
否则使用二分查找找到第一个大于当前元素的位置, 将其替换为当前元素
结果为 left + 1
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> longestObstacleCourseAtEachPosition(vector<int>& obstacles) 
    {
        int n = obstacles.size(), p = 0;
        vector<int> dp, result(n);
        for (int i = 0; i < n; i++)
        {
            if ((p = upper_bound(dp.begin(), dp.end(), obstacles[i]) - dp.begin()) < dp.size()) dp[p] = obstacles[i];
            else dp.emplace_back(obstacles[i]);
            result[i] = p + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] longestObstacleCourseAtEachPosition(int[] obstacles) {
        int n = obstacles.length, result[] = new int[n];
        List<Integer> dp = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int cur = obstacles[i], left = 0, right = dp.size(), mid = 0;
            while (left < right) {
                if (dp.get(mid = left + ((right - left) >> 1)) > cur) right = mid;
                else left = mid + 1;
            }
            if (left == dp.size()) dp.add(cur);
            else dp.set(left, cur);
            result[i] = left + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestObstacleCourseAtEachPosition(self, obstacles: List[int]) -> List[int]:
        dp, result = [], [0] * (n := len(obstacles))
        for i, v in enumerate(obstacles):
            if (p := bisect_left(dp, v + 1)) < len(dp):
                dp[p] = v
            else:
                dp.append(v)
            result[i] = p + 1
        return result
```
