# 2136 Earliest Possible Day of Full Bloom 全部开花的最早一天

__Description:__

You have `n` flower seeds. Every seed must be planted first before it can begin to grow, then bloom. Planting a seed takes time and so does the growth of a seed. You are given two __0-indexed__ integer arrays `plantTime` and `growTime`, of length `n` each:

- `plantTime[i]` is the number of __full days__ it takes you to __plant__ the `i ^ th` seed. Every day, you can work on planting exactly one seed. You __do not__ have to work on planting the same seed on consecutive days, but the planting of a seed is not complete __until__ you have worked `plantTime[i]` days on planting it in total.
- `growTime[i]` is the number of __full days__ it takes the `i ^ th` seed to grow after being completely planted. __After__ the last day of its growth, the flower __blooms__ and stays bloomed forever.

From the beginning of day `0`, you can plant the seeds in __any__ order.

Return the earliest possible day where all seeds are blooming.

__Example:__

Example 1:

![2136-1](https://assets.leetcode.com/uploads/2021/12/21/1.png)

```text
Input: plantTime = [1,4,3], growTime = [2,3,1]
Output: 9
Explanation: The grayed out pots represent planting days, colored pots represent growing days, and the flower represents the day it blooms.
One optimal way is:
On day 0, plant the 0th seed. The seed grows for 2 full days and blooms on day 3.
On days 1, 2, 3, and 4, plant the 1st seed. The seed grows for 3 full days and blooms on day 8.
On days 5, 6, and 7, plant the 2nd seed. The seed grows for 1 full day and blooms on day 9.
Thus, on day 9, all the seeds are blooming.
```

Example 2:

![2136-2](https://assets.leetcode.com/uploads/2021/12/21/2.png)

```text
Input: plantTime = [1,2,3,2], growTime = [2,1,2,1]
Output: 9
Explanation: The grayed out pots represent planting days, colored pots represent growing days, and the flower represents the day it blooms.
One optimal way is:
On day 1, plant the 0th seed. The seed grows for 2 full days and blooms on day 4.
On days 0 and 3, plant the 1st seed. The seed grows for 1 full day and blooms on day 5.
On days 2, 4, and 5, plant the 2nd seed. The seed grows for 2 full days and blooms on day 8.
On days 6 and 7, plant the 3rd seed. The seed grows for 1 full day and blooms on day 9.
Thus, on day 9, all the seeds are blooming.
```

Example 3:

```text
Input: plantTime = [1], growTime = [1]
Output: 2
Explanation: On day 0, plant the 0th seed. The seed grows for 1 full day and blooms on day 2.
Thus, on day 2, all the seeds are blooming.
```

__Constraints:__

- `n == plantTime.length == growTime.length`
- `1 <= n <= 10 ^ 5`
- `1 <= plantTime[i], growTime[i] <= 10 ^ 4`

__题目描述:__

你有 `n` 枚花的种子。每枚种子必须先种下，才能开始生长、开花。播种需要时间，种子的生长也是如此。给你两个下标从 __0__ 开始的整数数组 `plantTime` 和 `growTime` ，每个数组的长度都是 `n` :

- `plantTime[i]` 是 __播种__ 第 `i` 枚种子所需的 __完整天数__ 。每天，你只能为播种某一枚种子而劳作。__无须__ 连续几天都在种同一枚种子，但是种子播种必须在你工作的天数达到 `plantTime[i]` 之后才算完成。
- `growTime[i]` 是第 `i` 枚种子完全种下后生长所需的 __完整天数__ 。在它生长的最后一天 __之后__ ，将会开花并且永远 __绽放__ 。

从第 `0` 开始，你可以按 __任意__ 顺序播种种子。

返回所有种子都开花的 最早 一天是第几天。

__示例:__

示例 1：

![2136-3](https://assets.leetcode.com/uploads/2021/12/21/1.png)

```text
输入：plantTime = [1,4,3], growTime = [2,3,1]
输出：9
解释：灰色的花盆表示播种的日子，彩色的花盆表示生长的日子，花朵表示开花的日子。
一种最优方案是：
第 0 天，播种第 0 枚种子，种子生长 2 整天。并在第 3 天开花。
第 1、2、3、4 天，播种第 1 枚种子。种子生长 3 整天，并在第 8 天开花。
第 5、6、7 天，播种第 2 枚种子。种子生长 1 整天，并在第 9 天开花。
因此，在第 9 天，所有种子都开花。
```

示例 2：

![2136-4](https://assets.leetcode.com/uploads/2021/12/21/2.png)

```text
输入：plantTime = [1,2,3,2], growTime = [2,1,2,1]
输出：9
解释：灰色的花盆表示播种的日子，彩色的花盆表示生长的日子，花朵表示开花的日子。 
一种最优方案是：
第 1 天，播种第 0 枚种子，种子生长 2 整天。并在第 4 天开花。
第 0、3 天，播种第 1 枚种子。种子生长 1 整天，并在第 5 天开花。
第 2、4、5 天，播种第 2 枚种子。种子生长 2 整天，并在第 8 天开花。
第 6、7 天，播种第 3 枚种子。种子生长 1 整天，并在第 9 天开花。
因此，在第 9 天，所有种子都开花。
```

示例 3：

```text
输入：plantTime = [1], growTime = [1]
输出：2
解释：第 0 天，播种第 0 枚种子。种子需要生长 1 整天，然后在第 2 天开花。
因此，在第 2 天，所有种子都开花。
```

__提示：__

- `n == plantTime.length == growTime.length`
- `1 <= n <= 10 ^ 5`
- `1 <= plantTime[i], growTime[i] <= 10 ^ 4`

__思路:__

```text
贪心 ➕ 排序
无论如何都要等所有的种子都开花了才算完成
即需要加上所有种子的播种时间, 即 sum(plantTime)
为了让种子充分利用播种时间, 优先种 growTime 长的种子
将 growTime 排序, 对应种下 plantTime 的种子
遍历种子, 计算每个种子的开花时间, 并更新最大值 max(result, sum(plantTime[:i]) + growTime[i])
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int earliestFullBloom(vector<int>& plantTime, vector<int>& growTime) 
    {
        for (auto it = growTime.cbegin(); int& i : plantTime) i |= *it++ << 16;
        ranges::sort(plantTime);
        return accumulate(plantTime.crbegin(), plantTime.crend(), 0, [now = 0](int grow, int i)mutable { now += i & 0xffff; return max(grow, now + (i >> 16)); });
    }
};
```

__Java__:

```Java
class Solution {
    public int earliestFullBloom(int[] plantTime, int[] growTime) {
        int result = 0, days = 0, n = plantTime.length, seed[][] = new int[n][2];
        for (int i = 0; i < n; i++) {
            seed[i][0] = -growTime[i];
            seed[i][1] = plantTime[i];
        }
        Arrays.sort(seed, (a, b) -> a[0] - b[0]);
        for (int i = 0; i < n; i++) {
            days += seed[i][1];
            result = Math.max(result, -seed[i][0] + days);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def earliestFullBloom(self, plantTime: List[int], growTime: List[int]) -> int:
        result = days = 0
        for p, g in sorted(zip(plantTime, growTime), key=lambda z: -z[1]):
            days += p
            result = max(result, days + g)
        return result
```
