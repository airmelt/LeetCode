# 1482 Minimum Number of Days to Make m Bouquets 制作m束花所需的最少天数

__Description:__

You are given an integer array  `bloomDay`, an integer  `m` and an integer  `k`.

You want to make  `m` bouquets. To make a bouquet, you need to use  `k` __adjacent flowers__ from the garden.

The garden consists of  `n` flowers, the  `i ^ th` flower will bloom in the  `bloomDay[i]` and then can be used in __exactly one__ bouquet.

Return _the minimum number of days you need to wait to be able to make_  `m` _bouquets from the garden_. If it is impossible to make m bouquets return  `-1`.

__Example:__

Example 1:

```text
Input: bloomDay = [1,10,3,10,2], m = 3, k = 1
Output: 3
Explanation: Let us see what happened in the first three days. x means flower bloomed and _ means flower did not bloom in the garden.
We need 3 bouquets each should contain 1 flower.
After day 1: [x, _, _, _, _]   // we can only make one bouquet.
After day 2: [x, _, _, _, x]   // we can only make two bouquets.
After day 3: [x, _, x, _, x]   // we can make 3 bouquets. The answer is 3.
```

Example 2:

```text
Input: bloomDay = [1,10,3,10,2], m = 3, k = 2
Output: -1
Explanation: We need 3 bouquets each has 2 flowers, that means we need 6 flowers. We only have 5 flowers so it is impossible to get the needed bouquets and we return -1.
```

Example 3:

```text
Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
Output: 12
Explanation: We need 2 bouquets each should have 3 flowers.
Here is the garden after the 7 and 12 days:
After day 7: [x, x, x, x, _, x, x]
We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent.
After day 12: [x, x, x, x, x, x, x]
It is obvious that we can make two bouquets in different ways.
```

__Constraints:__

- `bloomDay.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= bloomDay[i] <= 10 ^ 9`
- `1 <= m <= 10 ^ 6`
- `1 <= k <= n`

__题目描述:__

给你一个整数数组  `bloomDay`，以及两个整数  `m` 和  `k` 。

现需要制作  `m` 束花。制作花束时，需要使用花园中 __相邻的  `k` 朵花__ 。

花园中有  `n` 朵花，第  `i` 朵花会在  `bloomDay[i]` 时盛开，__恰好__ 可以用于 __一束__ 花中。

请你返回从花园中摘  `m` 束花需要等待的最少的天数。如果不能摘到  `m` 束花则返回 __-1__ 。

__示例:__

示例 1：

```text
输入：bloomDay = [1,10,3,10,2], m = 3, k = 1
输出：3
解释：让我们一起观察这三天的花开过程，x 表示花开，而 _ 表示花还未开。
现在需要制作 3 束花，每束只需要 1 朵。
1 天后：[x, _, _, _, _]   // 只能制作 1 束花
2 天后：[x, _, _, _, x]   // 只能制作 2 束花
3 天后：[x, _, x, _, x]   // 可以制作 3 束花，答案为 3
```

示例 2：

```text
输入：bloomDay = [1,10,3,10,2], m = 3, k = 2
输出：-1
解释：要制作 3 束花，每束需要 2 朵花，也就是一共需要 6 朵花。而花园中只有 5 朵花，无法满足制作要求，返回 -1 。
```

示例 3：

```text
输入：bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
输出：12
解释：要制作 2 束花，每束需要 3 朵。
花园在 7 天后和 12 天后的情况如下：
7 天后：[x, x, x, x, _, x, x]
可以用前 3 朵盛开的花制作第一束花。但不能使用后 3 朵盛开的花，因为它们不相邻。
12 天后：[x, x, x, x, x, x, x]
显然，我们可以用不同的方式制作两束花。
```

示例 4：

```text
输入：bloomDay = [1000000000,1000000000], m = 1, k = 1
输出：1000000000
解释：需要等 1000000000 天才能采到花来制作花束
```

示例 5：

```text
输入：bloomDay = [1,10,2,9,3,8,4,7,5,6], m = 4, k = 2
输出：9
```

__提示：__

- `bloomDay.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= bloomDay[i] <= 10 ^ 9`
- `1 <= m <= 10 ^ 6`
- `1 <= k <= n`

__思路:__

```text
二分查找
用一个辅助函数计算是否能够制造足够的花束
然后用二分查找缩小范围
时间复杂度为 O(Nlog(high - low)), 空间复杂度为 O(1), high 表示数组的最大值, low 表示数组的最小值
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    bool helper(int days, vector<int>& bloomDay, int m, int k) 
    {
        int bouquets = 0, flowers = 0;
        for (int bloom: bloomDay) 
        {
            if (bloom <= days) 
            {
                if (++flowers == k) 
                {
                    if (++bouquets == m) break;
                    flowers = 0;
                }
            }
            else flowers = 0;
        }
        return bouquets == m;
    }
public:
    int minDays(vector<int>& bloomDay, int m, int k) 
    {
        if ((long)m * k > bloomDay.size()) return -1;
        int low = *min_element(bloomDay.begin(), bloomDay.end()), high = *max_element(bloomDay.begin(), bloomDay.end());
        while (low < high) 
        {
            int days = low + ((high - low) >> 1);
            if (helper(days, bloomDay, m, k)) high = days;
            else low = ++days;
        }
        return low;
    }
};
```

__Java__:

```Java
class Solution {
    public int minDays(int[] bloomDay, int m, int k) {
        if ((long)m * k > bloomDay.length) return -1;
        int low = bloomDay[0], high = bloomDay[0];
        for (int day: bloomDay) {
            low = Math.min(day, low);
            high = Math.max(day, high);
        }
        while (low < high) {
            int days = low + ((high - low) >> 1);
            if (helper(days, bloomDay, m, k)) high = days;
            else low = ++days;
        }
        return low;
    }
    
    private boolean helper(int days, int[] bloomDay, int m, int k) {
        int bouquets = 0, flowers = 0;
        for (int bloom: bloomDay) {
            if (bloom <= days) {
                if (++flowers == k) {
                    if (++bouquets == m) break;
                    flowers = 0;
                }
            }
            else flowers = 0;
        }
        return bouquets == m;
    }
}
```

__Python__:

```Python
class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        if m * k > len(bloomDay):
            return -1
        def helper(days: int) -> bool:
            bouquets = flowers = 0
            for bloom in bloomDay:
                if bloom <= days:
                    flowers += 1
                    if flowers == k:
                        bouquets += 1
                        if bouquets == m:
                            break
                        flowers = 0
                else:
                    flowers = 0
            return bouquets == m
        low, high = min(bloomDay), max(bloomDay)
        while low < high:
            days = (low + high) >> 1
            if helper(days):
                high = days
            else:
                low = days + 1
        return low
```
