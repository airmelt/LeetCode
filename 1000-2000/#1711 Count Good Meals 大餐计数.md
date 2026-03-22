# 1711 Count Good Meals 大餐计数

__Description:__

A good meal is a meal that contains exactly two different food items with a sum of deliciousness equal to a power of two.

You can pick any two different foods to make a good meal.

Given an array of integers `deliciousness` where `deliciousness[i]` is the deliciousness of the `i ^ ​​​​​​th​​​​`​​​​ item of food, return _the number of different __good meals__ you can make from this list modulo_ `10 ^ 9 + 7`.

Note that items with different indices are considered different even if they have the same deliciousness value.

__Example:__

Example 1:

```text
Input: deliciousness = [1,3,5,7,9]
Output: 4
Explanation: The good meals are (1,3), (1,7), (3,5) and, (7,9).
Their respective sums are 4, 8, 8, and 16, all of which are powers of 2.
```

Example 2:

```text
Input: deliciousness = [1,1,1,3,3,3,7]
Output: 15
Explanation: The good meals are (1,1) with 3 ways, (1,3) with 9 ways, and (1,7) with 3 ways.
```

__Constraints:__

- `1 <= deliciousness.length <= 10 ^ 5`
- `0 <= deliciousness[i] <= 2 ^ 20`

__题目描述:__

大餐 是指 恰好包含两道不同餐品 的一餐，其美味程度之和等于 2 的幂。

你可以搭配 任意 两道餐品做一顿大餐。

给你一个整数数组 `deliciousness` ，其中 `deliciousness[i]` 是第 `i​​​​​​​​​​`​​​​ 道餐品的美味程度，返回你可以用数组中的餐品做出的不同 __大餐__ 的数量。结果需要对 `10 ^ 9 + 7` 取余。

注意，只要餐品下标不同，就可以认为是不同的餐品，即便它们的美味程度相同。

__示例:__

示例 1：

```text
输入：deliciousness = [1,3,5,7,9]
输出：4
解释：大餐的美味程度组合为 (1,3) 、(1,7) 、(3,5) 和 (7,9) 。
它们各自的美味程度之和分别为 4 、8 、8 和 16 ，都是 2 的幂。
```

示例 2：

```text
输入：deliciousness = [1,1,1,3,3,3,7]
输出：15
解释：大餐的美味程度组合为 3 种 (1,1) ，9 种 (1,3) ，和 3 种 (1,7) 。
```

__提示：__

- `1 <= deliciousness.length <= 10 ^ 5`
- `0 <= deliciousness[i] <= 2 ^ 20`

__思路:__

```text
哈希表
利用哈希表记录数组中每个元素的出现次数
遍历排序后哈希表中的每个元素
如果当前元素为 0, 则跳过
如果当前元素大于等于 target, 则将 target 左移一位
将当前元素的出现次数与 target - 当前元素的出现次数相乘, 并将结果加到结果中
如果当前元素等于 target, 则加上 (当前元素的出现次数 * (当前元素的出现次数 - 1) / 2), 这一部分是用的组合数学的知识
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPairs(vector<int>& deliciousness) 
    {
        long long MOD = 1e9 + 7, target = 1, result = 0;
        map<int, long long> m;
        for (const auto& d : deliciousness) ++m[d];
        for (const auto& [k, v] : m)
        {
            if (!k) continue;
            while (target < k) target <<= 1;
            result += (v * m[target - k] + (k == target) * (v * (v - 1) >> 1)) % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPairs(int[] deliciousness) {
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0, mod = 1000000007;
        for (int d : deliciousness) {
            for (int i = 1; i < (1 << 22); i <<= 1) result = (result + map.getOrDefault(i - d, 0)) % mod;
            map.put(d, map.getOrDefault(d, 0) + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPairs(self, deliciousness: List[int]) -> int:
        MOD, count, target, result = 10 ** 9 + 7, Counter(deliciousness), 1, 0
        for i in sorted(count):
            if not i:
                continue
            while target < i:
                target <<= 1
            result += count[i] * count[target - i] + (i == target) * (count[i] * (count[i] - 1) >> 1)
        return result % MOD
```
