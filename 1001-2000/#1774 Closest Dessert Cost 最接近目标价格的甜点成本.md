# 1774 Closest Dessert Cost 最接近目标价格的甜点成本

__Description:__

You would like to make dessert and are preparing to buy the ingredients. You have `n` ice cream base flavors and `m` types of toppings to choose from. You must follow these rules when making your dessert:

- There must be __exactly one__ ice cream base.
- You can add __one or more__ types of topping or have no toppings at all.
- There are __at most two__ of __each type__ of topping.

You are given three inputs:

- `baseCosts`, an integer array of length `n`, where each `baseCosts[i]` represents the price of the `i ^ th` ice cream base flavor.
- `toppingCosts`, an integer array of length `m`, where each `toppingCosts[i]` is the price of __one__ of the `i ^ th` topping.
- `target`, an integer representing your target price for dessert.

You want to make a dessert with a total cost as close to `target` as possible.

Return _the closest possible cost of the dessert to_ `target`. If there are multiple, return _the __lower__ one._

__Example:__

Example 1:

```text
Input: baseCosts = [1,7], toppingCosts = [3,4], target = 10
Output: 10
Explanation: Consider the following combination (all 0-indexed):
- Choose base 1: cost 7
- Take 1 of topping 0: cost 1 x 3 = 3
- Take 0 of topping 1: cost 0 x 4 = 0
Total: 7 + 3 + 0 = 10.
```

Example 2:

```text
Input: baseCosts = [2,3], toppingCosts = [4,5,100], target = 18
Output: 17
Explanation: Consider the following combination (all 0-indexed):
- Choose base 1: cost 3
- Take 1 of topping 0: cost 1 x 4 = 4
- Take 2 of topping 1: cost 2 x 5 = 10
- Take 0 of topping 2: cost 0 x 100 = 0
Total: 3 + 4 + 10 + 0 = 17. You cannot make a dessert with a total cost of 18.
```

Example 3:

```text
Input: baseCosts = [3,10], toppingCosts = [2,5], target = 9
Output: 8
Explanation: It is possible to make desserts with cost 8 and 10. Return 8 as it is the lower cost.
```

__Constraints:__

- `n == baseCosts.length`
- `m == toppingCosts.length`
- `1 <= n, m <= 10`
- `1 <= baseCosts[i], toppingCosts[i] <= 10 ^ 4`
- `1 <= target <= 10 ^ 4`

__题目描述:__

你打算做甜点，现在需要购买配料。目前共有 `n` 种冰激凌基料和 `m` 种配料可供选购。而制作甜点需要遵循以下几条规则:

- 必须选择 __一种__ 冰激凌基料。
- 可以添加 __一种或多种__ 配料，也可以不添加任何配料。
- 每种类型的配料 __最多两份__ 。

给你以下三个输入：

- `baseCosts` ，一个长度为 `n` 的整数数组，其中每个 `baseCosts[i]` 表示第 `i` 种冰激凌基料的价格。
- `toppingCosts`，一个长度为 `m` 的整数数组，其中每个 `toppingCosts[i]` 表示 __一份__ 第 `i` 种冰激凌配料的价格。
- `target` ，一个整数，表示你制作甜点的目标价格。

你希望自己做的甜点总成本尽可能接近目标价格 `target` 。

返回最接近 `target` 的甜点成本。如果有多种方案，返回 __成本相对较低__ 的一种。

__示例:__

示例 1：

```text
输入：baseCosts = [1,7], toppingCosts = [3,4], target = 10
输出：10
解释：考虑下面的方案组合（所有下标均从 0 开始）：
- 选择 1 号基料：成本 7
- 选择 1 份 0 号配料：成本 1 x 3 = 3
- 选择 0 份 1 号配料：成本 0 x 4 = 0
总成本：7 + 3 + 0 = 10 。
```

示例 2：

```text
输入：baseCosts = [2,3], toppingCosts = [4,5,100], target = 18
输出：17
解释：考虑下面的方案组合（所有下标均从 0 开始）：
- 选择 1 号基料：成本 3
- 选择 1 份 0 号配料：成本 1 x 4 = 4
- 选择 2 份 1 号配料：成本 2 x 5 = 10
- 选择 0 份 2 号配料：成本 0 x 100 = 0
总成本：3 + 4 + 10 + 0 = 17 。不存在总成本为 18 的甜点制作方案。
```

示例 3：

```text
输入：baseCosts = [3,10], toppingCosts = [2,5], target = 9
输出：8
解释：可以制作总成本为 8 和 10 的甜点。返回 8 ，因为这是成本更低的方案。
```

示例 4：

```text
输入：baseCosts = [10], toppingCosts = [1], target = 1
输出：10
解释：注意，你可以选择不添加任何配料，但你必须选择一种基料。
```

__提示：__

- `n == baseCosts.length`
- `m == toppingCosts.length`
- `1 <= n, m <= 10`
- `1 <= baseCosts[i], toppingCosts[i] <= 10 ^ 4`
- `1 <= target <= 10 ^ 4`

__思路:__

```text
DFS
由于数据量不大可以直接暴力搜索
对每个 baseCosts 进行搜索
如果能取更小的就更新结果
如果价格超过 target 或者搜索结束就剪枝
每次最多添加 2 个配料
时间复杂度为 O(N * 3 ^ M), 空间复杂度为 O(M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int closestCost(vector<int>& baseCosts, vector<int>& toppingCosts, int target) 
    {
        this -> target = target;
        this -> toppingCosts = toppingCosts;
        this -> n = toppingCosts.size();
        this -> result = INT_MAX;
        for (const auto& base : baseCosts) dfs(0, base);
        return result;
    }
private:
    int result, target, n;
    vector<int> toppingCosts;

    void dfs(int start, int total) 
    {
        int cur = abs(result - target) - abs(total - target);
        if (cur > 0 || (cur == 0 && total < result)) result = total;
        if (total >= target || start == n) return;
        for (int i = 0; i < 3; i++) dfs(start + 1, total + toppingCosts[start] * i);
    }
};
```

__Java__:

```Java
class Solution {
    private int result = Integer.MAX_VALUE;
    private int target;
    private int n;
    private int[] toppingCosts;

    public int closestCost(int[] baseCosts, int[] toppingCosts, int target) {
        this.target = target;
        this.toppingCosts = toppingCosts;
        this.n = toppingCosts.length;
        for (int base : baseCosts) dfs(0, base);
        return result;
    }

    private void dfs(int start, int total) {
        int cur = Math.abs(result - target) - Math.abs(total - target);
        if (cur > 0 || (cur == 0 && total < result)) result = total;
        if (total >= target || start == n) return;
        for (int i = 0; i < 3; i++) dfs(start + 1, total + toppingCosts[start] * i);
    }
}
```

__Python__:

```Python
class Solution:
    def closestCost(self, baseCosts: List[int], toppingCosts: List[int], target: int) -> int:
        result, n = float('inf'), len(toppingCosts)

        def dfs(start: int, total: int) -> NoReturn:
            nonlocal result
            if (cur := abs(result - target) - abs(total - target)) > 0 or not cur and total < result:
                result = total
            if total >= target or start == n:
                return 
            for i in range(3):
                dfs(start + 1, total + toppingCosts[start] * i)
        for base in baseCosts:
            dfs(0, base)
        return result
```
