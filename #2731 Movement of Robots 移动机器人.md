# 2731 Movement of Robots 移动机器人

__Description:__

Some robots are standing on an infinite number line with their initial coordinates given by a __0-indexed__ integer array `nums` and will start moving once given the command to move. The robots will move a unit distance each second.

You are given a string `s` denoting the direction in which robots will move on command. `'L'` means the robot will move towards the left side or negative side of the number line, whereas `'R'` means the robot will move towards the right side or positive side of the number line.

If two robots collide, they will start moving in opposite directions.

Return _the sum of distances between all the pairs of robots_ `d` _seconds after the command._ Since the sum can be very large, return it modulo `10 ^ 9 + 7`.

Note:

- For two robots at the index `i` and `j`, pair `(i,j)` and pair `(j,i)` are considered the same pair.
- When robots collide, they __instantly change__ their directions without wasting any time.
- Collision happens when two robots share the same place in a moment.

  - For example, if a robot is positioned in 0 going to the right and another is positioned in 2 going to the left, the next second they'll be both in 1 and they will change direction and the next second the first one will be in 0, heading left, and another will be in 2, heading right.
  - For example, if a robot is positioned in 0 going to the right and another is positioned in 1 going to the left, the next second the first one will be in 0, heading left, and another will be in 1, heading right.

__Example:__

Example 1:

```text
Input: nums = [-2,0,2], s = "RLL", d = 3
Output: 8
Explanation: 
After 1 second, the positions are [-1,-1,1]. Now, the robot at index 0 will move left, and the robot at index 1 will move right.
After 2 seconds, the positions are [-2,0,0]. Now, the robot at index 1 will move left, and the robot at index 2 will move right.
After 3 seconds, the positions are [-3,-1,1].
The distance between the robot at index 0 and 1 is abs(-3 - (-1)) = 2.
The distance between the robot at index 0 and 2 is abs(-3 - 1) = 4.
The distance between the robot at index 1 and 2 is abs(-1 - 1) = 2.
The sum of the pairs of all distances = 2 + 4 + 2 = 8.
```

Example 2:

```text
Input: nums = [1,0], s = "RL", d = 2
Output: 5
Explanation: 
After 1 second, the positions are [2,-1].
After 2 seconds, the positions are [3,-2].
The distance between the two robots is abs(-2 - 3) = 5.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `-2 * 10 ^ 9 <= nums[i] <= 2 * 10 ^ 9`
- `0 <= d <= 10 ^ 9`
- `nums.length == s.length`
- `s` consists of 'L' and 'R' only
- `nums[i]` will be unique.

__题目描述:__

有一些机器人分布在一条无限长的数轴上，他们初始坐标用一个下标从 __0__ 开始的整数数组 `nums` 表示。当你给机器人下达命令时，它们以每秒钟一单位的速度开始移动。

给你一个字符串 `s` ，每个字符按顺序分别表示每个机器人移动的方向。 `'L'` 表示机器人往左或者数轴的负方向移动， `'R'` 表示机器人往右或者数轴的正方向移动。

当两个机器人相撞时，它们开始沿着原本相反的方向移动。

请你返回指令重复执行 `d` 秒后，所有机器人之间两两距离之和。由于答案可能很大，请你将答案对 `10 ^ 9 + 7` 取余后返回。

注意：

- 对于坐标在 `i` 和 `j` 的两个机器人， `(i,j)` 和 `(j,i)` 视为相同的坐标对。也就是说，机器人视为无差别的。
- 当机器人相撞时，它们 __立即改变__ 它们的前进方向，这个过程不消耗任何时间。

- 当两个机器人在同一时刻占据相同的位置时，就会相撞。

  - 例如，如果一个机器人位于位置 0 并往右移动，另一个机器人位于位置 2 并往左移动，下一秒，它们都将占据位置 1，并改变方向。再下一秒钟后，第一个机器人位于位置 0 并往左移动，而另一个机器人位于位置 2 并往右移动。
  - 例如，如果一个机器人位于位置 0 并往右移动，另一个机器人位于位置 1 并往左移动，下一秒，第一个机器人位于位置 0 并往左行驶，而另一个机器人位于位置 1 并往右移动。

__示例:__

示例 1：

```text
输入：nums = [-2,0,2], s = "RLL", d = 3
输出：8
解释：
1 秒后，机器人的位置为 [-1,-1,1] 。现在下标为 0 的机器人开始往左移动，下标为 1 的机器人开始往右移动。
2 秒后，机器人的位置为 [-2,0,0] 。现在下标为 1 的机器人开始往左移动，下标为 2 的机器人开始往右移动。
3 秒后，机器人的位置为 [-3,-1,1] 。
下标为 0 和 1 的机器人之间距离为 abs(-3 - (-1)) = 2 。
下标为 0 和 2 的机器人之间的距离为 abs(-3 - 1) = 4 。
下标为 1 和 2 的机器人之间的距离为 abs(-1 - 1) = 2 。
所有机器人对之间的总距离为 2 + 4 + 2 = 8 。
```

示例 2：

```text
输入：nums = [1,0], s = "RL", d = 2
输出：5
解释：
1 秒后，机器人的位置为 [2,-1] 。
2 秒后，机器人的位置为 [3,-2] 。
两个机器人的距离为 abs(-2 - 3) = 5 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `-2 * 10 ^ 9 <= nums[i] <= 2 * 10 ^ 9`
- `0 <= d <= 10 ^ 9`
- `nums.length == s.length`
- `s` 只包含 `'L'` 和 `'R'` 。
- `nums[i]` 互不相同。

