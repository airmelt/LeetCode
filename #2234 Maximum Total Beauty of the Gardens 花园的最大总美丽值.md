# 2234 Maximum Total Beauty of the Gardens 花园的最大总美丽值

__Description:__

Alice is a caretaker of `n` gardens and she wants to plant flowers to maximize the total beauty of all her gardens.

You are given a __0-indexed__ integer array `flowers` of size `n`, where `flowers[i]` is the number of flowers already planted in the `i ^ th` garden. Flowers that are already planted __cannot__ be removed. You are then given another integer `newFlowers`, which is the __maximum__ number of flowers that Alice can additionally plant. You are also given the integers `target`, `full`, and `partial`.

A garden is considered __complete__ if it has __at least__ `target` flowers. The __total beauty__ of the gardens is then determined as the __sum__ of the following:

- The number of __complete__ gardens multiplied by `full`.
- The __minimum__ number of flowers in any of the __incomplete__ gardens multiplied by `partial`. If there are no incomplete gardens, then this value will be `0`.

Return _the __maximum__ total beauty that Alice can obtain after planting at most_ `newFlowers` _flowers._

__Example:__

Example 1:

```text
Input: flowers = [1,3,1,1], newFlowers = 7, target = 6, full = 12, partial = 1
Output: 14
Explanation: Alice can plant
```

- 2 flowers in the 0th garden
- 3 flowers in the 1st garden
- 1 flower in the 2nd garden
- 1 flower in the 3rd garden

The gardens will then be [3,6,2,2]. She planted a total of 2 + 3 + 1 + 1 = 7 flowers.

There is 1 garden that is complete.

The minimum number of flowers in the incomplete gardens is 2.

Thus, the total beauty is 1 \* 12 + 2 \* 1 = 12 + 2 = 14.
No other way of planting flowers can obtain a total beauty higher than 14.

Example 2:

```text
Input: flowers = [2,4,5,3], newFlowers = 10, target = 5, full = 2, partial = 6
Output: 30
Explanation: Alice can plant
```

- 3 flowers in the 0th garden
- 0 flowers in the 1st garden
- 0 flowers in the 2nd garden
- 2 flowers in the 3rd garden

The gardens will then be [5,4,5,5]. She planted a total of 3 + 0 + 0 + 2 = 5 flowers.

There are 3 gardens that are complete.

The minimum number of flowers in the incomplete gardens is 4.

Thus, the total beauty is 3 \* 2 + 4 \* 6 = 6 + 24 = 30.

No other way of planting flowers can obtain a total beauty higher than 30.

Note that Alice could make all the gardens complete but in this case, she would obtain a lower total beauty.

__Constraints:__

- `1 <= flowers.length <= 10 ^ 5`
- `1 <= flowers[i], target <= 10 ^ 5`
- `1 <= newFlowers <= 10 ^ 10`
- `1 <= full, partial <= 10 ^ 5`

__题目描述:__

Alice 是 `n` 个花园的园丁，她想通过种花，最大化她所有花园的总美丽值。

给你一个下标从 __0__ 开始大小为 `n` 的整数数组 `flowers` ，其中 `flowers[i]` 是第 `i` 个花园里已经种的花的数目。已经种了的花 __不能__ 移走。同时给你 `newFlowers` ，表示 Alice 额外可以种花的 __最大数目__ 。同时给你的还有整数 `target` ， `full` 和 `partial` 。

如果一个花园有 __至少__ `target` 朵花，那么这个花园称为 __完善的__ ，花园的 __总美丽值__ 为以下分数之 __和__ :

- _完善_ 花园数目乘以 `full`.
- 剩余 __不完善__ 花园里，花的 __最少数目__ 乘以 `partial` 。如果没有不完善花园，那么这一部分的值为 `0` 。

请你返回 Alice 种最多 `newFlowers` 朵花以后，能得到的 __最大__ 总美丽值。

__示例:__

示例 1：

```text
输入：flowers = [1,3,1,1], newFlowers = 7, target = 6, full = 12, partial = 1
输出：14
解释：Alice 可以按以下方案种花
```

- 在第 0 个花园种 2 朵花
- 在第 1 个花园种 3 朵花
- 在第 2 个花园种 1 朵花
- 在第 3 个花园种 1 朵花

花园里花的数目为 [3,6,2,2] 。总共种了 2 + 3 + 1 + 1 = 7 朵花。

只有 1 个花园是完善的。

不完善花园里花的最少数目是 2 。

