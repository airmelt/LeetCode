# 2079 Watering Plants 给植物浇水

__Description:__

You want to water `n` plants in your garden with a watering can. The plants are arranged in a row and are labeled from `0` to `n - 1` from left to right where the `i ^ th` plant is located at `x = i`. There is a river at `x = -1` that you can refill your watering can at.

Each plant needs a specific amount of water. You will water the plants in the following way:

- Water the plants in order from left to right.
- After watering the current plant, if you do not have enough water to __completely__ water the next plant, return to the river to fully refill the watering can.
- You __cannot__ refill the watering can early.

You are initially at the river (i.e., `x = -1`). It takes __one step__ to move __one unit__ on the x-axis.

Given a __0-indexed__ integer array `plants` of `n` integers, where `plants[i]` is the amount of water the `i ^ th` plant needs, and an integer `capacity` representing the watering can capacity, return _the __number of steps__ needed to water all the plants_.

__Example:__

Example 1:

```text
Input: plants = [2,2,3,3], capacity = 5
Output: 14
Explanation: Start at the river with a full watering can:
```

- Walk to plant 0 (1 step) and water it. Watering can has 3 units of water.
- Walk to plant 1 (1 step) and water it. Watering can has 1 unit of water.
- Since you cannot completely water plant 2, walk back to the river to refill (2 steps).
- Walk to plant 2 (3 steps) and water it. Watering can has 2 units of water.
- Since you cannot completely water plant 3, walk back to the river to refill (3 steps).
- Walk to plant 3 (4 steps) and water it.

Steps needed = 1 + 1 + 2 + 3 + 3 + 4 = 14.

Example 2:

```text
Input: plants = [1,1,1,4,2,3], capacity = 4
Output: 30
Explanation: Start at the river with a full watering can:
```

- Water plants 0, 1, and 2 (3 steps). Return to river (3 steps).
- Water plant 3 (4 steps). Return to river (4 steps).
- Water plant 4 (5 steps). Return to river (5 steps).
- Water plant 5 (6 steps).

Steps needed = 3 + 3 + 4 + 4 + 5 + 5 + 6 = 30.

Example 3:

```text
Input: plants = [7,7,7,7,7,7,7], capacity = 8
Output: 49
Explanation: You have to refill before watering each plant.
Steps needed = 1 + 1 + 2 + 2 + 3 + 3 + 4 + 4 + 5 + 5 + 6 + 6 + 7 = 49.
```

__Constraints:__

- `n == plants.length`
- `1 <= n <= 1000`
- `1 <= plants[i] <= 10 ^ 6`
- `max(plants[i]) <= capacity <= 10 ^ 9`

__题目描述:__

你打算用一个水罐给花园里的 `n` 株植物浇水。植物排成一行，从左到右进行标记，编号从 `0` 到 `n - 1` 。其中，第 `i` 株植物的位置是 `x = i` 。 `x = -1` 处有一条河，你可以在那里重新灌满你的水罐。

每一株植物都需要浇特定量的水。你将会按下面描述的方式完成浇水：

- 按从左到右的顺序给植物浇水。
- 在给当前植物浇完水之后，如果你没有足够的水 __完全__ 浇灌下一株植物，那么你就需要返回河边重新装满水罐。
- 你 __不能__ 提前重新灌满水罐。

最初，你在河边（也就是， `x = -1`），在 x 轴上每移动 __一个单位__ 都需要 __一步__ 。

给你一个下标从 __0__ 开始的整数数组 `plants` ，数组由 `n` 个整数组成。其中， `plants[i]` 为第 `i` 株植物需要的水量。另有一个整数 `capacity` 表示水罐的容量，返回浇灌所有植物需要的 __步数__ 。

__示例:__

示例 1：

```text
输入：plants = [2,2,3,3], capacity = 5
输出：14
解释：从河边开始，此时水罐是装满的：
```

- 走到植物 0 (1 步) ，浇水。水罐中还有 3 单位的水。
- 走到植物 1 (1 步) ，浇水。水罐中还有 1 单位的水。
- 由于不能完全浇灌植物 2 ，回到河边取水 (2 步)。
- 走到植物 2 (3 步) ，浇水。水罐中还有 2 单位的水。
- 由于不能完全浇灌植物 3 ，回到河边取水 (3 步)。
- 走到植物 3 (4 步) ，浇水。

需要的步数是 = 1 + 1 + 2 + 3 + 3 + 4 = 14 。

示例 2：

```text
输入：plants = [1,1,1,4,2,3], capacity = 4
输出：30
解释：从河边开始，此时水罐是装满的：
```

- 走到植物 0，1，2 (3 步) ，浇水。回到河边取水 (3 步)。
- 走到植物 3 (4 步) ，浇水。回到河边取水 (4 步)。
- 走到植物 4 (5 步) ，浇水。回到河边取水 (5 步)。
- 走到植物 5 (6 步) ，浇水。

需要的步数是 = 3 + 3 + 4 + 4 + 5 + 5 + 6 = 30 。

示例 3：

```text
输入：plants = [7,7,7,7,7,7,7], capacity = 8
输出：49
解释：每次浇水都需要重新灌满水罐。
需要的步数是 = 1 + 1 + 2 + 2 + 3 + 3 + 4 + 4 + 5 + 5 + 6 + 6 + 7 = 49 。
```

__提示：__

- `n == plants.length`
- `1 <= n <= 1000`
- `1 <= plants[i] <= 10 ^ 6`
- `max(plants[i]) <= capacity <= 10 ^ 9`

__思路:__

```text
模拟
至少要走 plants 的长度
如果当前的水不够就要走 i 的两倍的距离并把水装满
否则就减去当前植物的水量
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int wateringPlants(vector<int>& plants, int capacity) 
    {
        int n = plants.size(), result = n, water = capacity;
        for (int i = 0; i < n; i++)
        {
            if (water < plants[i])
            {
                result += i << 1;
                water = capacity;
            }
            water -= plants[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int wateringPlants(int[] plants, int capacity) {
        int n = plants.length, result = n, water = capacity;
        for (int i = 0; i < n; i++)
        {
            if (water < plants[i])
            {
                result += i << 1;
                water = capacity;
            }
            water -= plants[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def wateringPlants(self, plants: List[int], capacity: int) -> int:
        result, water = len(plants), capacity
        for i, p in enumerate(plants):
            if water < p:
                result += i << 1
                water = capacity
            water -= p
        return result
```
