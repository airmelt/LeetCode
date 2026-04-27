# 2105 Watering Plants II 给植物浇水 II

__Description:__

Alice and Bob want to water `n` plants in their garden. The plants are arranged in a row and are labeled from `0` to `n - 1` from left to right where the `i ^ th` plant is located at `x = i`.

Each plant needs a specific amount of water. Alice and Bob have a watering can each, initially full. They water the plants in the following way:

- Alice waters the plants in order from __left to right__, starting from the `0 ^ th` plant. Bob waters the plants in order from __right to left__, starting from the `(n - 1) ^ th` plant. They begin watering the plants __simultaneously__.
- It takes the same amount of time to water each plant regardless of how much water it needs.
- Alice/Bob __must__ water the plant if they have enough in their can to __fully__ water it. Otherwise, they __first__ refill their can (instantaneously) then water the plant.
- In case both Alice and Bob reach the same plant, the one with __more__ water currently in his/her watering can should water this plant. If they have the same amount of water, then Alice should water this plant.

Given a __0-indexed__ integer array `plants` of `n` integers, where `plants[i]` is the amount of water the `i ^ th` plant needs, and two integers `capacityA` and `capacityB` representing the capacities of Alice's and Bob's watering cans respectively, return _the __number of times__ they have to refill to water all the plants_.

__Example:__

Example 1:

```text
Input: plants = [2,2,3,3], capacityA = 5, capacityB = 5
Output: 1
Explanation:
```

- Initially, Alice and Bob have 5 units of water each in their watering cans.
- Alice waters plant 0, Bob waters plant 3.
- Alice and Bob now have 3 units and 2 units of water respectively.
- Alice has enough water for plant 1, so she waters it. Bob does not have enough water for plant 2, so he refills his can then waters it.

So, the total number of times they have to refill to water all the plants is 0 + 0 + 1 + 0 = 1.

Example 2:

```text
Input: plants = [2,2,3,3], capacityA = 3, capacityB = 4
Output: 2
Explanation:
```

- Initially, Alice and Bob have 3 units and 4 units of water in their watering cans respectively.
- Alice waters plant 0, Bob waters plant 3.
- Alice and Bob now have 1 unit of water each, and need to water plants 1 and 2 respectively.
- Since neither of them have enough water for their current plants, they refill their cans and then water the plants.

So, the total number of times they have to refill to water all the plants is 0 + 1 + 1 + 0 = 2.

Example 3:

```text
Input: plants = [5], capacityA = 10, capacityB = 8
Output: 0
Explanation:
```

- There is only one plant.
- Alice's watering can has 10 units of water, whereas Bob's can has 8 units. Since Alice has more water in her can, she waters this plant.

So, the total number of times they have to refill is 0.

__Constraints:__

- `n == plants.length`
- `1 <= n <= 10 ^ 5`
- `1 <= plants[i] <= 10 ^ 6`
- `max(plants[i]) <= capacityA, capacityB <= 10 ^ 9`

__题目描述:__

Alice 和 Bob 打算给花园里的 `n` 株植物浇水。植物排成一行，从左到右进行标记，编号从 `0` 到 `n - 1` 。其中，第 `i` 株植物的位置是 `x = i` 。

每一株植物都需要浇特定量的水。Alice 和 Bob 每人有一个水罐，最初是满的 。他们按下面描述的方式完成浇水：

- Alice 按 __从左到右__ 的顺序给植物浇水，从植物 `0` 开始。Bob 按 __从右到左__ 的顺序给植物浇水，从植物 `n - 1` 开始。他们 __同时__ 给植物浇水。
- 无论需要多少水，为每株植物浇水所需的时间都是相同的。
- 如果 Alice/Bob 水罐中的水足以 __完全__ 灌溉植物，他们 __必须__ 给植物浇水。否则，他们 __首先__（立即）重新装满罐子，然后给植物浇水。
- 如果 Alice 和 Bob 到达同一株植物，那么当前水罐中水 __更多__ 的人会给这株植物浇水。如果他俩水量相同，那么 Alice 会给这株植物浇水。

