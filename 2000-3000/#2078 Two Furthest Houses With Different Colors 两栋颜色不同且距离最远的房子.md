# 2078 Two Furthest Houses With Different Colors 两栋颜色不同且距离最远的房子

__Description:__

There are `n` houses evenly lined up on the street, and each house is beautifully painted. You are given a __0-indexed__ integer array `colors` of length `n`, where `colors[i]` represents the color of the `i ^ th` house.

Return the maximum distance between two houses with different colors.

The distance between the `i ^ th` and `j ^ th` houses is `abs(i - j)`, where `abs(x)` is the __absolute value__ of `x`.

__Example:__

Example 1:

![2078-1](https://assets.leetcode.com/uploads/2021/10/31/eg1.png)

```text
Input: colors = [1,1,1,6,1,1,1]
Output: 3
Explanation: In the above image, color 1 is blue, and color 6 is red.
The furthest two houses with different colors are house 0 and house 3.
House 0 has color 1, and house 3 has color 6. The distance between them is abs(0 - 3) = 3.
Note that houses 3 and 6 can also produce the optimal answer.
```

Example 2:

![2078-2](https://assets.leetcode.com/uploads/2021/10/31/eg2.png)

```text
Input: colors = [1,8,3,8,3]
Output: 4
Explanation: In the above image, color 1 is blue, color 8 is yellow, and color 3 is green.
The furthest two houses with different colors are house 0 and house 4.
House 0 has color 1, and house 4 has color 3. The distance between them is abs(0 - 4) = 4.
```

Example 3:

```text
Input: colors = [0,1]
Output: 1
Explanation: The furthest two houses with different colors are house 0 and house 1.
House 0 has color 0, and house 1 has color 1. The distance between them is abs(0 - 1) = 1.
```

__Constraints:__

- `n == colors.length`
- `2 <= n <= 100`
- `0 <= colors[i] <= 100`
- Test data are generated such that __at least__ two houses have different colors.

__题目描述:__

街上有 `n` 栋房子整齐地排成一列，每栋房子都粉刷上了漂亮的颜色。给你一个下标从 __0__ 开始且长度为 `n` 的整数数组 `colors` ，其中 `colors[i]` 表示第  `i` 栋房子的颜色。

返回 两栋 颜色 不同 房子之间的 最大 距离。

第 `i` 栋房子和第 `j` 栋房子之间的距离是 `abs(i - j)` ，其中 `abs(x)` 是 `x` 的绝对值。

__示例:__

示例 1：

![2078-3](https://assets.leetcode.com/uploads/2021/10/31/eg1.png)

```text
输入：colors = [1,1,1,6,1,1,1]
输出：3
解释：上图中，颜色 1 标识成蓝色，颜色 6 标识成红色。
两栋颜色不同且距离最远的房子是房子 0 和房子 3 。
房子 0 的颜色是颜色 1 ，房子 3 的颜色是颜色 6 。两栋房子之间的距离是 abs(0 - 3) = 3 。
注意，房子 3 和房子 6 也可以产生最佳答案。
```

示例 2：

![2078-4](https://assets.leetcode.com/uploads/2021/10/31/eg2.png)

```text
输入：colors = [1,8,3,8,3]
输出：4
解释：上图中，颜色 1 标识成蓝色，颜色 8 标识成黄色，颜色 3 标识成绿色。
两栋颜色不同且距离最远的房子是房子 0 和房子 4 。
房子 0 的颜色是颜色 1 ，房子 4 的颜色是颜色 3 。两栋房子之间的距离是 abs(0 - 4) = 4 。
```

示例 3：

```text
输入：colors = [0,1]
输出：1
解释：两栋颜色不同且距离最远的房子是房子 0 和房子 1 。
房子 0 的颜色是颜色 0 ，房子 1 的颜色是颜色 1 。两栋房子之间的距离是 abs(0 - 1) = 1 。
```

__提示：__

- `n == colors.length`
- `2 <= n <= 100`
- `0 <= colors[i] <= 100`
- 生成的测试数据满足 __至少__ 存在 2 栋颜色不同的房子

__思路:__

```text
双指针
如果 colors[0] != colors[n - i - 1] 或者 colors[i] != colors[-1]
则有最大值 n - i - 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxDistance(vector<int>& colors) 
    {
        for (int i = 0, n = colors.size(); i < n; i++) if (colors[i] != colors.back() || colors.front() != colors[n - i - 1]) return n - i - 1;
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDistance(int[] colors) {
        for (int i = 0, n = colors.length; i < n; i++) if (colors[i] != colors[n - 1] || colors[0] != colors[n - i - 1]) return n - i - 1;
        return 0;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDistance(self, colors: List[int]) -> int:
        return max(j - i for i in range(len(colors)) for j in range(i + 1, len(colors)) if colors[i] != colors[j])
```
