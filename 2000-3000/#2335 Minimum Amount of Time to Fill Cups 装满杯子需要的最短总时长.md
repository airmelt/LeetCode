# 2335 Minimum Amount of Time to Fill Cups 装满杯子需要的最短总时长

__Description:__

You have a water dispenser that can dispense cold, warm, and hot water. Every second, you can either fill up `2` cups with __different__ types of water, or `1` cup of any type of water.

You are given a __0-indexed__ integer array `amount` of length `3` where `amount[0]`, `amount[1]`, and `amount[2]` denote the number of cold, warm, and hot water cups you need to fill respectively. Return _the __minimum__ number of seconds needed to fill up all the cups_.

__Example:__

Example 1:

```text
Input: amount = [1,4,2]
Output: 4
Explanation: One way to fill up the cups is:
Second 1: Fill up a cold cup and a warm cup.
Second 2: Fill up a warm cup and a hot cup.
Second 3: Fill up a warm cup and a hot cup.
Second 4: Fill up a warm cup.
It can be proven that 4 is the minimum number of seconds needed.
```

Example 2:

```text
Input: amount = [5,4,4]
Output: 7
Explanation: One way to fill up the cups is:
Second 1: Fill up a cold cup, and a hot cup.
Second 2: Fill up a cold cup, and a warm cup.
Second 3: Fill up a cold cup, and a warm cup.
Second 4: Fill up a warm cup, and a hot cup.
Second 5: Fill up a cold cup, and a hot cup.
Second 6: Fill up a cold cup, and a warm cup.
Second 7: Fill up a hot cup.
```

Example 3:

```text
Input: amount = [5,0,0]
Output: 5
Explanation: Every second, we fill up a cold cup.
```

__Constraints:__

- `amount.length == 3`
- `0 <= amount[i] <= 100`

__题目描述:__

现有一台饮水机，可以制备冷水、温水和热水。每秒钟，可以装满 `2` 杯 __不同__ 类型的水或者 `1` 杯任意类型的水。

给你一个下标从 __0__ 开始、长度为 `3` 的整数数组 `amount` ，其中 `amount[0]`、 `amount[1]` 和 `amount[2]` 分别表示需要装满冷水、温水和热水的杯子数量。返回装满所有杯子所需的 __最少__ 秒数。

__示例:__

示例 1：

```text
输入：amount = [1,4,2]
输出：4
解释：下面给出一种方案：
第 1 秒：装满一杯冷水和一杯温水。
第 2 秒：装满一杯温水和一杯热水。
第 3 秒：装满一杯温水和一杯热水。
第 4 秒：装满一杯温水。
可以证明最少需要 4 秒才能装满所有杯子。
```

示例 2：

```text
输入：amount = [5,4,4]
输出：7
解释：下面给出一种方案：
第 1 秒：装满一杯冷水和一杯热水。
第 2 秒：装满一杯冷水和一杯温水。
第 3 秒：装满一杯冷水和一杯温水。
第 4 秒：装满一杯温水和一杯热水。
第 5 秒：装满一杯冷水和一杯热水。
第 6 秒：装满一杯冷水和一杯温水。
第 7 秒：装满一杯热水。
```

示例 3：

```text
输入：amount = [5,0,0]
输出：5
解释：每秒装满一杯冷水。
```

__提示：__

- `amount.length == 3`
- `0 <= amount[i] <= 100`

__思路:__

```text
贪心
不妨设 x >= y >= z
如果 x >= y + z, 那么可以将其他的杯子都装满再填充 x 杯子, 此时返回 max(amount)
否则, x < y + z, 
如果, y + z - x 为偶数, 那么可以先将 y, z 各装 (y + z - x) // 2 杯水, 然后再同时填充 x 杯子和 y 或 z 这样刚好能填满, 此时需要 x + y + z // 2 秒
否则 y + z - x 为奇数, 那么可以先将 y, z 各装 (y + z - x - 1) // 2 杯水, 然后再同时填充 x 杯子和 y 或 z 最后一杯单独填充 y 或 z, 此时需要 x + y + z + 1 // 2 秒
其实就是尽可能的多使用 2 杯水填充, 如果剩下的杯子数为奇数, 那么需要单独填充一杯水
综上所述, 返回 max(max(amount), (sum(amount) + 1) // 2)
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int fillCups(vector<int>& amount) 
    {
        return max(*max_element(amount.begin(), amount.end()), (accumulate(amount.begin(), amount.end(), 0) + 1) >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int fillCups(int[] amount) {
        return Math.max(Arrays.stream(amount).max().getAsInt(), (Arrays.stream(amount).sum() + 1) >> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def fillCups(self, amount: List[int]) -> int:
        return max(max(amount), (sum(amount) + 1) >> 1)
```