给你一个下标从 __0__ 开始的整数数组 `plants` ，数组由 `n` 个整数组成。其中， `plants[i]` 为第 `i` 株植物需要的水量。另有两个整数 `capacityA` 和 `capacityB` 分别表示 Alice 和 Bob 水罐的容量。返回两人浇灌所有植物过程中重新灌满水罐的 __次数__ 。

__示例:__

示例 1：

```text
输入：plants = [2,2,3,3], capacityA = 5, capacityB = 5
输出：1
解释：
```

- 最初，Alice 和 Bob 的水罐中各有 5 单元水。
- Alice 给植物 0 浇水，Bob 给植物 3 浇水。
- Alice 和 Bob 现在分别剩下 3 单元和 2 单元水。
- Alice 有足够的水给植物 1 ，所以她直接浇水。Bob 的水不够给植物 2 ，所以他先重新装满水，再浇水。

所以，两人浇灌所有植物过程中重新灌满水罐的次数 = 0 + 0 + 1 + 0 = 1 。

示例 2：

```text
输入：plants = [2,2,3,3], capacityA = 3, capacityB = 4
输出：2
解释：
```

- 最初，Alice 的水罐中有 3 单元水，Bob 的水罐中有 4 单元水。
- Alice 给植物 0 浇水，Bob 给植物 3 浇水。
- Alice 和 Bob 现在都只有 1 单元水，并分别需要给植物 1 和植物 2 浇水。
- 由于他们的水量均不足以浇水，所以他们重新灌满水罐再进行浇水。

所以，两人浇灌所有植物过程中重新灌满水罐的次数 = 0 + 1 + 1 + 0 = 2 。

示例 3：

```text
输入：plants = [5], capacityA = 10, capacityB = 8
输出：0
解释：
```

- 只有一株植物
- Alice 的水罐有 10 单元水，Bob 的水罐有 8 单元水。因此 Alice 的水罐中水更多，她会给这株植物浇水。

所以，两人浇灌所有植物过程中重新灌满水罐的次数 = 0 。

__提示：__

- `n == plants.length`
- `1 <= n <= 10 ^ 5`
- `1 <= plants[i] <= 10 ^ 6`
- `max(plants[i]) <= capacityA, capacityB <= 10 ^ 9`

__思路:__

```text
双指针
指针没相遇的时候尝试浇水
如果水不够，就重新灌满水罐
每次减去浇水的水量
最后用 max(a, b) 和当前植物的水量比较
如果当前植物的水量大于 max(a, b)，则需要重新灌满水罐, 多加一次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumRefill(vector<int>& plants, int capacityA, int capacityB) 
    {
        int n = plants.size(), i = 0, j = n - 1, a = capacityA, b = capacityB, result = 0;
        while (i < j) 
        {
            if (a < plants[i]) 
            {
                a = capacityA;
                ++result;
            }
            if (b < plants[j]) 
            {
                b = capacityB;
                ++result;
            }
            a -= plants[i++];
            b -= plants[j--];
        }
        return result + (i == j and max(a, b) < plants[i]);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumRefill(int[] plants, int capacityA, int capacityB) {
        int n = plants.length, i = 0, j = n - 1, a = capacityA, b = capacityB, result = 0;
        while (i < j) {
            if (a < plants[i]) {
                a = capacityA;
                ++result;
            }
            if (b < plants[j]) {
                b = capacityB;
                ++result;
            }
            a -= plants[i++];
            b -= plants[j--];
        }
        return result + (i == j && Math.max(a, b) < plants[i] ? 1 : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumRefill(self, plants: List[int], capacityA: int, capacityB: int) -> int:
        i, j, a, b, result = 0, (n := len(plants)) - 1, capacityA, capacityB, 0
        while i < j:
            if a < plants[i]:
                a = capacityA
                result += 1
            if b < plants[j]:
                b = capacityB
                result += 1
            a -= plants[i]
            b -= plants[j]
            i += 1
            j -= 1
        return result + (i == j and max(a, b) < plants[i])
```