所以总美丽值为 1 \* 12 + 2 \* 1 = 12 + 2 = 14 。

没有其他方案可以让花园总美丽值超过 14 。

示例 2：

```text
输入：flowers = [2,4,5,3], newFlowers = 10, target = 5, full = 2, partial = 6
输出：30
解释：Alice 可以按以下方案种花
```

- 在第 0 个花园种 3 朵花
- 在第 1 个花园种 0 朵花
- 在第 2 个花园种 0 朵花
- 在第 3 个花园种 2 朵花

花园里花的数目为 [5,4,5,5] 。总共种了 3 + 0 + 0 + 2 = 5 朵花。

有 3 个花园是完善的。

不完善花园里花的最少数目为 4 。

所以总美丽值为 3 \* 2 + 4 \* 6 = 6 + 24 = 30 。

没有其他方案可以让花园总美丽值超过 30 。

注意，Alice可以让所有花园都变成完善的，但这样她的总美丽值反而更小。

__提示：__

- `1 <= flowers.length <= 10 ^ 5`
- `1 <= flowers[i], target <= 10 ^ 5`
- `1 <= newFlowers <= 10 ^ 10`
- `1 <= full, partial <= 10 ^ 5`

__思路:__

```text
贪心
先对花园进行排序
如果花园里的花朵数目都大于等于 target, 那么直接返回 n * full
正常思路是逆序遍历花园, 从最大的花园开始种花, 直到所有花园都种到完善的状态, 或者将 flowers[i] 之后的都种满, 而不种 flowers[i] 之前的并且使得 flowers[i] 之前的花园的花朵数尽可能接近 target - 1
计算 flowers[i] 之前的可以用前缀和优化
用二分法可以找到填充的值从而使得前缀里的最小值尽可能大
记 left = newFlowers - target * n 为将所有花园填充还剩下的可以种植的花朵数目
已经有的花朵数目为 min(flowers[i], target), 超过 target 的花朵数目都是多余的
left 加上所有花园的花朵数目
填充之后所有前缀和的最大值为 left + pre / x 和 target - 1 的较小值
这一部分即为 partial 的美丽值
如果 left 不为负数
说明还可以种树
找到最大的 x 使得 (flowers[x] * x - pre) <= left
则 flowers[x] 之前的花园都可以种满
更新美丽值
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要开销在排序上
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumBeauty(vector<int>& flowers, long long newFlowers, int target, int full, int partial) 
    {
        sort(flowers.begin(), flowers.end());
        long long n = flowers.size(), result = 0LL, pre = 0LL, left = newFlowers - target * n;
        if (flowers.front() >= target) return n * full;
        for (int i = 0; i < n; i++) left += (flowers[i] = min(flowers[i], target));
        for (int i = 0, x = 0; i <= n; left += target - flowers[i < n ? i : i - 1], i++) 
        {
            if (left > -1) 
            {
                while (x < i and (long long)flowers[x] * x - pre <= left) pre += flowers[x++];
                result = max(result, (n - i) * full + (x ? min((left + pre) / x, target - 1LL) * partial : 0LL));
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumBeauty(int[] flowers, long newFlowers, int target, int full, int partial) {
        Arrays.sort(flowers);
        long n = flowers.length, result = 0L, pre = 0L, left = newFlowers - target * n;
        if (flowers[0] >= target) return n * full;
        for (int i = 0; i < n; i++) left += (flowers[i] = Math.min(flowers[i], target));
        for (int i = 0, x = 0; i <= n; left += target - flowers[i < n ? i : i - 1], i++) {
            if (left > -1) {
                while (x < i && (long)flowers[x] * x - pre <= left) pre += flowers[x++];
                result = Math.max(result, (n - i) * full + (x == 0 ? 0L : Math.min((left + pre) / x, target - 1L) * partial));
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumBeauty(self, flowers: List[int], newFlowers: int, target: int, full: int, partial: int) -> int:
        flowers.sort()
        left, result, x, pre = newFlowers - target * (n := len(flowers)), 0, 0, 0
        if flowers[0] >= target:
            return n * full
        for i in range(n):
            flowers[i] = min(flowers[i], target)
            left += flowers[i]
        for i in range(n + 1):
            if left > -1:
                while x < i and flowers[x] * x - pre <= left:
                    pre += flowers[x]
                    x += 1
                result = max(result, (n - i) * full + (not not x) * min((left + pre) // max(x, 1), target - 1) * partial)
            if i < n:
                left += target - flowers[i]
        return result
```