__思路:__

```text
贪心 ➕ 贡献法
不需要考虑是否相撞
撞了之后两个机器人交换身份继续移动就行
最后计算的时候并不需要考虑初始位置是对应那个机器人
计算出每个机器人最后停留的位置
为了快速计算绝对值之差
将位置进行排序
最后结果为 r[1] - r[0] + r[2] - r[0] + ... + r[n - 1] - r[0] + r[2] - r[1] + ... + r[n - 1] - r[1] + ... + r[n - 1] - r[n - 2]
对每个 r[i] 有 r[i] - r[0] + r[i] - r[1] + ... + r[i] - r[i - 1]
即 i * r[i] - sum(r[:i])
所以可以一边遍历一边计算出总和
时间复杂度为 O(NlogN), 空间复杂度为 O(1), 主要开销是排序, 可以原地在 nums 上修改使得空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumDistance(vector<int>& nums, string s, int d) 
    {
        long long MOD = 1e9 + 7LL, result = 0LL, cur = 0LL;
        int n = nums.size();
        vector<long long> r(n);
        for (int i = 0; i < n; i++) r[i] = (long long)nums[i] + (s[i] == 'L' ? -d : d);
        sort(r.begin(), r.end());
        for (int i = 0; i < n; i++) 
        {
            result = (result + i * r[i] - cur) % MOD;
            cur += r[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumDistance(int[] nums, String s, int d) {
        long MOD = 1_000_000_007L, r[] = new long[nums.length], result = 0L, cur = 0L;
        for (int i = 0, n = nums.length; i < n; i++) r[i] = (long)nums[i] + (s.charAt(i) == 'L' ? -d : d);
        Arrays.sort(r);
        for (int i = 0, n = nums.length; i < n; i++) {
            result = (result + i * r[i] - cur) % MOD;
            cur += r[i];
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumDistance(self, nums: List[int], s: str, d: int) -> int:
        return sum(num * (i - (len(nums) - i - 1)) for i, num in enumerate(nums)) % (10 ** 9 + 7) if (nums := sorted([num + (d if s[i] == 'R' else -d) for i, num in enumerate(nums)])) else 0
```
