# 1503 Last Moment Before All Ants Fall Out of a Plank 所有蚂蚁掉下来前的最后一刻

__Description:__

We have a wooden plank of the length `n` __units__. Some ants are walking on the plank, each ant moves with a speed of __1 unit per second__. Some of the ants move to the __left__, the other move to the __right__.

When two ants moving in two different directions meet at some point, they change their directions and continue moving again. Assume changing directions does not take any additional time.

When an ant reaches __one end__ of the plank at a time `t`, it falls out of the plank immediately.

Given an integer `n` and two integer arrays `left` and `right`, the positions of the ants moving to the left and the right, return _the moment when the last ant(s) fall out of the plank_.

__Example:__

Example 1:

![1503-1](https://assets.leetcode.com/uploads/2020/06/17/ants.jpg)

```text
Input: n = 4, left = [4,3], right = [0,1]
Output: 4
Explanation: In the image above:
-The ant at index 0 is named A and going to the right.
-The ant at index 1 is named B and going to the right.
-The ant at index 3 is named C and going to the left.
-The ant at index 4 is named D and going to the left.
The last moment when an ant was on the plank is t = 4 seconds. After that, it falls immediately out of the plank. (i.e., We can say that at t = 4.0000000001, there are no ants on the plank).
```

Example 2:

![1503-2](https://assets.leetcode.com/uploads/2020/06/17/ants2.jpg)

```text
Input: n = 7, left = [], right = [0,1,2,3,4,5,6,7]
Output: 7
Explanation: All ants are going to the right, the ant at index 0 needs 7 seconds to fall.
```

Example 3:

![1503-3](https://assets.leetcode.com/uploads/2020/06/17/ants3.jpg)

```text
Input: n = 7, left = [0,1,2,3,4,5,6,7], right = []
Output: 7
Explanation: All ants are going to the left, the ant at index 7 needs 7 seconds to fall.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`
- `0 <= left.length <= n + 1`
- `0 <= left[i] <= n`
- `0 <= right.length <= n + 1`
- `0 <= right[i] <= n`
- `1 <= left.length + right.length <= n + 1`
- All values of `left` and `right` are unique, and each value can appear __only in one__ of the two arrays.

__题目描述:__

有一块木板，长度为 `n` 个 __单位__ 。一些蚂蚁在木板上移动，每只蚂蚁都以 __每秒一个单位__ 的速度移动。其中，一部分蚂蚁向 __左__ 移动，其他蚂蚁向 __右__ 移动。

当两只向 不同 方向移动的蚂蚁在某个点相遇时，它们会同时改变移动方向并继续移动。假设更改方向不会花费任何额外时间。

而当蚂蚁在某一时刻 `t` 到达木板的一端时，它立即从木板上掉下来。

给你一个整数 `n` 和两个整数数组 `left` 以及 `right` 。两个数组分别标识向左或者向右移动的蚂蚁在 `t = 0` 时的位置。请你返回最后一只蚂蚁从木板上掉下来的时刻。

__示例:__

示例 1：

![1503-4](https://assets.leetcode.com/uploads/2020/06/17/ants.jpg)

```text
输入：n = 4, left = [4,3], right = [0,1]
输出：4
解释：如上图所示：
-下标 0 处的蚂蚁命名为 A 并向右移动。
-下标 1 处的蚂蚁命名为 B 并向右移动。
-下标 3 处的蚂蚁命名为 C 并向左移动。
-下标 4 处的蚂蚁命名为 D 并向左移动。
请注意，蚂蚁在木板上的最后时刻是 t = 4 秒，之后蚂蚁立即从木板上掉下来。（也就是说在 t = 4.0000000001 时，木板上没有蚂蚁）。
```

示例 2：

![1503-5](https://assets.leetcode.com/uploads/2020/06/17/ants2.jpg)

```text
输入：n = 7, left = [], right = [0,1,2,3,4,5,6,7]
输出：7
解释：所有蚂蚁都向右移动，下标为 0 的蚂蚁需要 7 秒才能从木板上掉落。
```

示例 3：

![1503-6](https://assets.leetcode.com/uploads/2020/06/17/ants3.jpg)

```text
输入：n = 7, left = [0,1,2,3,4,5,6,7], right = []
输出：7
解释：所有蚂蚁都向左移动，下标为 7 的蚂蚁需要 7 秒才能从木板上掉落。
```

__提示：__

- `1 <= n <= 10 ^ 4`
- `0 <= left.length <= n + 1`
- `0 <= left[i] <= n`
- `0 <= right.length <= n + 1`
- `0 <= right[i] <= n`
- `1 <= left.length + right.length <= n + 1`
- `left` 和 `right` 中的所有值都是唯一的，并且每个值 __只能出现在二者之一__ 中。

__思路:__

```text
贪心
两只蚂蚁相遇时互相掉头实际上等于两只蚂蚁继续往前走
所以只需要返回 left 数组的最大值和 n - right 数组的最小值两者中的较大值
如果 left 为空最大值默认为 0, right 为空最小值默认为 n
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getLastMoment(int n, vector<int>& left, vector<int>& right) 
    {
        return max(left.empty() ? 0 : *max_element(left.begin(), left.end()), n - (right.empty() ? n : *min_element(right.begin(), right.end())));
    }
};
```

__Java__:

```Java
class Solution {
    public int getLastMoment(int n, int[] left, int[] right) {
        return Math.max(left.length == 0 ? 0 : IntStream.of(left).max().getAsInt(), n - (right.length == 0 ? n : IntStream.of(right).min().getAsInt()));
    }
}
```

__Python__:

```Python
class Solution:
    def getLastMoment(self, n: int, left: List[int], right: List[int]) -> int:
        return max(max(left + [0]), n - min(right + [n]))
```
