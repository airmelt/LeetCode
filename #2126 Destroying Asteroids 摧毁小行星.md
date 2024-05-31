# 2126 Destroying Asteroids 摧毁小行星

__Description:__

You are given an integer `mass`, which represents the original mass of a planet. You are further given an integer array `asteroids`, where `asteroids[i]` is the mass of the `i ^ th` asteroid.

You can arrange for the planet to collide with the asteroids in any arbitrary order. If the mass of the planet is greater than or equal to the mass of the asteroid, the asteroid is destroyed and the planet gains the mass of the asteroid. Otherwise, the planet is destroyed.

Return `true` _if __all__ asteroids can be destroyed. Otherwise, return_ `false`_._

__Example:__

Example 1:

```text
Input: mass = 10, asteroids = [3,9,19,5,21]
Output: true
Explanation: One way to order the asteroids is [9,19,5,3,21]:
```

- The planet collides with the asteroid with a mass of 9. New planet mass: 10 + 9 = 19
- The planet collides with the asteroid with a mass of 19. New planet mass: 19 + 19 = 38
- The planet collides with the asteroid with a mass of 5. New planet mass: 38 + 5 = 43
- The planet collides with the asteroid with a mass of 3. New planet mass: 43 + 3 = 46
- The planet collides with the asteroid with a mass of 21. New planet mass: 46 + 21 = 67

All asteroids are destroyed.

Example 2:

```text
Input: mass = 5, asteroids = [4,9,23,4]
Output: false
Explanation: 
The planet cannot ever gain enough mass to destroy the asteroid with a mass of 23.
After the planet destroys the other asteroids, it will have a mass of 5 + 4 + 9 + 4 = 22.
This is less than 23, so a collision would not destroy the last asteroid.
```

__Constraints:__

- `1 <= mass <= 10 ^ 5`
- `1 <= asteroids.length <= 10 ^ 5`
- `1 <= asteroids[i] <= 10 ^ 5`

__题目描述:__

给你一个整数 `mass` ，它表示一颗行星的初始质量。再给你一个整数数组 `asteroids` ，其中 `asteroids[i]` 是第 `i` 颗小行星的质量。

你可以按 任意顺序 重新安排小行星的顺序，然后让行星跟它们发生碰撞。如果行星碰撞时的质量 大于等于 小行星的质量，那么小行星被 摧毁 ，并且行星会 获得 这颗小行星的质量。否则，行星将被摧毁。

如果所有小行星 __都__ 能被摧毁，请返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：mass = 10, asteroids = [3,9,19,5,21]
输出：true
解释：一种安排小行星的方式为 [9,19,5,3,21] ：
```

- 行星与质量为 9 的小行星碰撞。新的行星质量为：10 + 9 = 19
- 行星与质量为 19 的小行星碰撞。新的行星质量为：19 + 19 = 38
- 行星与质量为 5 的小行星碰撞。新的行星质量为：38 + 5 = 43
- 行星与质量为 3 的小行星碰撞。新的行星质量为：43 + 3 = 46
- 行星与质量为 21 的小行星碰撞。新的行星质量为：46 + 21 = 67

所有小行星都被摧毁。

示例 2：

```text
输入：mass = 5, asteroids = [4,9,23,4]
输出：false
解释：
行星无论如何没法获得足够质量去摧毁质量为 23 的小行星。
行星把别的小行星摧毁后，质量为 5 + 4 + 9 + 4 = 22 。
它比 23 小，所以无法摧毁最后一颗小行星。
```

__提示：__

- `1 <= mass <= 10 ^ 5`
- `1 <= asteroids.length <= 10 ^ 5`
- `1 <= asteroids[i] <= 10 ^ 5`

__思路:__

```text
1. 贪心
由于小行星的位置与答案无关
优先撞击质量小的
对小行星排序
每次加上小行星的质量直到撞不动, 返回 False
否则可以撞完
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 数学
将所有小行星放入 [0, 1], [2, 4], [5, 8], 等等以 2 的 n 次幂的桶中
如果桶最小的值都比当前值大, 返回 False
否则可以加上桶中的质量之和
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool asteroidsDestroyed(int mass, vector<int>& asteroids) 
    {
        vector<int> min(17, -1);
        vector<long long> sum(17);
        for (int i = 0, n = asteroids.size(), h = 0; i < n; i++) 
        {
            if (min[h = 31 - __builtin_clz(asteroids[i])] == -1 or asteroids[i] < min[h]) min[h] = asteroids[i];
            sum[h] += asteroids[i];
        }
        long long cur = mass;
        for (int i = 0; i < 17; i++) 
        {
            if (cur < min[i]) return false;
            cur += sum[i];
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean asteroidsDestroyed(int mass, int[] asteroids) {
        Arrays.sort(asteroids);
        long cur = mass;
        for (int asteroid : asteroids) {
            if (cur < asteroid) return false;
            cur += asteroid;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def asteroidsDestroyed(self, mass: int, asteroids: List[int]) -> bool:
        return mass >= pre[0] and all(mass + pre[i - 1] >= pre[i] - pre[i - 1] for i in range(1, len(pre))) if (pre := list(accumulate(sorted(asteroids)))) else False
```
