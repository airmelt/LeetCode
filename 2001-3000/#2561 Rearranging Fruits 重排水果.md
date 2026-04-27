# 2561 Rearranging Fruits 重排水果

__Description:__

You have two fruit baskets containing `n` fruits each. You are given two __0-indexed__ integer arrays `basket1` and `basket2` representing the cost of fruit in each basket. You want to make both baskets __equal__. To do so, you can use the following operation as many times as you want:

- Chose two indices `i` and `j`, and swap the `i_th` fruit of `basket1` with the `j_th` fruit of `basket2`.
- The cost of the swap is `min(basket1[i],basket2[j])`.

Two baskets are considered equal if sorting them according to the fruit cost makes them exactly the same baskets.

Return _the minimum cost to make both the baskets equal or_ `-1` _if impossible._

__Example:__

Example 1:

```text
Input: basket1 = [4,2,2,2], basket2 = [1,4,1,2]
Output: 1
Explanation: Swap index 1 of basket1 with index 0 of basket2, which has cost 1. Now basket1 = [4,1,2,2] and basket2 = [2,4,1,2]. Rearranging both the arrays makes them equal.
```

Example 2:

```text
Input: basket1 = [2,3,4,1], basket2 = [3,2,5,1]
Output: -1
Explanation: It can be shown that it is impossible to make both the baskets equal.
```

__Constraints:__

- `basket1.length == basket2.length`
- `1 <= basket1.length <= 10 ^ 5`
- `1 <= basket1[i],basket2[i] <= 10 ^ 9`

__题目描述:__

你有两个果篮，每个果篮中有 `n` 个水果。给你两个下标从 __0__ 开始的整数数组 `basket1` 和 `basket2` ，用以表示两个果篮中每个水果的交换成本。你想要让两个果篮相等。为此，可以根据需要多次执行下述操作:

- 选中两个下标 `i` 和 `j` ，并交换 `basket1` 中的第 `i` 个水果和 `basket2` 中的第 `j` 个水果。
- 交换的成本是 `min(basket1_i,basket2j)` 。

根据果篮中水果的成本进行排序，如果排序后结果完全相同，则认为两个果篮相等。

返回使两个果篮相等的最小交换成本，如果无法使两个果篮相等，则返回 `-1` 。

__示例:__

示例 1：

```text
输入：basket1 = [4,2,2,2], basket2 = [1,4,1,2]
输出：1
解释：交换 basket1 中下标为 1 的水果和 basket2 中下标为 0 的水果，交换的成本为 1 。此时，basket1 = [4,1,2,2] 且 basket2 = [2,4,1,2] 。重排两个数组，发现二者相等。
```

示例 2：

```text
输入：basket1 = [2,3,4,1], basket2 = [3,2,5,1]
输出：-1
解释：可以证明无法使两个果篮相等。
```

__提示：__

- `basket1.length == basket2.length`
- `1 <= basket1.length <= 10 ^ 5`
- `1 <= basket1_i,basket2_i <= 10 ^ 9`

__思路:__

```text
贪心
先统计两个篮子每种水果的数量差异
若某种水果的数量差异为奇数, 则无法使两个篮子相等, 返回 -1
否则总可以交换成功
对于每次交换, 要么直接交换取 min(basket1[i], basket2[j]) 的值
要么使用全局最小值的两倍完成交换, 即先交换最小值再换回来
所以取 min(basket1[i], basket2[j], min_value * 2) 的值
将水果排序取前一半的值, 每次交换取 min(v, min_value * 2) 的值
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minCost(vector<int>& basket1, vector<int>& basket2) 
    {
        unordered_map<int, int> c;
        int n = basket1.size(), mn = INT_MAX;
        for (int i = 0; i < n; i++) 
        {
            ++c[basket1[i]];
            --c[basket2[i]];
        }
        vector<int> a;
        for (const auto& [k, v] : c) 
        {
            if (v & 1) return -1;
            mn = min(mn, k);
            for (int i = abs(v) >> 1; i > 0; i--) a.emplace_back(k);
        }
        long long result = 0LL;
        sort(a.begin(), a.end());
        for (int i = 0, m = a.size() >> 1; i < m; i++) result += min(a[i], mn << 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minCost(int[] basket1, int[] basket2) {
        var map = new HashMap<Integer, Integer>();
        int n = basket1.length, mn = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            map.merge(basket1[i], 1, Integer::sum);
            map.merge(basket2[i], -1, Integer::sum);
        }
        var a = new ArrayList<Integer>();
        for (var e : map.entrySet()) {
            if ((e.getValue() & 1) == 1) return -1;
            mn = Math.min(mn, e.getKey());
            for (int i = Math.abs(e.getValue()) >>> 1; i > 0; i--) a.add(e.getKey());
        }
        long result = 0L;
        a.sort((x, y) -> x - y);
        for (int i = 0, m = a.size() >>> 1; i < m; i++) result += Math.min(a.get(i), mn << 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, basket1: List[int], basket2: List[int]) -> int:
        c = Counter()
        for x, y in zip(basket1, basket2):
            c[x] += 1
            c[y] -= 1
        a, mn = [], min(c)
        for k, v in c.items():
            if v & 1:
                return -1
            a.extend([k] * (abs(v) >> 1))
        return sum(min(x, mn << 1) for x in sorted(a)[:len(a) >> 1])
```
